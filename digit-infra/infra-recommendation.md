---
description: >-
  This document covers the minimal hardware recommendations for the DIGIT
  Platform deployed on Kubernetes cluster.
---

# Infra Recommendation

Primary Infra to setup DIGIT is Kubernetes cluster. We urges to take advantage of managed kubernetes clusters from the commercial clouds like **Amazon**, **Microsoft**, **Google, others** and also **GI Cloud - NIC,** since majority of these cloud providers can offer you an almost-instant, scalable cluster on demand, but there’s a trick or two you can pull while leveraging SDC's architecture. One of the reasons state might want to deploy their own Kubernetes cluster is because states already have some on-prem or collocated bare-metal hardware. Another is the flexibility of state data center architecture. Price also comes to mind if you already own metal in your racks. 

Deploying an actual kubernetes cluster is relatively easy and quick; based on how it will fit into your existing infrastructure. Most of the tutorials, books, and blogs around will either presuppose or advise you to create a cluster with some of the major cloud providers — Google, Amazon, Microsoft,etc. But when a state decides to setup kubernetes on state data center leveraging either full virtualization (HCI, KVM and Hyper-V) or containerization (LXC), a full-gigabit uplink, and a free /25 public IPv4 pool, meaning state can practically provision for whatever demand the Kubernetes cluster should throw at us.

In SDC's case one should consider the practical implications and demands for real-life challenges one might want to convert to Kubernetes architecture. As important as deciding what makes sense to containerize is deciding what not to containerize — and how to make it play nice in SDCs hybrid landscape.

These are the major requirement we should consider:

* network topology and connectivity
* compute resources
* storage technology
* non-volatile services
* Elasticity
* HA/DRS
* Speed
* Monitoring/Alerting
* Data backups/recovery
* Operations/SLA/Turnaround time

In case of managed services most of these operations are taken care, where as in the manual provisioning apart from managing just the application workload you may have to put additional efforts to maintain your own cluster considering the above aspects.

## Hardware Recommendations

### Master Node VM Estimation <a href="kublr-platform-feature-requirements" id="kublr-platform-feature-requirements"></a>

**Dev & UAT Envs:** You can have minimal Single master Kubernetes cluster with **4GB** memory and **2 vCPU**. Please note: We do not recommend using this configuration in production but this configuration is suitable to start exploring the DIGIT Platform.

**Prod Env:** For a production workload, run the cluster on HA, DRS mode, so run the kubernetes cluster on **3 master nodes quorum**. Which is **3 x 4 GB** memory and **3 x 2 vCPU**.

| Specification              | Required CPU | Required memory |
| -------------------------- | ------------ | --------------- |
| Minimum Master Node        | 2 vCPUs      | 4 GB            |
| **Production Master Node** | 2x3 vCPUs    | 4x3 GB          |

### Worker Node VM Estimation <a href="kublr-platform-feature-requirements" id="kublr-platform-feature-requirements"></a>

Though there are Master nodes, we recommend you only use them only for managing the kubernetes cluster components, to schedule the actual application workloads you need to provision the Worker nodes depending upon the number of services/modules that you are going to implement and you can always add more worker nodes as you increase the capacity incrementally. 

Since DIGIT as a platform has many services like core/infra which also varies based on the municipal services that a state want to implement either phase-by-phase or ulb-after-ulb. So the hardware requirement for the worker nodes will vary based on the modules that state want to implement and also the env type like Dev, UAT and PROD. Below is the high level estimation on how it would vary between the range, however you can arrive at the estimation specific to a states need based on the similar [estimation details](infra-estimation.md) mentioned.

| DIGIT Platform/Feature                          | Required CPU  | Required memory |
| ----------------------------------------------- | ------------- | --------------- |
| Feature: Core Platform/Infra Services           | 10 vCPUs      | 40 GB           |
| Feature: Municipal Services (Varies on modules) | 4 - 20 vCPUs  | 4 - 40 GB       |
| Feature: Centralized monitoring/Logging/Tracing | 2 vCPUs       | 6 GB            |
| Feature: k8s core components                    | 1 vCPUs       | 2 GB            |
| **TOTAL**                                       | 17 - 37 vCPUs | 52 - 90GB RAM   |

### VM Types Examples based on above specs: <a href="kublr-platform-deployment-example" id="kublr-platform-deployment-example"></a>

| Provider              | Master Instance Type            | Worker Instance Type             |
| --------------------- | ------------------------------- | -------------------------------- |
| Amazon Web Services   | t2.large/t3.large (2 vCPU, 4GB) | 3× t2(t3) xlarge (4 vCPU, 16GB)  |
| Google Cloud Platform | n1-standard-2 (2 vCPU, 7.5GB)   | 3 × n1-standard-4 (4 vCPU, 15GB) |
| Microsoft Azure       | A2 v2 (2 vCPU, 4GB)             | 3 × A8 v2 (8 vCPU, 16GB)         |
| On-Premises           | 2 vCPU, 5GB                     | 3 × VM (3 vCPU, 16GB)            |

