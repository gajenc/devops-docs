---
description: Kubernetes Setup Instruction using Kubeadm
---

# Kubeadm

## Setting Up Kubernetes Cluster

Let's look at the steps that includes to setup kubeadm that can help deploying a kubernetes cluster on any of the Ubuntu 16.04 or above machine.

### **High level Kubernetes Master-Node Architecture **

![Image for post](https://miro.medium.com/max/1276/1\*56l6-yNIXFaNZgofRqPAkA.jpeg)

**Prerequisites:**

1. Make sure below ports are open for the communication of Kubernetes Master & Node.
2. A good Internet connectivity
3. Make sure the OS is updates with the apt-get update
4. Make sure the user which you are using have **sudo** privileges. You can add your user into sudoers group by following these [steps](https://www.digitalocean.com/community/tutorials/how-to-create-a-sudo-user-on-ubuntu-quickstart).

**Inbound Rules** For Kubernetes **Master**

![Image for post](https://miro.medium.com/max/1038/1\*TU4kzu3yV4K62vCOwEhk3A.png)

**Inbound Rules** For Kubernetes **Worker** Node

![Image for post](https://miro.medium.com/max/1068/1\*1GjO922JqoWOZvsC2r491w.png)

### **Step 1 : Install Docker**

```
apt-get update
apt-get install -y docker.io
```

### **Step 2 : Install Kubeadm, Kubelet & Kubectl**

**Note:** If you already have kubeadm installed, you should run below command and skip the step 2.

**`apt-get update && apt-get upgrade`**

You can copy below command and paste it directly in you command console if you don’t have kubeadm installed.

```
apt-get update && apt-get install -y apt-transport-https curl
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key add -
cat <<EOF >/etc/apt/sources.list.d/kubernetes.list
deb http://apt.kubernetes.io/ kubernetes-xenial main
EOF
apt-get update
apt-get install -y kubelet kubeadm kubectl
```

### **Step 3 : Checking the CGROUP Driver**

The CGROUP driver used by Kubelet should be same as used by Docker.

You can verify the same by using the below command.

```
docker info | grep -i cgroup
cat /etc/systemd/system/kubelet.service.d/10-kubeadm.conf
```

If the Docker CGROUP driver and the Kubelet config don’t match, change the Kubelet config to match the Docker CGROUP driver.

The flag you need to change is “cgroup-driver”.

If it’s already set, you can update like so:

```
sed -i "s/cgroup-driver=systemd/cgroup-driver=cgroupfs/g" /etc/systemd/system/kubelet.service.d/10-kubeadm.conf
```

Otherwise, you will need to open the systemd file and add the flag to an existing environment line.

### **Step 4 : Restart Kubelet**

```
systemctl daemon-reload
systemctl restart kubelet
```

### **Step 5 : Initializing Kubernetes Master**

Run below command to initialize your Kubernetes Master.

```
kubeadm init
```

The output will be like :

```
[init] Using Kubernetes version: v1.8.0
[init] Using Authorization modes: [Node RBAC]
[preflight] Running pre-flight checks
[kubeadm] WARNING: starting in 1.8, tokens expire after 24 hours by default (if you require a non-expiring token use --token-ttl 0)
[certificates] Generated ca certificate and key.
[certificates] Generated apiserver certificate and key.
[certificates] apiserver serving cert is signed for DNS names [kubeadm-master kubernetes kubernetes.default kubernetes.default.svc kubernetes.default.svc.cluster.local] and IPs [10.96.0.1 10.138.0.4]
[certificates] Generated apiserver-kubelet-client certificate and key.
[certificates] Generated sa key and public key.
[certificates] Generated front-proxy-ca certificate and key.
[certificates] Generated front-proxy-client certificate and key.
[certificates] Valid certificates and keys now exist in "/etc/kubernetes/pki"
[kubeconfig] Wrote KubeConfig file to disk: "admin.conf"
[kubeconfig] Wrote KubeConfig file to disk: "kubelet.conf"
[kubeconfig] Wrote KubeConfig file to disk: "controller-manager.conf"
[kubeconfig] Wrote KubeConfig file to disk: "scheduler.conf"
[controlplane] Wrote Static Pod manifest for component kube-apiserver to "/etc/kubernetes/manifests/kube-apiserver.yaml"
[controlplane] Wrote Static Pod manifest for component kube-controller-manager to "/etc/kubernetes/manifests/kube-controller-manager.yaml"
[controlplane] Wrote Static Pod manifest for component kube-scheduler to "/etc/kubernetes/manifests/kube-scheduler.yaml"
[etcd] Wrote Static Pod manifest for a local etcd instance to "/etc/kubernetes/manifests/etcd.yaml"
[init] Waiting for the kubelet to boot up the control plane as Static Pods from directory "/etc/kubernetes/manifests"
[init] This often takes around a minute; or longer if the control plane images have to be pulled.
[apiclient] All control plane components are healthy after 39.511972 seconds
[uploadconfig] Storing the configuration used in ConfigMap "kubeadm-config" in the "kube-system" Namespace
[markmaster] Will mark node master as master by adding a label and a taint
[markmaster] Master master tainted and labelled with key/value: node-role.kubernetes.io/master=""
[bootstraptoken] Using token: <token>
[bootstraptoken] Configured RBAC rules to allow Node Bootstrap tokens to post CSRs in order for nodes to get long term certificate credentials
[bootstraptoken] Configured RBAC rules to allow the csrapprover controller automatically approve CSRs from a Node Bootstrap Token
[bootstraptoken] Creating the "cluster-info" ConfigMap in the "kube-public" namespace
[addons] Applied essential addon: kube-dns
[addons] Applied essential addon: kube-proxy

Your Kubernetes master has initialized successfully!

To start using your cluster, you need to run (as a regular user):

  mkdir -p $HOME/.kube
  sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
  sudo chown $(id -u):$(id -g) $HOME/.kube/config

You should now deploy a pod network to the cluster.
Run "kubectl apply -f [podnetwork].yaml" with one of the options listed at:
  http://kubernetes.io/docs/admin/addons/

You can now join any number of machines by running the following on each node
as root:

  kubeadm join 10.XXX.0.5:6443 --token 44zge3.4sdqgem4t9v1loc8 --discovery-token-ca-cert-hash sha256:1f1823262cfe3a263c5f1178178597d1f5b3faf397f9e8717c1f57b72103143a
```

The command mentioned below will be used to join Master and needs to be run on node.

```
kubeadm join <master-ip>:<master-port> --token <token> --discovery-token-ca-cert-hash sha256:<hash>
```

In our output,

Master IP : 10.XXX.0.5

Master Port : 6443

Token : 44zge3.4sdqgem4t9v1loc8

Hash :1f1823262cfe3a263c5f1178178597d1f5b3faf397f9e8717c1f57b72103143a

To make kubectl work for your non-root user, you might want to run these commands (which is also a part of the **`kubeadm init`** output):

```
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

Alternatively, if you are the **`root`** user, you could run this:

```
export KUBECONFIG=/etc/kubernetes/admin.conf
```

### **Step 6 : Installing a POD Network with Weave Net**

**NOTE:** You can install **only one** pod network per cluster.

Run below command to install a POD network with Weave Net.

```
export kubever=$(kubectl version | base64 | tr -d '\n')
kubectl apply -f "https://cloud.weave.works/k8s/net?k8s-version=$kubever"
```

The output will be like below, Weave Net POD Network Creation

![Image for post](https://miro.medium.com/max/2732/1\*KGK86pFSSukIAAQZHbPnHw.png)

Check your master is ready or not.

**Troubleshooting** :

In case if your master is showing STATUS = Not Ready, run below command to see logs.

```
journalctl -xe
```

If you are seeing any message as below,

![Image for post](https://miro.medium.com/max/1348/1\*aEfcfkMGXAg72i1-CTyf_A.png)

Run below command which sets the configuration of weave net to version 1.6 which overcomes this issue.

```
kubectl apply — filename https://git.io/weave-kube-1.6
```

### **Step 7 : Adding Nodes to Network**

Prerequisite : Node should have **Kubeadm & Kubectl **installed. \[Refer Step 2]

* SSH to the machine
* Become root (e.g. **`sudo su -`**)
* Run the command that was output by **`kubeadm init`**. For example:

```
kubeadm join --token <token> <master-ip>:<master-port> --discovery-token-ca-cert-hash sha256:<hash>
```

In our case the command is as below,

```
kubeadm join 10.XXX.0.5:6443 --token 44zge3.4sdqgem4t9v1loc8 --discovery-token-ca-cert-hash sha256:1f1823262cfe3a263c5f1178178597d1f5b3faf397f9e8717c1f57b72103143a
```

The output will be like,

![Image for post](https://miro.medium.com/max/2732/1\*0n8ADJIQYsq0NwZ6Z59NQQ.png)

If you want to check the node is successfully added to the network or not, go to master and run below command and check the output.

```
kubectl get nodes
```

You will be seeing both master and node in ready status as below.

![Image for post](https://miro.medium.com/max/1546/1\*c3JvZZVMMKn7OibsjNPuiw.png)

Your Kubernetes Master-Node Architecture is ready for rock and roll! :)

[**To know more on kubeadm**](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/create-cluster-kubeadm/)****
