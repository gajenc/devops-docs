---
description: Requirement and estimation
---

# Infra Requirement

## Overview

While DIGIT is a microservice based and cloud native platform/application, it can literally be deployed anywhere based on this [recommendations](infra-recommendation.md). 

**Public Clouds:** All the public clouds like AWS, Azure, GCP, etc.. will have the kubernetes as completely managed services with full fledged elasticity, secured, optimized, pay-as-u-go, instantaneous, cost-effective ways to provision a kubernetes cluster.  

**Private Clouds:** Most of the SDCs and NICs are essentially a private clouds that has got the basic services like VMs, Storage, Network, Load Balancers, Routers, Firewall, etc. These are the infra requirements to create kubernetes cluster as well. Some SDCs might already have kubernetes as a manages services, where you may just have to subscribe to it. Where as some SDCs might kubernetes as a service in case if the underlying Infra was either from VMware, IBM, Nutanix, OpenStack, Oracle, etc.

In case of managed kubernetes service availability you can directly subscribe to it and get started with the kubernetes cluster instantaneously. Just the worker nodes capacity to be estimated based on the number applications and anticipated load. [Refer here to provision infra on public cloud.](../devops-general/the-rise-of-kubernetes/setup-kubernetes/on-cloud.md)

In the absence of the kubernetes as a managed service, one needs to provision the kubernetes cluster from the available base infra.  [Refer this to provision kubernetes cluster on private cloud where managed kubernetes cluster is not available](../devops-general/the-rise-of-kubernetes/setup-kubernetes/on-premise-1/).  

### **Infra Requirement** 

**Public Clouds:** All the public clouds like AWS, Azure, GCP, etc.. will have the kubernetes as completely managed services with full fledged elasticity, secured, optimized, pay-as-u-go, instantaneous, cost-effective ways to provision a kubernetes cluster.  

**Private Clouds:** Most of the SDCs and NIC are essentially a private clouds that has got the basic services like VMs, Storage, Network, Load Balancers, Routers, Firewall, etc. These are the infra requirements to create kubernetes cluster as well. Some SDCs might already have kubernetes as a managed service in case of the underlying Infra was either from VMware, IBM, Nutanix, OpenStack, Oracle, etc. Where as some SDCs might have to provision kubernetes as a service from the bare-metal or simple VM infrastructure.

To provision a kubernetes cluster you need master nodes \(VMs\) for the kubernetes cluster components and worker nodes \(VMs\) for the application workload that'll be deployed. 

