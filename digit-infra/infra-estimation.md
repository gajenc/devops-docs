# Infra Estimation

### DIGIT Infra Estimation based on the Modules.

DIGIT has several services that makes platform itself and also various product features that can be deployed based on the need, all of these services could be installed separately on the  Kubernetes cluster. Calculating needed CPU, Memory and Disk to provision the kubernetes cluster is based on selection of Business modules and the expected load (TPS) on any given env. Usually the Dev/UAT might have very less load compared to production, so number of services needed to be deployed on Dev, UAT would be same, but the number of replicas of the same services could be more in production. So calculate the infra accordingly. 

{% tabs %}
{% tab title="Overall Modules" %}
| **Service Category** | **Product Services (v2.0)**          |
| -------------------- | ------------------------------------ |
| **Backbone**         |                                      |
|                      | Redis                                |
|                      | Zookeeper                            |
|                      | nginx-ingress                        |
|                      | cert-manager                         |
|                      | ES-DATA-Infra (Add-on)               |
|                      | ES-MASTER-Infra (Add-on)             |
|                      | ES-DATA                              |
|                      | ES-MASTER                            |
|                      | Kafka                                |
|                      | Kafka-Infra (Addon)                  |
|                      | PostGres DB                          |
| **Infra **           |                                      |
|                      | Jaeger (Addon)                       |
|                      | Kibana-Infra (Addon)                 |
|                      | Kibana-Product                       |
|                      | minio                                |
|                      | Grafana                              |
|                      | Alert manager                        |
|                      | Prometheus                           |
|                      | WordPress Portal                     |
| Core                 |                                      |
|                      | egov-enc-service                     |
|                      | egov-searcher                        |
|                      | egov-pg-service                      |
|                      | egov-filestore                       |
|                      | zuul                                 |
|                      | egov-notification-mail               |
|                      | egov-notification-sms                |
|                      | egov-localization                    |
|                      | egov-persister                       |
|                      | egov-idgen                           |
|                      | egov-user                            |
|                      | egov-mdms-service                    |
|                      | egov-url-shortening                  |
|                      | egov-indexer                         |
|                      | report                               |
|                      | egov-workflow-v2                     |
|                      | egov-user-event                      |
|                      | pdf-service                          |
|                      | egov-pdf                             |
|                      | chatbot                              |
|                      | egov-accesscontrol                   |
|                      | egov-location                        |
|                      | egov-otp                             |
|                      | egov-custom-consumer                 |
|                      | user-otp                             |
| **Business**         |                                      |
|                      | egov-apportion-service               |
|                      | collection-services                  |
|                      | billing-service                      |
|                      | egov-hrms                            |
|                      | dashboard-analytics                  |
|                      | dashboard-ingest                     |
|                      | egf-instrument                       |
|                      | egf-master                           |
|                      | finance-collections-voucher-consumer |
| **Municipal**        |                                      |
|                      | tl-services                          |
|                      | tl-calculator                        |
|                      | firenoc-services                     |
|                      | firenoc-calculator                   |
|                      | rainmaker-pgr                        |
|                      | property-services                    |
|                      | pt-calculator-v2                     |
|                      | pt-services-v2                       |
|                      | ws-services                          |
|                      | ws-calculator                        |
|                      | sw-services                          |
|                      | sw-calculator                        |
|                      | bpa-calculator                       |
|                      | bpa-services                         |
|                      | land-services                        |
| **Containerized**    |                                      |
|                      | FINANCE - Containerized              |
|                      | egov-edcr - Containerized            |
| **Frontend**         |                                      |
|                      | dss-dashboard                        |
|                      | Trade License                        |
|                      | Bill Genie                           |
|                      | Universal Collections                |
|                      | PGR                                  |
|                      | FIRENOC                              |
|                      | mSeva                                |
|                      | Property Tax                         |
|                      | HRMS                                 |
|                      | W\&S                                 |
|                      | BPA                                  |
|                      | Employee App                         |
|                      | Citizen App                          |
{% endtab %}

{% tab title="H/W Estimation" %}


| **CPU CALCULATION**   | **Category**       | **No of Services** | **MIN (vCores)** |
| --------------------- | ------------------ | ------------------ | ---------------- |
| @0.2 core/per service | Backbone Services  | 27                 | 5.4              |
| @0.1 core/per service | Infra Services     | 7                  | 0.7              |
| @0.1 core/per service | Core Services      | 27                 | 2.7              |
| @0.1 core/per service | Business Services  | 7                  | 0.7              |
| @0.2 core/per service | Municipal Services | 15                 | 5.5              |
| @2 core/Per service   | Containerized      | 2                  | 4                |
| @0.1 core/per service | Frontend           | 12                 | 1.2              |
|                       | **TOTAL**          | **97**             | **20**           |

| **RAM CALCULATION** | **Category**       | **No of Services** | **MIN (GB)** |
| ------------------- | ------------------ | ------------------ | ------------ |
| @512mb/per service  | Backbone Services  | 27                 | 13.5         |
| @512mb/per service  | Infra Services     | 7                  | 3.5          |
| @512mb/per service  | Core Services      | 27                 | 16.2         |
| @512mb/per service  | Business Services  | 7                  | 3.5          |
| @512mb/per service  | Municipal Services | 15                 | 7.5          |
| @4GB/Per service    | Containerized      | 2                  | 8            |
| @256mb/per service  | Frontend           | 12                 | 3            |
|                     | **TOTAL**          | **97**             | **55**       |

| DISK CALCULATION  | Category           | **No of Services** | MIN      |
| ----------------- | ------------------ | ------------------ | -------- |
| @50GB/Statefulset | Backbone Services  | 27                 | 1350     |
|                   | Infra Services     | 7                  | 0        |
|                   | Core Services      | 27                 | 0        |
|                   | Business Services  | 7                  | 0        |
| @50GB/Services    | Municipal Services | 15                 | 750      |
| @100GB Filestore  | Containerized      | 2                  | 200      |
|                   | Frontend           | 12                 | 0        |
|                   | **TOTAL**          | **97**             | **2300** |
{% endtab %}
{% endtabs %}

