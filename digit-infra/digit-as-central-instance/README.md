---
description: A multi tenanted DIGIT instance
---

# Multi Tenancy - Central Instance

DIGIT being a distributed system, hosting the DIGIT services as a Central SaaS Instance is surely an option and scaling the all/specific services/workloads depending on each state's needs/requirements. To achieve the same, **Multi tenancy** is a key topic. We experienced this increased interest first hand at CDG. This interest is further curated by the results of the most respondents who indicated the use of a central instance simplifies the infra/operation for individual states or Small states with a deep isolation of access, services and resources. The emphasis on a central instance are the security, privacy and independant customization/configs which is an obvious challenge that DIGIT as a platform to address.

Multi-tenancy is a hard nut to crack in the context of Infra depending on whether it is NIC or Commercial Cloud Infra. Comparatively, comercial clouds provide number of services that can simplify the process of isolating the infra resources using VPCs, Security groups, Encryptions, RBACs, operations, monitoring etc.

In this article we will briefly review the concepts of tenants and multi-tenancy in the context of DIGIT Central Instance, identify the challenges that have to be overcome and outline best practices for Isolated development, customization, Deployments, DevOps and infra/operations admins operating multi-tenant Kubernetes clusters. Before we continue further it is important that we know difference between multiple-instance and Multi-tenant architecture, the below picture depicts the high level details on how it differs, however we'll be focusing only on the multi-tenant architecture throughout this page.

![](<../../.gitbook/assets/image (52).png>)

### Who are Tenants in DIGIT Central Instance?

The tenant defined as representing a groups of users who could be from either of a state, ULBs, applications, distributed teams, partners/system integrators, departments or projects. In any of these cases users that will have access to only a subset/intended DIGIT services on a centrally hosted instance, and the services might vary from a usergroup-to-usergroup depending upon the opted DIGIT municipal services. 

The phenomenon of subsetting/Isolating the services from the central instance to a specific tenant also impacts the infra resources (compute, storage, networking, control plane and API resources) as well as resource limits and quotas for the use of those resources. Resource limits and quotas lay out tenant boundaries. These boundaries extend to the control plane allowing for grouping of the resources owned by the tenant, limited access or visibility to resources outside of the control plane domain and tenant authentication.

Providing isolation and fail resource sharing between multiple users and their workloads within a cluster is an important aspect in DIGIT Single Instance as a multi-tenancy.

### What is multi-tenancy in DIGIT Infra?

Multi-tenancy in DIGIT is sharing the DIGIT central infra resources among multiple individual tenants. Based on the definition provided earlier, tenants can be anything from groups of users. Examples include the core DIGIT platform services maintained centrally and multiple distributed state teams deploying customizations/configs to the DIGIT Central Instances, like Dev, staging and production environments residing on the same Infra and workloads are shared leveraging the central instance's infra resources.

#### Hard Multi-tenancy

Hard multi-tenancy assumes tenants to be malicious and therefore advocates zero trust between them. Tenant resources are isolated and access to other tenant’s resources is not allowed. Clusters are configured in a way that isolate tenant resources and prevent access to other tenant’s resources.

This concept would need to be applied to Kubernetes namespaces as well. Rather than controlling resources like memory consumption and CPU, it would apply to nodes. The tenant within a namespace would only be able to access certain nodes designated to it. All the namespace services would be isolated at the machine level as well. No services from different tenants would run on the same machine. This could always be a setting in the future but the default should be that nodes are not shared.

DIGIT Infra is essentially means kubernetes, Let’s look at multi-tenancy practices and the key considerations in the context of Kubernetes. Some of the best practices for DevOps (Source Code, Deployment, Configs and cluster administrators) operating multi-tenant Kubernetes clusters.

### Multi-tenancy Best Practices and Key Considerations

![](<../../.gitbook/assets/image (12).png>)

![](<../../.gitbook/assets/image (16).png>)



### Key Takeaways

**Use case:**

* How Trusted are the tenant users and workloads?
* What degree and kinds of Isolation is needed?

**Solution: Namespace-centric multi-tenancy**

