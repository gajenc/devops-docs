---
description: A multi tenanted DIGIT instance
---

# DIGIT as Central Instance

DIGIT being a distributed system, hosting the DIGIT services as a Central SaaS Instance is surely an option and scaling the all/specific services/workloads depending on each state's needs/requirements. To achieve the same **Multi tenancy** is a key topic. We experienced this increased interest first hand at CDG. This interest is further curated by the results of the most respondents who indicated the use of a central instance simplifies the infra/operation for individual states or Small states with a deep isolation of access, services and resources. The emphasis on a central instance are the security, privacy and independant customization/configs which is an obvious challenge that DIGIT as a platform to address.

Multi-tenancy is a hard nut to crack in the context of Infra depending on whether it is NIC or Commercial Cloud Infra. Comparatively, comercial clouds provide number of services that can simplify the process of isolating the infra resources using VPCs, Security groups, Encryptions, RBACs, operations, monitoring etc.

In this article we will briefly review the concepts of tenants and multi-tenancy in the context of DIGIT Central Instance, identify the challenges that have to be overcome and outline best practices for Isolated development, customization, Deployments, DevOps and infra/operations admins operating multi-tenant Kubernetes clusters.

![](../.gitbook/assets/image%20%2825%29.png)

### Who are Tenants in DIGIT Central Instance?

The tenant defined as representing a groups of users who could be from either of a state, ULBs, applications, distributed teams, partners/system integrators, departments or projects. In any of these cases users that will have access to only a subset/intended DIGIT services on a centrally hosted instance, and the services might vary from a usergroup-to-usergroup depending upon the opted DIGIT municipal services. 

The phenomenon of subsetting/Isolating the services from the central instance to a specific tenant also impacts the infra resources \(compute, storage, networking, control plane and API resources\) as well as resource limits and quotas for the use of those resources. Resource limits and quotas lay out tenant boundaries. These boundaries extend to the control plane allowing for grouping of the resources owned by the tenant, limited access or visibility to resources outside of the control plane domain and tenant authentication.

![](../.gitbook/assets/image%20%2816%29.png)

### What is multi-tenancy in DIGIT Infra?

Multi-tenancy in DIGIT is sharing the DIGIT central infra resources among multiple individual tenants. Based on the definition provided earlier, tenants can be anything from groups of users. Examples include the core DIGIT platform services maintained centrally and multiple distributed state teams deploying customizations/configs to the DIGIT Central Instances, like Dev, staging and production environments residing on the same Infra and workloads are shared leveraging the central instance's infra resources.

#### Hard Multi-tenancy

Hard multi-tenancy assumes tenants to be malicious and therefore advocates zero trust between them. Tenant resources are isolated and access to other tenant’s resources is not allowed. Clusters are configured in a way that isolate tenant resources and prevent access to other tenant’s resources.

This concept would need to be applied to Kubernetes namespaces as well. Rather than controlling resources like memory consumption and CPU, it would apply to nodes. The tenant within a namespace would only be able to access certain nodes designated to it. All the namespace services would be isolated at the machine level as well. No services from different tenants would run on the same machine. This could always be a setting in the future but the default should be that nodes are not shared.

### Multi-tenancy Best Practices and Key Considerations

![](../.gitbook/assets/image%20%2814%29.png)

DIGIT Infra is abstracted to kubernetes, Let’s move on now to multi-tenancy practices in the context of Kubernetes. In the section below we will outline best practices for DevOps \(Source Code, Deployment, Configs and cluster administrators\) operating multi-tenant Kubernetes clusters.

![](../.gitbook/assets/image%20%2829%29.png)

![](../.gitbook/assets/image%20%2823%29.png)

![](../.gitbook/assets/image%20%2830%29.png)

#### 

![](../.gitbook/assets/image%20%2813%29.png)

#### 

![](../.gitbook/assets/image%20%2828%29.png)

![](../.gitbook/assets/image%20%2819%29.png)



![](../.gitbook/assets/image%20%2817%29.png)

![](../.gitbook/assets/image%20%2820%29.png)







![](../.gitbook/assets/image%20%2824%29.png)

![](../.gitbook/assets/image%20%2821%29.png)

![](../.gitbook/assets/image%20%2827%29.png)

![](../.gitbook/assets/image%20%2826%29.png)

![](../.gitbook/assets/image%20%2812%29.png)

![](../.gitbook/assets/image%20%2818%29.png)

#### Categorize Namespaces