<table>
  <thead>
    <tr>
      <th style="text-align:left"><b>Systems</b>
      </th>
      <th style="text-align:left"><b>Specification</b>
      </th>
      <th style="text-align:left"><b>Spec/Count</b>
      </th>
      <th style="text-align:left"><b>Comment</b>
      </th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left"><b>Envs</b>
      </td>
      <td style="text-align:left">Dev, UAT and Prod<b> </b>
      </td>
      <td style="text-align:left"><b>3 </b>
      </td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left"><b>VPN/Cluster Roles</b>
      </td>
      <td style="text-align:left">Admin, Deploy, ReadOnly</td>
      <td style="text-align:left"><b>3</b>
      </td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left"><b>OS</b>
      </td>
      <td style="text-align:left">Any Linux (preferably Ubuntu/RHEL)</td>
      <td style="text-align:left"><b>All</b>
      </td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left"><b>VM Spec</b>
      </td>
      <td style="text-align:left">root, SSH with Internet enabled</td>
      <td style="text-align:left">All the VMs</td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left"><b>Kubernetes as a managed service (or) VMs to provision own Kubernetes</b>
      </td>
      <td style="text-align:left">
        <p>Managed Kubernetes service with HA/DRS</p>
        <p>(Or) VMs with 2 vCore, 4 GB RAM, 20 GB Disk</p>
      </td>
      <td style="text-align:left">
        <p><b>If no managed k8s</b>
        </p>
        <p><b>3 VMs/env</b>
        </p>
      </td>
      <td style="text-align:left">
        <p><b>Dev - 1 VM</b>
        </p>
        <p><b>UAT - 1VM</b>
        </p>
        <p><b>Prod - 3VMs<br /></b>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><b>Kubernetes worker nodes (or) VMs to provision Kube worker nodes.</b>
      </td>
      <td style="text-align:left">VMs with 4 vCore, 16 GB RAM, 20 GB Disk / per env</td>
      <td style="text-align:left"><b>3-5 VMs/env </b>
      </td>
      <td style="text-align:left">
        <p><b>DEV - 3VMs</b>
        </p>
        <p><b>UAT - 4VMs</b>
        </p>
        <p><b>PROD - 5VMs</b>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><b>Disk Storage (SAN/NFS/NAS/iSCSI) </b>
      </td>
      <td style="text-align:left">Storage with backup, snapshot, dynamic inc/dec</td>
      <td style="text-align:left"><b>1 TB/env</b>
      </td>
      <td style="text-align:left">
        <p><b>Dev - 1000 GB</b>
        </p>
        <p><b>UAT - 800 GB</b>
        </p>
        <p><b>PROD - 1.5 TB</b>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><b>VM Instance IOPS</b>
      </td>
      <td style="text-align:left">Max throughput 1750 MB/s</td>
      <td style="text-align:left"><b>1750 MS/s</b>
      </td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left"><b>Disk IOPS</b>
      </td>
      <td style="text-align:left">Max throughput 1000 MB/s</td>
      <td style="text-align:left"><b>1000 MB/s</b>
      </td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left"><b>Internet Speed</b>
      </td>
      <td style="text-align:left">Min 100 MB - 1000MB/Sec (dedicated bandwidth)</td>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left"><b>Public IP/NAT or LB</b>
      </td>
      <td style="text-align:left">Internet-facing 1 public ip per env</td>
      <td style="text-align:left"><b>3</b>
      </td>
      <td style="text-align:left"><b>3 Ips</b>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><b>Availability Region</b>
      </td>
      <td style="text-align:left">VMs from the different region is preferable for the DRS/HA</td>
      <td style="text-align:left"><b>1 or 2 Regions</b>
      </td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left"><b>Private vLan</b>
      </td>
      <td style="text-align:left">Per env, we recommend all VMs should within private vLan/VPC</td>
      <td style="text-align:left">1 per env</td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left"><b>Gateways</b>
      </td>
      <td style="text-align:left">NAT Gateway, Internet Gateway, Payment and SMS gateway</td>
      <td style="text-align:left"><b>1 per env</b>
      </td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left"><b>Firewall</b>
      </td>
      <td style="text-align:left">Ability to configure Inbound, Outbound ports/rules</td>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p><b>Managed DataBase </b>
        </p>
        <p><b>(or) VM Instance</b>
        </p>
      </td>
      <td style="text-align:left">
        <p>Postgres 12 &amp; above Managed DB with backup, snapshot, logging capabilities.</p>
        <p>(Or) 1 VM with 4 vCore, 16GB RAM, 100 GB Disk per env.</p>
      </td>
      <td style="text-align:left"><b>per env</b>
      </td>
      <td style="text-align:left">
        <p><b>DEV - 1VMs</b>
        </p>
        <p><b>UAT - 1VMs</b>
        </p>
        <p><b>PROD - 2VMs</b>
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><b>CI/CD server self hosted (or) Managed DevOps</b>
      </td>
      <td style="text-align:left">
        <p>Self Hosted Jenkins : Master, Slave (VM 4vCore, 8 GB each)</p>
        <p>(Or) Managed CI/CD: NIC DevOps or AWS CodeDeploy or Azure DevOps</p>
      </td>
      <td style="text-align:left"><b>2 VMs (Master, Slave)</b>
      </td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left"><b>Nexus Repo</b>
      </td>
      <td style="text-align:left">Self hosted Artifactory Repo (Or) NIC Nexus Artifactory</td>
      <td style="text-align:left"><b>1</b>
      </td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left"><b>DockerRegistry</b>
      </td>
      <td style="text-align:left">DockerHub (Or) SelfHosted private docker reg</td>
      <td style="text-align:left"><b>1</b>
      </td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left"><b>Git/SCM</b>
      </td>
      <td style="text-align:left">GitHub (Or) Any Source Control tool</td>
      <td style="text-align:left"><b>1</b>
      </td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left"><b>DNS </b>
      </td>
      <td style="text-align:left">main domain &amp; ability to add more sub-domain</td>
      <td style="text-align:left"><b>1</b>
      </td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left"><b>SSL Certificate</b>
      </td>
      <td style="text-align:left">NIC managed (Or) SDC managed SSL certificate per URL</td>
      <td style="text-align:left"><b>2 urls per env</b>
      </td>
      <td style="text-align:left"></td>
    </tr>
  </tbody>
</table>

  

Here is the useful links to understand DIGIT Infra prerequisites:

* [Understand containers, dockers](https://medium.com/faun/containers-docker-kubernetes-introduction-part-1-fed143dafc05)
* [What is kubernetes](https://medium.com/easyread/step-by-step-introduction-to-basic-concept-of-kubernetes-e20383bdd118) and [Architecture](https://medium.com/edureka/kubernetes-architecture-c43531593ca5)
* [Requirements to setup kubernetes cluster](https://medium.com/faun/kubernetes-prerequisites-for-setup-kubenetes-cluster-part-2-699b3f93d6cc)
* [Provider Managed Cluster vs Self provisioning Cluster](https://www.magalix.com/blog/provider-managed-vs.-self-managed-kubernetes) 

