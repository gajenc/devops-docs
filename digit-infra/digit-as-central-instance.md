---
description: A multi tenanted instance approach
---

# DIGIT as Central Instance

Multi tenancy is a key topic when it comes to host DIGIT as a central Instance. We experienced this increased interest first hand at CDG. 

Multi-tenancy is a hard nut to crack in the context of SDC, NIC and Commercial Cloud Infra. This is supported by the results of a survey where 74% of respondents indicated the use of separate instance for individual states. One reason for this are the security and resource isolation challenges that come with multi-tenant central Instance.

In this article we will briefly review the concepts of tenants and multi-tenancy, identify the challenges that have to be overcome and outline best practices for Isolated development, customization, Deployments, DevOps and infra/operations admins operating multi-tenant Kubernetes clusters.

### What is a Tenant?

The tenant defined as representing a groups of users from either a distributed teams, states, applications, departments or projects. In any of these case users that will have access to only a subset of DIGIT services hosted central instance, and the subset of the services might vary from a usergroup-to-usergroup depending upon the opted DIGIT municipal services. 

This phenomenon of subsetting the services to a tenant also impacts the infra resources \(compute, storage, networking, control plane and API resources\) as well as resource limits and quotas for the use of those resources. Resource limits and quotas lay out tenant boundaries. These boundaries extend to the control plane allowing for grouping of the resources owned by the tenant, limited access or visibility to resources outside of the control plane domain and tenant authentication.

### What is multi-tenancy in Kubernetes?

Multi-tenant Kubernetes clusters are one’s that share infra resources of the central instance among tenants. Based on the definition provided earlier, tenants can be anything from groups of users to distributed teams, states, applications, departments or projects. Examples include multiple distributed DevOps teams deploying applications to the same cluster, staging and production environments residing on the same cluster, Kubernetes clusters being shared among multiple end-users or customers and multiple applications or workloads sharing a cluster’s resources.

There are two multi-tenancy models in Kubernetes: Soft and Hard multi-tenancy.

#### Soft Multi-tenancy

[Soft multi-tenancy](https://static.sched.com/hosted_files/kccnceu19/ed/2019%20Kubecon%20EU%20-%20Multitenancy%20WG%20Intro.pdf) trusts tenants to be good actors and assumes them to be non-malicious. Soft multi-tenancy is focused on minimising accidents and managing the fallout if they do.

#### Hard Multi-tenancy

[Hard multi-tenancy](https://static.sched.com/hosted_files/kccnceu19/ed/2019%20Kubecon%20EU%20-%20Multitenancy%20WG%20Intro.pdf) assumes tenants to be malicious and therefore advocates zero trust between them. Tenant resources are isolated and access to other tenant’s resources is not allowed. Clusters are configured in a way that isolate tenant resources and prevent access to other tenant’s resources.

### Kubernetes Multi-tenancy Best Practices

Let’s move on now to multi-tenancy best practices in the context of Kubernetes. In the section below we will outline best practices for DevOps and cluster administrators operating multi-tenant Kubernetes clusters.

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

