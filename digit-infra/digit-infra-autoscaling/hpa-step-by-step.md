# HPA: Step-by-Step

**What is the Horizontal Pod Autoscaler?**

The Horizontal Pod Autoscaler (HPA) scales the number of pods of a replica-set/ deployment/ statefulset based on per-pod metrics received from resource metrics API (metrics.k8s.io) provided by metrics-server, the custom metrics API (custom.metrics.k8s.io), or the external metrics API (external.metrics.k8s.io).

Fig:- Horizontal Pod Autoscaling

![Horizontal Pod Autoscaling](https://global-uploads.webflow.com/5d2dd7e1b4a76d8b803ac1aa/5f5617f11c3d8e1aeff818c9\_Horizontal%20Pod%20Autoscaling.png)

## ‍**Prerequisite** 

Verify that the metrics-server is already deployed and running using the command below, or deploy it using instructions [here](https://github.com/kubernetes-sigs/metrics-server).

CODE: [https://gist.github.com/velotiotech/199ac1a38a3ab0e3a07a043ef1daecca](https://gist.github.com/velotiotech/199ac1a38a3ab0e3a07a043ef1daecca).js

### **HPA using Multiple Resource Metrics**‍

HPA fetches per-pod resource metrics (like CPU, memory) from the resource metrics API and calculates the current metric value based on the mean values of all targeted pods. It compares the current metric value with the target metric value specified in the HPA spec and produces a ratio used to scale the number of desired replicas.\


**A. Setup: Create a Deployment and HPA resource**

In this blog post, I have used the config below to create a deployment of 3 replicas, with some memory load defined by “_**--vm-bytes", "850M”.**_

CODE: [https://gist.github.com/velotiotech/6a03575aa9153f04efe764133f3f4a6b](https://gist.github.com/velotiotech/6a03575aa9153f04efe764133f3f4a6b).js

**NOTE: It’s recommended not to use HPA and VPA on the same pods or deployments.**

CODE: [https://gist.github.com/velotiotech/76948daf9c35a4744d78972a1293aa3f**.**](https://gist.github.com/velotiotech/76948daf9c35a4744d78972a1293aa3f)js

Lets create an HPA resource for this deployment with multiple metric blocks defined. The HPA will consider each metric one-by-one and calculate the desired replica counts based on each of the metrics, and then select the one with the highest replica count.

CODE: [https://gist.github.com/velotiotech/b2987ff6717591d1925ffbfce925d3e8](https://gist.github.com/velotiotech/b2987ff6717591d1925ffbfce925d3e8).js\


* We have defined the minimum number  of replicas HPA can scale down to as 1 and the maximum number that it can scale up to as 10.
* **Target Average Utilization** and **Target Average Values** implies that the HPA should scale the replicas up/down to keep the **Current Metric Value** equal or closest to **Target Metric Value**.\


**B. Understanding the HPA Algorithm**

CODE: [https://gist.github.com/velotiotech/b592175a41a52bf0cdc44d6b1ff1b79a](https://gist.github.com/velotiotech/b592175a41a52bf0cdc44d6b1ff1b79a).js\


* HPA calculates pod utilization as total usage of all containers in the pod divided by total request. It looks at all containers individually and returns if container doesn't have request.
* The calculated  Current Metric Value for memory, i,e., 894188202666m, is higher than the Target Average Value of 500Mi, so the replicas need to be scaled up.
* The calculated  Current Metric Value for CPU i.e., 36%, is lower than the Target Average Utilization of 50, so  hence the replicas need to be scaled down.
* Replicas are calculated based on both metrics and the highest replica count selected. So, the replicas are scaled up to 6 in this case.\


### **HPA using Custom metrics**

We will use the prometheus-adapter resource to expose custom application metrics to **custom.metrics.k8s.io/v1beta1**, which are retrieved by HPA. By defining our own metrics through the adapter’s configuration, we can let HPA perform scaling based on our custom metrics.\


**A.** **Setup: Install Prometheus Adapter**

Create prometheus-adapter.yaml with the content below:

CODE: [https://gist.github.com/velotiotech/7323ef8969bfeb40929b0d1205518642](https://gist.github.com/velotiotech/7323ef8969bfeb40929b0d1205518642).js

CODE: [https://gist.github.com/velotiotech/acffd596f1992431c283699aa3292f23](https://gist.github.com/velotiotech/acffd596f1992431c283699aa3292f23).js

Once the charts are deployed, verify the metrics are exposed at v1beta1.custom.metrics.k8s.io:

CODE: [https://gist.github.com/velotiotech/5b4297bc2d38b4d993d82541ec6f2685](https://gist.github.com/velotiotech/5b4297bc2d38b4d993d82541ec6f2685).js

You can see the metrics value of all the replicas in the output.\


**B. Understanding Prometheus Adapter Configuration**

The adapter considers metrics defined with the parameters below:

1\. **seriesQuery** tells the Prometheus Metric name to the adapter**‍**

2\. **resources** tells which Kubernetes resources each metric is associated with or which labels does the metric include, e.g., namespace, pod etc.

3\. **metricsQuery** is the actual Prometheus query that needs to be performed to calculate the actual values.

4\. **name** with which the metric should be exposed to the custom metrics API

For instance, if we want to calculate the rate of container_network_receive_packets_total, we will need to write this query in Prometheus UI:

_**sum(rate(container_network_receive_packets_total{namespace="autoscale-tester",pod=\~"autoscale-tester.\*"}\[10m])) by (pod)**_

This query is represented as below in the adapter configuration:

_**metricsQuery: 'sum(rate(<<.series>>{<<.labelmatchers>>}10m])) by (<<.groupby>>)'**_

**C. Create an HPA resource**

Now, let's create an HPA resource with the pod metric _**packets_in**_ using the config below, and then describe the HPA resource.

CODE: [https://gist.github.com/velotiotech/06bc056955cf524784e006b8cca0efff](https://gist.github.com/velotiotech/06bc056955cf524784e006b8cca0efff).js

CODE: [https://gist.github.com/velotiotech/c7782cc7e2c3dbe9f31bd896d5232843](https://gist.github.com/velotiotech/c7782cc7e2c3dbe9f31bd896d5232843).js\


Here, the current calculated metric value is 18666m. The _m_ represents milli-units. So, for example, 18666m means 18.666 which is what we expect ((33 + 11 + 10 )/3 = 18.666). Since it's less than the target average value (i.e., 50), the HPA scales down the replicas to make the **Current Metric Value : Target Metric Value** ratio closest to 1. Hence, replicas are scaled down to 2 and later to 1.\


Fig:- container_network_receive_packets_total

![Fig.2 Prometheus :container_network_receive_packets_total{namespace=”autoscale-tester}](https://global-uploads.webflow.com/5d2dd7e1b4a76d8b803ac1aa/5f560fb2626d8c2935312c51\_FmGichc2be902-VKufgEKUvWpr_bb8VniAmKJmQ7hm3a8aJQgXumTqYvluY3W6DCt4XVPw7hYuDxS1h9\_5S4rUdQLete\_3GK3gZuZQ960vUkvKNrEoQK84WGNHl2Fr1el81-PxKJ.png)

Fig:- Ratio to Target value\


![Prometheus: Ratio of avg(container_network_receive_packets_total{namespace=”autoscale-tester}) :Target Average Value](https://global-uploads.webflow.com/5d2dd7e1b4a76d8b803ac1aa/5f560fb1bc9949dac9599e12\_HfpdOdwYsqov5xpdPY-IS8UMvA_EYuxI-UChEyd-cTeDvw_Azz6hAMDfjNsldAUACj3xQSaZYx-03XOOJuzGXxpKmUfLNVKZxokgCQvH2AIs4dYxIslj8A7uaGXxnU9R7I57qyCx.png)

## ‍**Selective Container Scaling**

If you have a pod with multiple containers and you want to opt-out some of them, you can use the "Off" mode to turn off recommendations for a container.

You can also set containerName: "\*" to include all containers.

CODE: [https://gist.github.com/velotiotech/af352b64443e1198575c0c377af0d668](https://gist.github.com/velotiotech/af352b64443e1198575c0c377af0d668).js

## ****
