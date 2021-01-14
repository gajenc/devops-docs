---
description: >-
  This document covers the minimal hardware recommendations for the DIGIT
  Platform deployed on Kubernetes cluster.
---

# Infra Recommendation

DIGIT urges to take advantage of managed kubernetes services like EKS offered by Amazon, AKS offered by Microsoft, GKE offered by GKE and RKE offered by NIC \(GI Cloud\). But it never stops you to create your own kubernetes cluster from any of your available existing infrastructure.

In case of managed services most of the operations are taken care, where as in the manual provisioning apart from managing just the application workload you may have to put additional efforts to maintain your own cluster from the elasticity, HA, DRS point of view.

## Hardware Recommendations

### Master Node VM Estimation <a id="kublr-platform-feature-requirements"></a>

**Dev & UAT Env:** You can have minimal Single master Kubernetes cluster with 4GB memory and 2 CPU. Please note: We do not recommend using this configuration in production but this configuration is suitable to start exploring the DIGIT Platform.

**Prod Env:** For a production workload, run the cluster on HA, DRS mode, so run the kubernetes cluster on 3 master nodes. 4 x 3 GB memory and 2 x3 CPU.

| Specification | Required CPU | Required memory |
| :--- | :--- | :--- |
| Minimum Master Node | 2 vCPUs | 4 GB |
| **Production Master Node** | 2x3 vCPUs | 4x3 GB |

### Worker Node VM Estimation <a id="kublr-platform-feature-requirements"></a>

| DIGIT Platform/Feature | Required CPU | Required memory |
| :--- | :--- | :--- |
| Feature: Core Platform/Infra Services | 10 vCPUs | 40 GB |
| Feature: Municipal Services \(Varies on modules\) | 4 - 20 vCPUs | 4 - 40 GB |
| Feature: Centralized monitoring/Logging/Tracing | 2 vCPUs | 6 GB |
| Feature: k8s core components | 1 vCPUs | 2 GB |
| **TOTAL** | 17 - 37 vCPUs | 52 - 90GB RAM |

### VM Types Examples based on above specs: <a id="kublr-platform-deployment-example"></a>

| Provider | Master Instance Type | Worker Instance Type |
| :--- | :--- | :--- |
| Amazon Web Services | t2.large/t3.large \(2 vCPU, 4GB\) | 3× t2\(t3\) xlarge \(4 vCPU, 16GB\) |
| Google Cloud Platform | n1-standard-2 \(2 vCPU, 7.5GB\) | 3 × n1-standard-4 \(4 vCPU, 15GB\) |
| Microsoft Azure | A2 v2 \(2 vCPU, 4GB\) | 3 × A8 v2 \(8 vCPU, 16GB\) |
| On-Premises | 2 vCPU, 5GB | 3 × VM \(3 vCPU, 16GB\) |

### DIGIT Infra Estimation based on the Modules.

DIGIT has several product features, which could be installed separately on a provisioned Kubernetes clusters. Calculating Needed CPU, Memory and Disk for Business modules as per the below details.

{% tabs %}
{% tab title="Overall Modules" %}
| **Service Category** | **Product Services \(v2.0\)** |
| :--- | :--- |
| **Backbone** |  |
|  | Redis |
|  | Zookeeper |
|  | nginx-ingress |
|  | cert-manager |
|  | ES-DATA-Infra \(Add-on\) |
|  | ES-MASTER-Infra \(Add-on\) |
|  | ES-DATA |
|  | ES-MASTER |
|  | Kafka |
|  | Kafka-Infra \(Addon\) |
|  | PostGres DB |
| **Infra**  |  |
|  | Jaeger \(Addon\) |
|  | Kibana-Infra \(Addon\) |
|  | Kibana-Product |
|  | minio |
|  | Grafana |
|  | Alert manager |
|  | Prometheus |
|  | WordPress Portal |
| Core |  |
|  | egov-enc-service |
|  | egov-searcher |
|  | egov-pg-service |
|  | egov-filestore |
|  | zuul |
|  | egov-notification-mail |
|  | egov-notification-sms |
|  | egov-localization |
|  | egov-persister |
|  | egov-idgen |
|  | egov-user |
|  | egov-mdms-service |
|  | egov-url-shortening |
|  | egov-indexer |
|  | report |
|  | egov-workflow-v2 |
|  | egov-user-event |
|  | pdf-service |
|  | egov-pdf |
|  | chatbot |
|  | egov-accesscontrol |
|  | egov-location |
|  | egov-otp |
|  | egov-custom-consumer |
|  | user-otp |
| **Business** |  |
|  | egov-apportion-service |
|  | collection-services |
|  | billing-service |
|  | egov-hrms |
|  | dashboard-analytics |
|  | dashboard-ingest |
|  | egf-instrument |
|  | egf-master |
|  | finance-collections-voucher-consumer |
| **Municipal** |  |
|  | tl-services |
|  | tl-calculator |
|  | firenoc-services |
|  | firenoc-calculator |
|  | rainmaker-pgr |
|  | property-services |
|  | pt-calculator-v2 |
|  | pt-services-v2 |
|  | ws-services |
|  | ws-calculator |
|  | sw-services |
|  | sw-calculator |
|  | bpa-calculator |
|  | bpa-services |
|  | land-services |
| **Containerized** |  |
|  | FINANCE - Containerized |
|  | egov-edcr - Containerized |
| **Frontend** |  |
|  | dss-dashboard |
|  | Trade License |
|  | Bill Genie |
|  | Universal Collections |
|  | PGR |
|  | FIRENOC |
|  | mSeva |
|  | Property Tax |
|  | HRMS |
|  | W&S |
|  | BPA |
|  | Employee App |
|  | Citizen App |
{% endtab %}

{% tab title="H/W Estimation" %}


| **CPU CALCULATION** | **Category** | **No of Services** | **MIN \(vCores\)** |
| :--- | :--- | :--- | :--- |
| @0.2 core/per service | Backbone Services | 27 | 5.4 |
| @0.1 core/per service | Infra Services | 7 | 0.7 |
| @0.1 core/per service | Core Services | 27 | 2.7 |
| @0.1 core/per service | Business Services | 7 | 0.7 |
| @0.2 core/per service | Municipal Services | 15 | 5.5 |
| @2 core/Per service | Containerized | 2 | 4 |
| @0.1 core/per service | Frontend | 12 | 1.2 |
|  | **TOTAL** | **97** | **20** |

| **RAM CALCULATION** | **Category** | **No of Services** | **MIN \(GB\)** |
| :--- | :--- | :--- | :--- |
| @512mb/per service | Backbone Services | 27 | 13.5 |
| @512mb/per service | Infra Services | 7 | 3.5 |
| @512mb/per service | Core Services | 27 | 16.2 |
| @512mb/per service | Business Services | 7 | 3.5 |
| @512mb/per service | Municipal Services | 15 | 7.5 |
| @4GB/Per service | Containerized | 2 | 8 |
| @256mb/per service | Frontend | 12 | 3 |
|  | **TOTAL** | **97** | **55** |

| DISK CALCULATION | Category | **No of Services** | MIN |
| :--- | :--- | :--- | :--- |
| @50GB/Statefulset | Backbone Services | 27 | 1350 |
|  | Infra Services | 7 | 0 |
|  | Core Services | 27 | 0 |
|  | Business Services | 7 | 0 |
| @50GB/Services | Municipal Services | 15 | 750 |
| @100GB Filestore | Containerized | 2 | 200 |
|  | Frontend | 12 | 0 |
|  | **TOTAL** | **97** | **2300** |
{% endtab %}
{% endtabs %}



