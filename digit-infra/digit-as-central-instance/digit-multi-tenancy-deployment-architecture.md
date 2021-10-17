# DIGIT - Multi-tenancy Deployment Architecture

As per the primitives and key principles of the multi-tenancy the DIGIT central instance deployment is architected. This includes the standardizing and categorizing the Git repos, isolating the deployment configs, access control, roles, ci-cd system and pipeline. 

## **Git Repos:** 

In alignment with the various service layers of the DIGIT architecture, all the services from various repos will be merged and managed as a monorepo. This ways it provides the tenants to clone single repo and also take advantage of periodic releases by pulling the latest tags, code.

### How is Monorepo Tools Better for multi-tenancy? <a href="09e5" id="09e5"></a>

* Merge DIGIT existing repositories into 
* Merge them to subdirectories
* Keep their git history
* Keep hash of all commits
* In split repo keep merge commits with per-package files only
* And also do the split while keeping history, hash of commits, etc.

Tenants can clone the repo and do sparse checkout only those services or sub-directories to write configurations, extensions, plugins, etc. 

![](<../../.gitbook/assets/image (40).png>)

**Key Consideration: **Do not hardcode modifications when we have more tenants. Instead, make the engine configurable so that tenant-specific plugins can be loaded. When we want to modify the behavior, refactor the core engine to support a plugin, for example by introducing a new interface. Then provide an implementation for that interface as tenant-specific code. When multiple tenant need a functionality we can move that code into the core, but possibly disable it for tenants that don't need it.

## Deployment

As part a startup kit, all the various capabilities like Deployment Configs, Infra-as-code, CI-as-code, Test automation, are grouped into a another Git repo called "DIGIT-Devops", this will be available for the tenants to leverage to setup their CICD using the same way how digit does, as we release new enhancements they can take the full advantage of it by taking the pull. 

Recognize that having an individual deployment for a tenant is not the same as running an individual project for that tenant. We can deploy with different configurations from various isolated repos with the designated access control.

Deployment Config Repo will be as follows and every tenant can Clone it from the release tags to add their Deployment configs, wherever necessary.

![](<../../.gitbook/assets/image (42).png>)



## Multi-Tenant - Deployment architecture

![](<../../.gitbook/assets/image (32).png>)

![](<../../.gitbook/assets/image (36).png>)

### Categorize Namespaces

DIGIT central instance follows a namespace based hard multi-tenancy approach in the context of Kubernetes is to categorize namespaces into groups. Four such namespace groups from the above example is: \<kuber-systems>, \<digit>, \<tenant-A> and \<tenant-B>.

We can restrict the visibility of the services running on various namespaces with an efficient NetworkPolicies like the below.

```
kind: NetworkPolicy
apiVersion: networking.k8s.io/v1
metadata:
  namespace: digit
  name: allow-from-tenant-namespaces //Eg: can be as required 
spec:
  podSelector:
    matchLabels:
  ingress:
  - from:
    - podSelector: {}
```

1. **System namespaces: **\<kuber-systems>, \<default> Exclusively for system pods
2. **DIGIT namespaces:**  \<backbone>, \<digit>These namespaces should run backbone services and DIGIT platform services that need to be accessed by services in other namespaces.
3. **Tenant Namespaces:** Tenant namespaces should be spun up to run custom/extension services that do not need to be accessed from other namespaces in the cluster. But the services in this namespace can have access to DIGIT services in a \<digit> namespace.

#### Map Kubernetes Namespaces to Tenants

Once categorized a best practice is to create separate namespaces for individual tenants; for example creating a separate namespace for individual DevOps teams or individual namespaces housing the dev and staging environments.

Tenants can also own more than one namespaces. Cluster admins do need to ensure however that only the members of that specific team have access to the namespace and only they can perform operations on Kubernetes objects in it.

### Create Cluster Personas

