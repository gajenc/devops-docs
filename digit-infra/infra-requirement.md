---
description: Requirement and estimation
---

# Infra Overview

DIGIT being a microservice based and cloud native application, it can be deployed anywhere -  be it a public cloud or private cloud like SDC or NIC. The Infra requirement to deploy DIGIT is abstracted as [Kubernetes](../devops-general/the-rise-of-kubernetes/), It allows to plan, standardize the deployment and orchestration for any infra type. Essentially you can create kubernetes cluster from any infra type like on-premise, bare metal, physical machines, etc. It provides flexibility for running cloud-native applications anywhere like physical or virtual infrastructure or hypervisor or HCI and so on. Kubernetes handles the work of scheduling containerized services onto a compute cluster and manages the workloads to ensure they run as intended. And it substantially simplifies the deployment and management of microservices. 

Provisioning the kubernetes cluster will vary across from [commercial clouds](https://docs.google.com/spreadsheets/d/1RPpyDOLFmcgxMCpABDzrsBYWpPYCIBuvAoUQLwOGoQw/edit#gid=907731238) to state data centers, especially in the absence of [managed kubernetes services](https://medium.com/swlh/state-of-managed-kubernetes-2020-4be006643360) like AWS, Azure, GCP and NIC. Kubernetes clusters can also be provisioned on SDCs with bare-metal, virtual machines, hypervisors, HCI, etc. However providing integrated networking, monitoring, logging, and alerting is critical for operating Kubernetes Clusters when it comes to State data centers. DIGIT Platform also offers addons to monitor kubernetes cluster performance, logging, tracing, service monitoring and alerting, where the implementation team can take advantage.

**Public Clouds:** All the public clouds like AWS, Azure, GCP, etc.. will have the kubernetes as completely managed services with full fledged elasticity, secured, optimized, pay-as-u-go, instantaneous, cost-effective ways to provision a kubernetes cluster.  

**Private Clouds:** Most of the SDCs and NICs are essentially a private clouds that has got the basic services like VMs, Storage, Network, Load Balancers, Routers, Firewall, etc. These are the infra requirements to create kubernetes cluster as well. Some SDCs might already have kubernetes as a manages services, where you may just have to subscribe to it. Where as some SDCs might kubernetes as a service in case if the underlying Infra was either from VMware, IBM, Nutanix, OpenStack, Oracle, etc.

In case of managed kubernetes service availability you can directly subscribe to it and get started with the kubernetes cluster instantaneously. Just the worker nodes capacity to be estimated based on the number applications and anticipated load. [Refer here to provision infra on public cloud.](../devops-general/the-rise-of-kubernetes/setup-kubernetes/on-cloud.md)

In the absence of the kubernetes as a managed service, one needs to provision the kubernetes cluster from the available base infra.  [Refer this to provision kubernetes cluster on private cloud where managed kubernetes cluster is not available](../devops-general/the-rise-of-kubernetes/setup-kubernetes/on-premise-1/).  

**Infra Requirement** 

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
      <td style="text-align:left"><b>User Accounts/Envs</b>
      </td>
      <td style="text-align:left"><b>Dev, UAT and Prod </b>
      </td>
      <td style="text-align:left"><b>3 </b>
      </td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left"><b>VPN/User Roles</b>
      </td>
      <td style="text-align:left"><b>Admin, Deploy, ReadOnly</b>
      </td>
      <td style="text-align:left"><b>3</b>
      </td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left"><b>OS</b>
      </td>
      <td style="text-align:left"><b>Any Linux (preferably Ubuntu/RHEL)</b>
      </td>
      <td style="text-align:left"><b>All</b>
      </td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left"><b>Kubernetes as a managed service or VMs to provision Kubernetes</b>
      </td>
      <td style="text-align:left">
        <p><b>Managed Kubernetes service with HA/DRS</b>
        </p>
        <p><b>(Or) VMs with 2 vCore, 4 GB RAM, 20 GB Disk</b>
        </p>
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
      <td style="text-align:left"><b>Kubernetes worker nodes or VMs to provision Kube worker nodes.</b>
      </td>
      <td style="text-align:left"><b>VMs with 4 vCore, 16 GB RAM, 20 GB Disk / per env</b>
      </td>
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
      <td style="text-align:left"><b>Disk Storage (NFS/iSCSI) </b>
      </td>
      <td style="text-align:left"><b>Storage with backup, snapshot, dynamic inc/dec</b>
      </td>
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
      <td style="text-align:left"><b>Max throughput 1750 MB/s</b>
      </td>
      <td style="text-align:left"><b>1750 MS/s</b>
      </td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left"><b>Disk IOPS</b>
      </td>
      <td style="text-align:left"><b>Max throughput 1000 MB/s</b>
      </td>
      <td style="text-align:left"><b>1000 MB/s</b>
      </td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left"><b>Internet Speed</b>
      </td>
      <td style="text-align:left"><b>Min 100 MB - 1000MB/Sec (dedicated bandwidth)</b>
      </td>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left"><b>Public IP/NAT or LB</b>
      </td>
      <td style="text-align:left"><b>Internet-facing 1 public ip per env</b>
      </td>
      <td style="text-align:left"><b>3</b>
      </td>
      <td style="text-align:left"><b>3 Ips</b>
      </td>
    </tr>
    <tr>
      <td style="text-align:left"><b>Availability Region</b>
      </td>
      <td style="text-align:left"><b>VMs from the different region is preferable for the DRS/HA</b>
      </td>
      <td style="text-align:left"><b>at least 2 Regions</b>
      </td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left"><b>Private vLan</b>
      </td>
      <td style="text-align:left"><b>Per env all VMs should within private vLan/VPC</b>
      </td>
      <td style="text-align:left"><b>3</b>
      </td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left"><b>Gateways</b>
      </td>
      <td style="text-align:left"><b>NAT Gateway, Internet Gateway, Payment and SMS gateway</b>
      </td>
      <td style="text-align:left"><b>1 per env</b>
      </td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left"><b>Firewall</b>
      </td>
      <td style="text-align:left"><b>Ability to configure Inbound, Outbound ports/rules </b>
      </td>
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
        <p><b>Postgres 12 above Managed DB with backup, snapshot, logging. </b>
        </p>
        <p><b>(Or) 1 VM with 4 vCore, 16 GB RAM, 100 GB Disk per env.</b>
        </p>
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
        <p><b>Self Hosted Jenkins : Master, Slave (VM 4vCore, 8 GB each) </b>
        </p>
        <p><b>(Or) Managed CI/CD:  NIC DevOps or AWS  CodeDeploy or Azure DevOps </b>
        </p>
      </td>
      <td style="text-align:left"><b>2 VMs (Master, Slave)</b>
      </td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left"><b>Nexus Repo</b>
      </td>
      <td style="text-align:left"><b>Self hosted Artifactory Repo (Or) NIC Nexus Artifactory</b>
      </td>
      <td style="text-align:left"><b>1</b>
      </td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left"><b>DockerRegistry</b>
      </td>
      <td style="text-align:left"><b>DockerHub (Or) SelfHosted private docker reg</b>
      </td>
      <td style="text-align:left"><b>1</b>
      </td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left"><b>Git/SCM</b>
      </td>
      <td style="text-align:left"><b>GitHub (Or) Any Source Control tool</b>
      </td>
      <td style="text-align:left"><b>1</b>
      </td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left"><b>DNS </b>
      </td>
      <td style="text-align:left"><b>main domain &amp; ability to add more sub-domain</b>
      </td>
      <td style="text-align:left"><b>1</b>
      </td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left"><b>SSL Certificate</b>
      </td>
      <td style="text-align:left"><b>NIC managed (Or) SDC managed SSL certificate per URL</b>
      </td>
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