A starter best practice in the context of Kubernetes multi-tenancy is to categorize namespaces into groups. Three such namespace groups are [recommended](https://github.com/kubernetes-sigs/multi-tenancy/blob/master/docs/profiles/profile-soft-multitenancy-s1.md#kubernetes-pod-admission-and-security-policy-definition-for-profile-s1):

**System namespaces:** Exclusively for system pods

**Service namespaces:** These namespaces should run services or applications that need to be accessed by services in other namespaces.

**Tenant Namespaces:** Tenant namespaces should be spun up to run applications that do not need to be accessed from other namespaces in the cluster.

#### Map Kubernetes Namespaces to Tenants

Once categorized a best practice is to create separate namespaces for individual tenants; for example creating a separate namespace for individual DevOps teams or individual namespaces housing the dev and staging environments.

Tenants can also own more than one namespaces. Cluster admins do need to ensure however that only the members of that specific team have access to the namespace and only they can perform operations on Kubernetes objects in it.

#### Create Cluster Personas

Another best practice is to create a hierarchy of cluster personas scoped to varying levels of permissions and the operations they can perform. The Kubernetes [multi-tenancy SIG](https://github.com/kubernetes-sigs/multi-tenancy) outlines four such personas:

**Cluster admin:** Full read/write privileges for all resources in the cluster including those owned by tenants

**Cluster view:** Read privileges for all resources in the cluster including those owned by tenants

**Tenant admin:** Permissions to create new tenants, read/write resources scoped to that tenant, update or delete created tenants, no privileges to read/write resources scoped to other tenants/namespaces

**Tenant user:** Read/write privileges for all resources scoped to that tenant

Teams can pick and choose between these personas based on the size and requirements their application stack, team composition as well as other organisational environments.

#### Enable RBAC

Enabling RBAC is another best practice in the context of Kubernetes multi-tenancy. Once enabled cluster administrators can create the previously mentioned personas using the four API objects provided by [RBAC](https://kubernetes.io/docs/reference/access-authn-authz/rbac/): Roles, ClusterRoles, RoleBindings and ClusterRoleBindings. Disabling ABAC \(Attribute Based Access Control\) and static file based access control is also recommended.

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

To ensure resources are not wasted and avoid disproportionate resource usage across tenants, a best practice is to implement Kubernetes namespace resource quotas. [Resource quotas](https://kubernetes.io/docs/concepts/policy/resource-quotas/) allow for control of total resource usage \(CPU, memory, storage\) for the entire namespace scoped to a single tenant. Cluster admins should also ensure that tenants cannot create, update, patch or delete resource quotas.

#### Limit Tenant’s Access to non-namespaced Resources

Another best practice in the context of Kubernetes multi-tenancy is to ensure that tenants do not have access to non-namespaced resources. Non-namespaced resources are ones that do not belong to any specific node or namespace but to the cluster as a whole. Cluster admins should ensure that tenants cannot view, edit, create or delete cluster scoped resources such as Node, ClusterRole, ClusterRoleBinding. 

To see a list of all non-namespaced resources:

```text
kubectl --kubeconfig cluster-admin api-resources --namespaced=false
```

To see whether tenants can perform operations on resources:

```text
kubectl --kubeconfig “tenant_name” auth can-i “verb” “resource”
```

#### Limit Tenant’s Access to Resources from other Tenants

Continuing from the previous best practice cluster admins should also ensure that tenants cannot access namespaced resources belonging to other tenants. Namespaces resources are ones that are scoped to a specific namespace. As such cluster admins should ensure that tenants from other namespaces cannot view, edit, create or delete namespaces resources belonging to other tenants. 

To see a list of all namespaced resources belonging to a tenant:

```text
kubectl --kubeconfig “tenant_name” api-resources --namespaced=true
```

To see whether tenants can perform operations on namespaced resources belonging to other tenants:

```text
kubectl --kubeconfig “tenant_name” -n “namespace_name” “verb” “resource”
```

#### Limit Tenant’s Access to Multi-tenancy Resources

There is a subset of namespaced resources that should not be accessible to tenants of that namespace. These include the default network policy, namespace resource quotas and role bindings. Any changes to these resources can impact other tenants and their ability to consume or access resources. A best practice therefore is for cluster admins to ensure that this type of resource cannot be modified by tenants. 

To view list of resources managed by cluster admin:

```text
kubectl --kubeconfig=cluster-admin -n “namespace_name” get all -l =
```

To verify that the resource cannot be modified by the namespace tenant \(this requires labelling resources managed by the cluster admin\):

```text
kubectl --dry-run=true --kubeconfig=”tenant_name” -n “namespace_name” annotate key1=value1
```

#### Prevent use of HostPath Volumes

Tenants can use host volumes or directories to [access shared data](https://github.com/kubernetes-sigs/multi-tenancy/tree/master/benchmarks/e2e/tests/block_bind_mounts) or escalate privileges. A best practice therefore is for cluster admins to prevent tenants from creating pods with volumes of type HostPath. Cluster admins can do this natively using Kubernetes PodSecurityPolicy. [Here](https://gist.github.com/abhisek/b9acfdc0505905ffc4240841195326ee) is one example of a PodSecurityPolicy that prevents mounting HostPath.

#### Run Multi-tenancy e-2-e Validation Test 

The Kubernetes multi-tenancy SIG provides an e-2-e test tool that can be used to validate multi-tenant Kubernetes clusters. The e-2-e tool validates clusters based on a set of recommended multi-tenancy benchmarks. 

To run the tool do the following:

```text
git clone https://github.com/kubernetes-sigs/multi-tenancy.git
```

```text
cd multi-tenancy/benchmarks
```

Edit the config file \(config.yaml\) with your cluster configuration.

Run tests:

```text
go test ./e2e/tests
```

Or with path to config file

```text
go test ./e2e/tests -config 
```

#### To recap here are the recommended best practices for Multi-tenant Kubernetes clusters:

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