Another best practice is to create a hierarchy of cluster personas scoped to varying levels of permissions and the operations they can perform. The Kubernetes [multi-tenancy SIG](https://github.com/kubernetes-sigs/multi-tenancy) outlines four such personas:

| Persona         | Description                                                                                                                                                                                                                                                                                                                                                  |
| --------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| _Cluster-admin_ | This persona has full read/write privileges for all resources in the cluster including resources owned by various Tenants of the cluster.                                                                                                                                                                                                                    |
| _Cluster-view_  | This persona has read privileges for all resources in the cluster including reasources owned by various Tenants.                                                                                                                                                                                                                                             |
| _Tenant-admin_  | This persona has privileges to create a new tenant, read/write resources scoped to that Tenant and update or delete that Tenant. This persona does not have any privileges for accessing resources that are either cluster-scoped or scoped to namespaces that are not associated with the Tenant object for which this persona has Tenant-admin privileges. |
| _Tenant-user_   | This persona has read/write privileges for all resources scoped within a specific Tenant (that is resources that are scoped within namespaces that are owned by a specific Tenant)                                                                                                                                                                           |

### Enable RBAC

Enabling RBAC is another best practice in the context of Kubernetes multi-tenancy. Once enabled cluster administrators can create the previously mentioned personas using the four API objects provided by [RBAC](https://kubernetes.io/docs/reference/access-authn-authz/rbac/): Roles, ClusterRoles, RoleBindings and ClusterRoleBindings. Disabling ABAC (Attribute Based Access Control) and static file based access control is also recommended.

#### Isolate Tenant Namespaces using Network Policy

Network policies allow cluster admins to govern how groups of pods can communicate. A best practice is for admins to isolate tenant namespaces using the network policy resource. A sample policy is provided [here](https://raw.githubusercontent.com/kubernetes/website/master/content/en/examples/policy/restricted-psp.yaml). When using a networking plugin cluster admins should also ensure that it supports Kubernetes network policy.

#### Enable Built-in Admission Controllers

[Admission controllers](https://kubernetes.io/blog/2019/03/21/a-guide-to-kubernetes-admission-controllers/) allow cluster admins to govern and control how Kubernetes clusters are used. Admission controllers can both change incoming API requests or deny them.

A best practice for cluster administrators operating multi-tenant Kubernetes clusters is to enable the PodSecurityPolicy admission controller. PodSecurityPolicy kicks in on pod creation and modification to ensure that the new pod specification meets the set of conditions defined. Here is a [sample](https://github.com/kubernetes-sigs/multi-tenancy/blob/master/docs/profiles/profile-soft-multitenancy-s1.md#kubernetes-pod-admission-and-security-policy-definition-for-profile-s1) PodSecurityPolicy that cluster admins can implement.

Besides PodSecurityPolicy cluster admins should also enable the following admission controllers: 

* AlwaysPullImages
* NodeRestriction
* ServiceAccount
* ResourceQuota
* LimitRanger

#### Limit Tenant’s use of Shared Resources

To ensure resources are not wasted and avoid disproportionate resource usage across tenants, a best practice is to implement Kubernetes namespace resource quotas. [Resource quotas](https://kubernetes.io/docs/concepts/policy/resource-quotas/) allow for control of total resource usage (CPU, memory, storage) for the entire namespace scoped to a single tenant. Cluster admins should also ensure that tenants cannot create, update, patch or delete resource quotas.

#### Limit Tenant’s Access to non-namespaced Resources

Another best practice in the context of Kubernetes multi-tenancy is to ensure that tenants do not have access to non-namespaced resources. Non-namespaced resources are ones that do not belong to any specific node or namespace but to the cluster as a whole. Cluster admins should ensure that tenants cannot view, edit, create or delete cluster scoped resources such as Node, ClusterRole, ClusterRoleBinding. 

## Autoscaling

We can scale the infra based on the dynamic workloads using the custom metrics target a marker of pod usage like CPU usage, such as network traffic, memory, or a value relating to a service running inside a pod itself or an external metrics measure values that do not correlate to a pod. For example, an external metric could track the number of Incoming traffic TPS.

1. **Horizontal Pod Autoscaling (HPA)**
2. **Vertical Pod Autoscaling (VPA)**
3. **Cluster Autoscaler (nodes)**

****

#### To recap here are the recommended best practices for Multi-tenant cluster:

* Limit Tenant’s use of Shared Resources
* Enable Built-in Admission Controllers
* Isolate Tenant Namespaces using Network Policy
* Enable RBAC
* Create Cluster Personas
* Map Kubernetes Namespaces to Tenants
* Categorize Namespaces
* Limit Tenant’s Access to non-namespaced Resources
* Limit Tenant’s Access to Resources from other Tenants
* Limit Tenant’s Access to Multi-tenancy Resources
* Prevent use of HostPath Volumes
* Run Multi-tenancy e-2-e Validation Test









