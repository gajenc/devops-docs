---
description: What you need to know when setting up Kubernetes yourself
---

# On Private Cloud

Running Kubernetes on-premise gives developers a cloud-native experience or sets up to be cloud-agnostic.

Whether you have your own on-premise data center — there’s a few things you should know when getting started with on-premise K8s.

Kubernetes has a key component called [**control plane**](https://kubernetes.io/docs/concepts/overview/components/#master-components)** **which consists of the kube-apiserver, kube-scheduler, kube-controller-manager and an etcd datastore. For managed cloud solutions like [AWS EKS](https://aws.amazon.com/eks/), [Google’s Kubernetes Engine (GKE)](https://cloud.google.com/kubernetes-engine/) or [Azure’s Kubernetes Service (AKS)](https://azure.microsoft.com/en-us/services/kubernetes-service/) it also includes the **cloud-controller-manager**. This is the component that connects the cluster to the external cloud services to provide networking, storage, authentication, and other feature support.

To successfully deploy a bespoke Kubernetes cluster and achieve a cloud-like experience you’ll need to replicate all the same features you get with a managed solution. At a high-level this means you’ll probably want to:

1. **Automate the Setup process**
2. **Choose a networking solution**
3. **Choose a storage solution**
4. **Handle security and authentication**

## 1. Automating the Setup process <a href="91a2" id="91a2"></a>

Using a tool like ansible can make Setting up Kubernetes clusters on-premise trivial.

1. When deciding to manage your own Kubernetes clusters you’ll want to setup a few proof-of-concept (PoC) clusters to learn how everything works, perform performance and conformance tests, and try out different configuration options.
2.  After this phase, automating the setup process is an important if not necessary step to ensure consistency across any clusters you build. For this you have a few options, but the most popular are:

    a. [kubeadm](https://kubernetes.io/docs/reference/setup-tools/kubeadm/kubeadm/): a low-level tool that helps you bootstrap a minimum viable Kubernetes cluster that conforms to best practices

    b.[kubespray](https://github.com/kubernetes-sigs/kubespray): an ansible playbook that helps deploy production ready clusters

If you already use ansible, kubespray is a great option otherwise It is recommend writing automation around kubeadm using your preferred playbook tool. This will also increase your confidence and knowledge in the tooling surrounding Kubernetes.

## 2. Choosing a network solution <a href="a6c3" id="a6c3"></a>

When designing clusters, choosing the right container networking interface (CNI) plugin can be the hardest part. This is because choosing a CNI that will work well with an existing network topology can be tough. Do you need BGP peering capabilities? Do you want an overlay network using vxlan? How close to bare-metal performance are you trying to get?

There are a lot of articles that compare the various CNI provider solutions (calico, weave, flannel, kube-router, etc.) that are must-reads like the [_benchmark results of Kubernetes network plugins_](https://itnext.io/benchmark-results-of-kubernetes-network-plugins-cni-over-10gbit-s-network-updated-april-2019-4a9886efe9c4) article. It is usually recommend Project Calico for its maturity, continued support, and large feature set or flannel for it’s simplicity.

For ingress traffic you’ll need to pick a load-balancer solution. For a simple configuration you can use MetalLB, but if you’re lucky enough to have F5 hardware load-balancers available I recommend checking out the [K8s F5 BIG-IP Controller](https://clouddocs.f5.com/containers/v2/kubernetes/). The controller supports connecting your network plugin to the F5 either through either vxlan or BGP peering. This gives the controller full visibility into pod health and provides the best performance.

## 3. Choosing a storage solution <a href="a998" id="a998"></a>

Kubernetes provides a number of [included storage volume plugins](https://kubernetes.io/docs/concepts/storage/storage-classes/#provisioner). If you’re going on-premise you’ll probably want to use a network-attached storage (NAS) option to avoid forcing pods to be pinned to specific nodes.

For a cloud-like experience, you’ll need to add a plugin to dynamically create persistent volume objects that match the user’s persistent volume claims. You can use dynamic provisioning to reclaim these volume objects after a resource has been deleted.

Pure Storage has a great example helm chart, the [_Pure Service Orchestrator_ (PSO)](https://github.com/purestorage/helm-charts), that provides smart provisioning although it only works for Pure Storage products.

## 4. Handle security and authentication <a href="4f36" id="4f36"></a>

As anyone familiar with security knows, this is a rabbit-hole. You can always make your infrastructure more secure, and should be investing in continual improvements.

Including different Kubernetes plugins can help build a secure, cloud-like experience for your users

When designing on-premise clusters you’ll have to decide where to draw the line. To really harden your cluster’s security you can add plugins like:

* [istio](https://istio.io): provides the underlying secure communication channel, and manages authentication, authorization, and encryption of service communication at scale
* [gVisor](https://github.com/google/gvisor): is a user-space kernel, written in Go, that implements a substantial portion of the Linux system surface
* [vault](https://www.vaultproject.io/docs/): secure, store and tightly control access to tokens, passwords, certificates, encryption keys for protecting secrets and other sensitive data

For user authentication, It is recommended checking out [guard](https://github.com/appscode/guard) which will integrate with an existing authentication provider. If you’re already using Github teams to then this could be a no-brainer.

## Other Considerations <a href="68ba" id="68ba"></a>

Like It is mentioned above, your team will want to build proof-of-concept clusters, run conformance and performance tests, and really become experts on Kubernetes if you’re going to be using it to run production software on a on-premise kubernetes cluster.

Few other things your team should be thinking of:

* Externally backing up Kubernetes YAML, namespaces, and configuration files
* Running applications across clusters in an active-active configuration to allow for zero-downtime updates
* Running game days like deleting the CNI to measure and improve time-to-recovery