* Utilize Policy objects for scheduling access control.
* Decide the personas and map them to RBAC Cluster roles.
* Automate policies across clusters.

### Namespace per tenant

Namespaces provide logical isolation between tenants on cluster. Kubernetes policies are namespace-scoped.

* Logical isolation between tenants
* Policies for API access restrictions & resource uages constraints

**Pros:**

* Tenants can reuse extensions/controllers/CRDs
* Shared control plane (=shared ops, shared security/auditing...)

####

![](<../../.gitbook/assets/image (20).png>)

### Kubernetes RBAC - Auth related.

To achieve the multi-tenancy RBAC is an important aspect to isolate the access control, which **users/groups/service accounts** \
can do **which operations** \
on **which API resources** \
in **which** **namespaces**.

![](<../../.gitbook/assets/image (21).png>)

**These are mostly for:**

* Giving access to pods calling the Kubernetes API(with Kubernetes Service accounts)
* Giving fine-grained access to people/groups calling kubernetes API

**The key concepts to achieve the above:**

|  Kubernetes RBAC Objects | Description                              |
| ------------------------ | ---------------------------------------- |
| ClusterRole              | A preset of capabilities, cluster-wide   |
| Role                     | ClusterRole, but namespace-scoped        |
| ClusterRoleBinding       | Give permission of a ClusterRole to      |
| RoleBinding              | ClusterRoleBinding, but namespace-scoped |

```
---
apiVersion: v1
kind: ServiceAccount
metadata:
  name: digit-admin-user
  namespace: digit

---
kind: Role
apiVersion: rbac.authorization.k8s.io/v1beta1
metadata:
  name: digit-namespace-user-full-access
  namespace: digit
rules:
- apiGroups: ["", "extensions", "apps"]
  resources: ["*"] //we can restrict to resources here
  verbs: ["*"] //we can restrict operations here
- apiGroups: ["batch"]
  resources:
  - pods
  - deployments
  verbs: ["*"]

---
kind: RoleBinding
apiVersion: rbac.authorization.k8s.io/v1beta1
metadata:
  name: digit-namespace-user-view
  namespace: digit
subjects:
- kind: ServiceAccount
  name: digit-namespace-user
  namespace: digit
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: Role
  name: digit-namespace-user-full-access
  
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
metadata:
  name: monitoring-endpoints
  labels:
    rbac.example.com/aggregate-to-monitoring: "true"
# When you create the "monitoring-endpoints" ClusterRole,
# the rules below will be added to the "monitoring" ClusterRole.
rules:
- apiGroups: [""]
  resources: ["services", "endpoints", "pods"]
  verbs: ["get", "list", "watch"]

---
kind: ClusterRoleBinding
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  name: cluster-admin
subjects:
- kind: Group
  name: digit-cluster-admin # Name is case sensitive
  apiGroup: rbac.authorization.k8s.io
roleRef:
  kind: ClusterRole
  name: monitoring-endpoints
  apiGroup: rbac.authorization.k8s.io
```

### **Network Policy**

In a multi-tenancy cluster, it is important to isolate the access to various object which can be controlled via kubernetes network policies that allows to control the traffic to access specific pods from specific pods. For example: we can allow the tenants from a digit namespace matching the specific criteria of the tenant namespace, they can be authorized to gain the access either or both incoming and outgoing traffic. In case of fine-grained access control we can restrict it to the specific ipBlocks as well. 

```
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: allow-digit-services-to-frontend
  namespace: digit
spec:
  podSelector:
    matchLabels:
      role: digit-service
  policyTypes:
  - Ingress
  - Egress
  ingress:
  - from:
    - ipBlock:
        cidr: 172.17.0.0/16
        except:
        - 172.17.1.0/24
    - namespaceSelector:
        matchLabels:
          project: central-instance
    - podSelector:
        matchLabels:
          role: tenant-a-services
    ports:
    - protocol: TCP
      port: 6379
  egress:
  - to:
    - ipBlock:
        cidr: 10.0.0.0/24
    ports:
    - protocol: TCP
      port: 5978
```

###  Resource Isolation/Quota/Priority:

