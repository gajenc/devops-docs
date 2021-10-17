# Security Recommendations

## Introduction <a href="introduction" id="introduction"></a>

Security is always a difficult subject to approach either by the lack of experience; either by the fact you should know when the level of security is right for what you have to secure.

Security is a major concern when it comes to government systems and infra. As an architect, we can consider that working with technically educated people (engineers, experts) and tools (systems, frameworks, IDE) should prevent key VAPT issues.

However, it’s quite difficult to avoid, a certain infatuation from different categories of people to try to hack the systems.

## 1. Update to the latest version of the tools <a href="1-update-to-the-latest-version" id="1-update-to-the-latest-version"></a>

There aren’t only bug fixes in each release but also new security measures to require advantage of them, we recommend working with the newest stable version.

Updates and support could also be harder than the new features offered in releases, so plan your updates a minimum of once a quarter. Significantly simplify updates can utilize the providers of managed Kubernetes-solutions.

## 2. Enable role-based access control (RBAC) <a href="2-enable-role-based-access-control-rbac" id="2-enable-role-based-access-control-rbac"></a>

Use RBAC (Role-Based Access Control) to regulate who can access and what rights they need. Usually, RBAC is enabled by default in [**Kubernetes**](https://onlineitguru.com/blogger/what-is-kubernetes) version 1.6 and later (or later for a few providers), but if you’ve got been updated since then and didn’t change the configuration, you ought to double-check your settings.

However, enabling RBAC isn’t enough — it still must be used effectively. within the general case, the rights to the whole cluster (cluster-wide) should be avoided, giving preference to rights in certain namespaces. Avoid giving someone cluster administrator privileges even for debugging — it’s much safer to grant rights only necessary and from time to time.

If the appliance requires access to the Kubernetes API, create separate service accounts. and provides them with the minimum set of rights required for every use case. This approach is far better than giving an excessive amount of privilege to the default account within the namespace.

## 3. Use namespaces to set security boundaries <a href="3-use-namespaces-to-set-security-boundaries" id="3-use-namespaces-to-set-security-boundaries"></a>

Creating separate namespaces **is vital because of the** first level of component isolation. **it’s** much easier **to regulate** security settings — **for instance**, network policies — when **different types** of workloads are deployed in separate namespaces.

To get in-depth knowledge on Kubernetes, enrol a love demo on [**Kubernetes course**](https://onlineitguru.com/kubernetes-training.html)​

## 4. Separate sensitive workloads <a href="4-separate-sensitive-workloads" id="4-separate-sensitive-workloads"></a>

A good practice to limit the potential consequences of compromise is to run workloads with sensitive data on **a fanatical** set of machines. This approach reduces the risk of a less secure application accessing the application with sensitive data running in the same container executable environment or on the same host.

For example, a kubelet of a compromised node usually has access to the contents of secrets only if they are mounted on pods that are scheduled to be executed on the same node. If important secrets **are often** found on multiple cluster nodes, the attacker will have more opportunities **to urge** them.

Separation can be done using node pools (in the cloud or for on-premises), as well as Kubernetes controlling mechanisms, such as namespaces, taints, tolerations, and others.

Sensitive metadata — for instance, kubelet administrative credentials, are often stolen or used with malicious intent to escalate privileges during a cluster. For example, a recent find within Shopify’s bug bounty showed in detail how a user could exceed authority by receiving metadata from a cloud provider using specially generated data for one of the microservices.

The GKE metadata concealment function changes the mechanism for deploying the cluster in such how that avoids such a drag. And we recommend using it until a permanent solution is implemented.

## 6. Create and define cluster network policies <a href="6-create-and-define-cluster-network-policies" id="6-create-and-define-cluster-network-policies"></a>

Network Policies — allow you to control access to the network in and out of containerized applications. To use them, you must have a network provider with support for such a resource. For managed Kubernetes solution providers such as Google Kubernetes Engine (GKE), support will need to be enabled.

Once everything is ready, start with simple default network policies — for example, blocking (by default) traffic from other namespaces.

## 7. Set the Pod Security Policy for the cluster <a href="7-set-the-pod-security-policy-for-the-cluster" id="7-set-the-pod-security-policy-for-the-cluster"></a>

Pod Security Policy sets the default values ​​used to start workloads in the cluster. Consider defining a policy and enabling the Pod Security Policy admission controller: the instructions for these steps vary depending on the cloud provider or deployment model used.

In the beginning, you might want to disable the NET_RAW capability in containers to protect yourself from certain types of spoofing attacks.

## 8. Work on node security <a href="8-work-on-node-security" id="8-work-on-node-security"></a>

To improve host security, you can follow these steps:

* **Ensure that the host is securely and correctly configured.** One way is CIS Benchmarks; Many products have an auto checker that automatically checks the system for compliance with these standards.
* **Monitor the network availability of important ports.** Ensure that the network is blocking access to the ports used by kubelet, including 10250 and 10255. Consider restricting access to the Kubernetes API server — with the exception of trusted networks. In clusters that did not require authentication and authorization in the kubelet API, attackers used to access to such ports to launch cryptocurrency miners.
* **Minimize administrative access to Kubernetes hosts** Access to cluster nodes should in principle be limited: for debugging and solving other problems, as a rule, you can do without direct access to the node.

## 9. Enable Audit Logging <a href="9-enable-audit-logging" id="9-enable-audit-logging"></a>

Make sure that audit logs are enabled and that you are monitoring for the occurrence of unusual or unwanted API calls in them, especially in the context of any authorization failures — such entries will have a message with the “Forbidden” status. Authorization failures can mean that an attacker is trying to take advantage of the credentials obtained.

Managed solution providers (including GKE) provide access to this data in their interfaces and can help you set up notifications in case of authorization failures.

## Conclusion <a href="conclusion" id="conclusion"></a>

Follow these guidelines for a more secure [**Kubernetes cluster**](https://medium.com/faun/what-is-the-kubernetes-cluster-de2a33e6572?source=---------24------------------). Remember that even after the cluster is configured securely, you need to ensure security in other aspects of the configuration and operation of containers. To improve the security of the technology stack, study the tools that provide a central system for managing deployed containers, constantly monitoring and protecting containers and cloud-native applications.
