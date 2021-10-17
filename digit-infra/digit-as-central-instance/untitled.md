---
description: What does “multi-tenancy in Kubernetes” mean?
---

# Multi-tenancy with Kubernetes

Provide isolation and fair resource sharing between multiple users and their workloads within a single cluster

Multi-tenant Kubernetes clusters are one’s that share [cluster resources](https://www.replex.io/blog/5-ways-to-manage-your-kubernetes-resource-usage) among tenants. Tenants can be anything from groups of users to distributed teams, customers, applications, departments or projects.

Examples include multiple distributed DevOps teams deploying applications to the same cluster, staging and production environments residing on the same cluster, Kubernetes clusters being shared among multiple end-users or customers and multiple applications or workloads sharing a cluster’s resources.

Most enterprises want a multi-tenancy platform to run their cloud-native applications because it helps manage resources, costs, and operational efficiency and control cloud waste.

**Let’s visualise the multi-tenancy:**

**(1)** One tenant, one cluster (no multi-tenancy)

![](<../../.gitbook/assets/image (47).png>)

**(2)** Multiple tenants, multiple clusters (Dedicated resource for each tenant)

![](<../../.gitbook/assets/image (48).png>)

How does this (above (1) and (2)) scale operationally and financially?

![](<../../.gitbook/assets/image (49).png>)

**(3)** Many tenants, one cluster (Multi-tenancy)\


![](<../../.gitbook/assets/image (50).png>)

## **Why use multi-tenancy and benefits of multi-tenancy?** <a href="c8bc" id="c8bc"></a>

![](<../../.gitbook/assets/image (51).png>)

Multi-tenancy is the more common option for several reasons, but affordability tops the list:

**Cost efficiency:** Sharing of resources, databases, and the application itself means lower costs per customer. There is no need to buy or manage additional infrastructure or software. All the tenants share the server and storage space, which proves to be cheaper as it promotes economies of scale

**Fast, easy deployment:** With no new infrastructure to worry about, set-up and onboarding are simple. For instance carving out resources for a new team/project

**Built-in security:** Isolation between the tenants

**Optimum performance**: Multi-tenancies allow improve operational efficiency such as speed, utilisation, etc.

**High scalability:** Service small customers (whose size may not warrant dedicated infrastructure) and large organization's (that need access to unlimited computing resources).

## **Issues with multi-tenancy?** <a href="ea8e" id="ea8e"></a>

Compliance and regulations: Although rare, some regulations and industries will limit what data can be stored in a multi-tenant environment

## **How to implement multi-tenancy?** <a href="1d1d" id="1d1d"></a>

### _**(a) Namespaces**_ <a href="986e" id="986e"></a>

Namespaces are the primary unit of tenancy in Kubernetes. By themselves, they don’t do much except organize other objects — but almost all policies support namespaces by default

* Require cluster-level permissions to create
* Included in Kubernetes natively
* Official Kubernetes documentation on namespaces: [https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/](https://kubernetes.io/docs/concepts/overview/working-with-objects/namespaces/)

### _**(b) RBAC: Role based access control for Kubernetes**_ <a href="9b78" id="9b78"></a>

Kubernetes includes a built-in _role-based access control_ mechanism that enables you to configure fine-grained and specific sets of permissions that define how a given Google Cloud user, or group of users, can interact with any Kubernetes object in your cluster, or in a specific Namespace of your cluster.

* Kubernetes RBAC is enabled by default
* Official Kubernetes documentation on RBAC: [https://kubernetes.io/docs/reference/access-authn-authz/rbac/](https://kubernetes.io/docs/reference/access-authn-authz/rbac/)

### **(C) Network Policies: To isolate traffic between the namespaces** <a href="93b6" id="93b6"></a>

Network policies allow you to limit connections between Pods. Therefore, using network policies provide better security by reducing the compromise radius.

* Network Policies are an application-centric construct which allow you to specify how a [pod](https://kubernetes.io/docs/concepts/workloads/pods/) is allowed to communicate with various network “entities”
* Note that the network policies determine whether a connection is allowed, and they do not offer higher level features like authorization or secure transport (like SSL/TLS).
* Control traffic flow at the IP address or port level (OSI layer 3 or 4)
* Official Kubernetes documentation on Network Policies: [https://kubernetes.io/docs/concepts/services-networking/network-policies/](https://kubernetes.io/docs/concepts/services-networking/network-policies/)

## Use-case: <a href="5419" id="5419"></a>

Most common one: Different teams within the same company.

Within one company, you say we share one big pool of resources and different teams share them. The different teams really have good incentive and good reason to behave nicely. They’re not assumed to be malicious, they trust each other and accidents happen, and that’s what you try to protect from, but you don’t assume that they’re completely not trusted.

In that model, as you may by now have guessed, different teams will typically get different namespaces to share in one cluster. Oftentimes, what happens is that multi-tenancy is still something that requires a little bit of setup or maybe a lot of setup. Kubernetes administrator needs to make sure that policies are applied correctly and network is set up consistently, and so forth for these shared clusters.

## **Conclusion** <a href="e19d" id="e19d"></a>

1\. Understand your needs. Not everyone needs everything all at once! Think about cost, overhead, and risk tolerance.

2\. Understand the pros/cons of various approaches and technologies to solve your most critical problems.

3\. Deploy your solutions, and keep them up-to-date, and iterate to achieve more benefits.

## References: <a href="fb4e" id="fb4e"></a>

[https://www.cncf.io/wp-content/uploads/2020/08/CNCF-Webinar\_-better-walls-make-better-tenants.pdf](https://www.cncf.io/wp-content/uploads/2020/08/CNCF-Webinar\_-better-walls-make-better-tenants.pdf)