As the primitives, resource isolation and setting quota is very very important on a multi-tenancy cluster. It helps in efficient utilization of the resources and also allows to optimize the resources for the workloads.  

```
apiVersion: v1
kind: List
items:
- apiVersion: v1
  kind: ResourceQuota
  metadata:
    name: core-services-pods-high
  spec:
    hard:
      cpu: "1000"
      memory: 200Gi
      pods: "10"
    scopeSelector:
      matchExpressions:
      - operator : In
        scopeName: PriorityClass
        values: ["high"]
- apiVersion: v1
  kind: ResourceQuota
  metadata:
    name: cronjob-services-pods-medium
  spec:
    hard:
      cpu: "10"
      memory: 20Gi
      pods: "10"
    scopeSelector:
      matchExpressions:
      - operator : In
        scopeName: PriorityClass
        values: ["medium"]
- apiVersion: v1
  kind: ResourceQuota
  metadata:
    name: monitoring-services-pods-low
  spec:
    hard:
      cpu: "5"
      memory: 10Gi
      pods: "10"
    scopeSelector:
      matchExpressions:
      - operator : In
        scopeName: PriorityClass
        values: ["low"]
```

![](<../../.gitbook/assets/image (28).png>)

####

To see a list of all non-namespaced resources:

```
kubectl --kubeconfig cluster-admin api-resources --namespaced=false
```

To see whether tenants can perform operations on resources:

```
kubectl --kubeconfig “tenant_name” auth can-i “verb” “resource”
```

#### Limit Tenant’s Access to Resources from other Tenants

Continuing from the previous best practice cluster admins should also ensure that tenants cannot access namespaced resources belonging to other tenants. Namespaces resources are ones that are scoped to a specific namespace. As such cluster admins should ensure that tenants from other namespaces cannot view, edit, create or delete namespaces resources belonging to other tenants. 

To see a list of all namespaced resources belonging to a tenant:

```
kubectl --kubeconfig “tenant_name” api-resources --namespaced=true
```

To see whether tenants can perform operations on namespaced resources belonging to other tenants:

```
kubectl --kubeconfig “tenant_name” -n “namespace_name” “verb” “resource”
```

#### Limit Tenant’s Access to Multi-tenancy Resources

There is a subset of namespaced resources that should not be accessible to tenants of that namespace. These include the default network policy, namespace resource quotas and role bindings. Any changes to these resources can impact other tenants and their ability to consume or access resources. A best practice therefore is for cluster admins to ensure that this type of resource cannot be modified by tenants. 

To view list of resources managed by cluster admin:

```
kubectl --kubeconfig=cluster-admin -n “namespace_name” get all -l =
```

To verify that the resource cannot be modified by the namespace tenant (this requires labelling resources managed by the cluster admin):

```
kubectl --dry-run=true --kubeconfig=”tenant_name” -n “namespace_name” annotate key1=value1
```

#### Prevent use of HostPath Volumes

Tenants can use host volumes or directories to [access shared data](https://github.com/kubernetes-sigs/multi-tenancy/tree/master/benchmarks/e2e/tests/block_bind_mounts) or escalate privileges. A best practice therefore is for cluster admins to prevent tenants from creating pods with volumes of type HostPath. Cluster admins can do this natively using Kubernetes PodSecurityPolicy. [Here](https://gist.github.com/abhisek/b9acfdc0505905ffc4240841195326ee) is one example of a PodSecurityPolicy that prevents mounting HostPath.

#### Run Multi-tenancy e-2-e Validation Test 

The Kubernetes multi-tenancy SIG provides an e-2-e test tool that can be used to validate multi-tenant Kubernetes clusters. The e-2-e tool validates clusters based on a set of recommended multi-tenancy benchmarks. 

To run the tool do the following:

```
git clone https://github.com/kubernetes-sigs/multi-tenancy.git
```

```
cd multi-tenancy/benchmarks
```

Edit the config file (config.yaml) with your cluster configuration.

Run tests:

```
go test ./e2e/tests
```

Or with path to config file

```
go test ./e2e/tests -config 
```

####
