# Kubespray

## **Kubernetes Cluster Setup with Kubespray & Ansible** <a href="7111" id="7111"></a>

Ensure you have number of master and worker node VMs ready with the required CPU, Memory and storage capacity. You can refer the details Infra requirement to provision kubernetes cluster. Here we can see some sample ways with the step by step instructions of ansible.

> **1. Generate ssh key on host machine and transfer the keys to master and nodes to establish password-less authentication.**

Here,

Master : 172.27.XX.1XX

Node1 : 172.27.XX.4XX

Node2 : 172.27.XX.8XX

_Command :_

```
ssh-keygen
```

![](<../../../../.gitbook/assets/image (3).png>)

Transfer the keys to all master and nodes.

_Command :_

```
ssh-copy-id root@
```

Note : In case you do not know password, just copy and paste the keys.

> **2. Get the kubespray repo on your machine and untar it.**

_Command :_

```
wget https://github.com/kubernetes-incubator/kubespray/archive/v2.7.0.tar.gztar -xzf v2.7.0.tar.gz
```

> **3. Install the required packages as mentioned in requirements.txt.**

Note : pip needs to be installed on the machine as a pre-requisite.

```
sudo pip install -r requirements.txt
```

> **4. Create a folder for your cluster and copy the required files from “sample” folder.**

```
mkdir inventory/mycluster cp -rfp inventory/sample/* inventory/mycluster
```

> **5. Remove the unnecessary files. \[OPTIONAL STEP]**

_Command :_

```
rm -rf inventory/sample inventory/local/
```

> **6. Update Ansible inventory file with inventory builder**

_`declare -a IPS=(172.27.XX.1XX 172.27.XX.1XX 172.27.XX.1XX 172.27.XX.1XX)`_

_`CONFIG_FILE=inventory/mycluster/hosts.ini python3 contrib/inventory_builder/inventory.py ${IPS[@]}`_

> **7. Update your host.ini file as per your master-node architecture**

![](<../../../../.gitbook/assets/image (4).png>)

\[k8s-cluster:children]\
kube-master\
kube-node

\[all]\
master ansible_host=172.27.XX.1XX ip=172.27.XX.1XX\
node1 ansible_host=172.27.XX.XX ip=172.27.XX.XX\
node2 ansible_host=172.27.XX.XX ip=172.27.XX.XX

\[kube-master]\
master

\[kube-node]\
node1\
node2

\[etcd]\
master\
node1\
node2

\[calico-rr]

\[vault]\
node1\
node2

> **8. Run ansible playbook to create the cluster.**

_`ansible-playbook -i inventory/mycluster/hosts.ini cluster.yml`_

> **Addition of Node**

Update hosts.ini \[add a node in hosts.ini] and run below command.

`ansible-playbook -i inventory/mycluster/hosts.ini scale.yml –flush-cache`

_`ansible-playbook -i inventory/mycluster/hosts.ini cluster.yml`_

> **Deletion of Node**

It supports two ways to select the nodes:\
Use — extra-vars “node=,” to select the node you want to delete.

```
ansible-playbook -i inventory/mycluster/hosts.ini remove-node.yml -b -v \  --private-key=~/.ssh/private_key \  --extra-vars "node=nodename,nodename2"
```

or\
Use — limit nodename,nodename2 to select the node

```
ansible-playbook -i inventory/mycluster/hosts.ini remove-node.yml -b -v \  --private-key=~/.ssh/private_key \  --limit “nodename,nodename2" Ex. : 
ansible-playbook -i inventory/mycluster/hosts.ini remove-node.yml -b -v --private-key=~/.ssh/private_key   --extra-vars "node=node2"
```

![](<../../../../.gitbook/assets/image (5).png>)

> **Clean up**

Normally running reset.yml will cater all the clean up but sometimes to completely reset everything we need to run both the command in below sequence.

`ansible-playbook -i inventory/mycluster/hosts.ini remove-node.yml --flush-cache`

`ansible-playbook -i inventory/mycluster/hosts.ini reset.yml –flush-cache`

### **Possible Issues & Solutions**

> **Issue 1 : Permission denied**

**Solution** : You need to give all root/sudo access to the user which is used by ansible to use the playbook.

> **Issue 2 : Python not found.**

**Solution** : You need to check on master if python is installed on different folder or not present.\
If python is not present on master, then install.\
If python is installed at different location, create a symlink.

_`ln -s`_

> **Issue 3 : The conditional check ‘hostvars\[item]\[‘cluster_id’] == cluster_id’ failed.**

**Solution** : Remove nodes from \[calico-rr] and re-run below playbooks in sequence.

`ansible-playbook -i inventory/mycluster/hosts.ini remove-node.yml --flush-cache`

`ansible-playbook -i inventory/mycluster/hosts.ini reset.yml --flush-cache`

> **Issue 4 : Failed to create ‘IPPool’ resource: resource already exists: IPPool(default-pool).**

**Solution** : Run below playbooks in sequence.

`ansible-playbook -i inventory/mycluster/hosts.ini remove-node.yml --flush-cache`

`ansible-playbook -i inventory/mycluster/hosts.ini reset.yml –flush-cache`

> **Issue 5 : apt-get update”, “msg”: “W: The repository ‘**[**https://download.docker.com/linux/ubuntu**](https://download.docker.com/linux/ubuntu) **xenial Release’ does not have a Release file.**

**Solution** : Run below playbooks in sequence.

`ansible-playbook -i inventory/mycluster/hosts.ini remove-node.yml --flush-cache`

`ansible-playbook -i inventory/mycluster/hosts.ini reset.yml –flush-cache`

> **Issue 6 : fatal: \[node3]: FAILED! => {“msg”: “Timeout (12s) waiting for privilege escalation prompt: “}**

**Solution** : Enable Internet access on the machine where error is occurring.

> **Issue 7 : E:Unable to parse package file /var/lib/apt/lists/partial/us.archive.ubuntu.com_ubuntu_dists_xenial-updates_universe_binary-i386\_Packages.diff_Index (1), E:Unable to parse package file /var/lib/apt/lists/partial/us.archive.ubuntu.com_ubuntu_dists_xenial-backports_main_i18n_Translation-en.diff_Index (1)”}**

**Solution** : Run below commands on the machine which is giving error.\
sudo rm -r /var/lib/apt/lists/\*\
sudo apt-get update



References:

####  [To Know more on kubespray](https://kubernetes.io/docs/setup/production-environment/tools/kubespray/)

