# VPA: Step-by-Step

**What is Vertical Pod Autoscaler?**

Vertical Pod autoscaling (VPA) ensures that a container’s resources are not under- or over-utilized. It recommends optimized CPU and memory requests/limits values, and can also automatically update them for you so that the cluster resources are efficiently used.

Fig:- Vertical Pod Autoscaling

![](https://global-uploads.webflow.com/5d2dd7e1b4a76d8b803ac1aa/5f5619d41fdbf118d1b83121\_Vertical%20pod%20Autoscaling.png)

### **Architecture**

VPA consists of 3 components:

* **VPA admission controller**\
  ****Once you deploy and enable the Vertical Pod Autoscaler in your cluster, every pod submitted to the cluster goes through this webhook, which checks whether a VPA object is referencing it.
* **VPA recommender**\
  ****The recommender pulls the current and past resource consumption (CPU and memory) data for each container from metrics-server running in the cluster and provides optimal resource recommendations based on it, so that a container uses only what it needs.
* **VPA updater**\
  ****The updater checks at regular intervals if a pod is running within the recommended range. Otherwise, it accepts it for update, and the pod is evicted by the VPA updater to apply resource recommendation.\


### **Installation**

To install it manually follow below steps:

* Verify that the metrics-server deployment is running, or deploy it using instructions [here](https://github.com/kubernetes-sigs/metrics-server).

CODE: [https://gist.github.com/velotiotech/2b38b711e5e35802168443b447ce5900](https://gist.github.com/velotiotech/2b38b711e5e35802168443b447ce5900).js

* Also, verify the API below is enabled:

CODE: [https://gist.github.com/velotiotech/7609b41e20e60231aac6c2492bbfb44d](https://gist.github.com/velotiotech/7609b41e20e60231aac6c2492bbfb44d).js

* Clone the [kubernetes/autoscaler](https://github.com/kubernetes/autoscaler) GitHub repository, and then deploy the Vertical Pod Autoscaler with the following command.

CODE: [https://gist.github.com/velotiotech/d6fb0be829e747310de14f0324e6cf4b](https://gist.github.com/velotiotech/d6fb0be829e747310de14f0324e6cf4b).js\


Verify that the Vertical Pod Autoscaler pods are up and running:

CODE: [https://gist.github.com/velotiotech/a1c1ca0d5a3a8536938a7c35af219ab1](https://gist.github.com/velotiotech/a1c1ca0d5a3a8536938a7c35af219ab1).js

### **VPA using Resource Metrics**

**A. Setup: Create a Deployment and VPA resource**

Use the same deployment config to create a new deployment with _**"--vm-bytes", "850M"**_**.** Then create a VPA resource in Recommendation Mode with updateMode : Off

CODE: [https://gist.github.com/velotiotech/99d9368377626c8491040a02e6ce003b](https://gist.github.com/velotiotech/99d9368377626c8491040a02e6ce003b).js

* **minAllowed** is an optional parameter that specifies the minimum CPU request and memory request allowed for the container. 
* **maxAllowed** is an optional parameter that specifies the maximum CPU request and memory request allowed for the container.\


**B. Check the Pod’s Resource Utilization**

Check the resource utilization of the pods. Below, you can see only \~50 Mi memory is being used out of 1000Mi and only \~30m CPU out of 1000m. This clearly indicates that the pod resources are underutilized.

CODE: [https://gist.github.com/velotiotech/31f544044febaf3558794ca0bbb7287e](https://gist.github.com/velotiotech/31f544044febaf3558794ca0bbb7287e).js

If you describe the VPA resource, you can see the Recommendations provided. (It may take some time to show them.)

CODE: [https://gist.github.com/velotiotech/cdf95cfe43e033d32df25578466104b5](https://gist.github.com/velotiotech/cdf95cfe43e033d32df25578466104b5).js

**C. Understand the VPA recommendations**

**Target**: The recommended CPU request and memory request for the container that will be applied to the pod by VPA.

**Uncapped Target**: The recommended CPU request and memory request for the container if you didn’t configure upper/lower limits in the VPA definition. These values will not be applied to the pod. They’re used only as a status indication.

**Lower Bound**: The minimum recommended CPU request and memory request for the container. There is a --pod-recommendation-min-memory-mb flag that determines the minimum amount of memory the recommender will set—it defaults to 250MiB.

**Upper Bound**: The maximum recommended CPU request and memory request for the container.  It helps the VPA updater avoid eviction of pods that are close to the recommended target values. Eventually, the Upper Bound is expected to reach close to target recommendation.

CODE: [https://gist.github.com/velotiotech/d564f85bb52a39545d80326663eff416](https://gist.github.com/velotiotech/d564f85bb52a39545d80326663eff416).js

**D. VPA processing with Update Mode Off/Auto**\


Now, if you check the logs of vpa-updater, you can see it's not processing VPA objects as the Update Mode is set as Off.

CODE: [https://gist.github.com/velotiotech/afdf359d54ac22c94c283cbc72f294a5](https://gist.github.com/velotiotech/afdf359d54ac22c94c283cbc72f294a5).js

VPA allows various Update Modes, detailed [here](https://github.com/kubernetes/autoscaler/tree/master/vertical-pod-autoscaler#quick-start).

Let's change the VPA updateMode to “Auto” to see the processing.

As soon as you do that, you can see vpa-updater has started processing objects, and it's terminating all 3 pods.

CODE: [https://gist.github.com/velotiotech/97e8491ea7eded7230225404c404cfec](https://gist.github.com/velotiotech/97e8491ea7eded7230225404c404cfec).js

You can also check the logs of vpa-admission-controller:

CODE: [https://gist.github.com/velotiotech/2683d40c5449c38b8f7fa36dacf415d5](https://gist.github.com/velotiotech/2683d40c5449c38b8f7fa36dacf415d5).js

**NOTE:** Ensure that you have more than 1 running replicas. Otherwise, the pods won’t be restarted, and vpa-updater will give you this warning:

CODE: [https://gist.github.com/velotiotech/c329dda66ae95e1ef259c36e2d9ef097](https://gist.github.com/velotiotech/c329dda66ae95e1ef259c36e2d9ef097).js

Now, describe the new pods created and check that the resources match the Target recommendations:

CODE: [https://gist.github.com/velotiotech/a05c5adcd25bed54baef5e3196439d26](https://gist.github.com/velotiotech/a05c5adcd25bed54baef5e3196439d26).js

The Target Recommendation can not go below the minAllowed defined in the VPA spec.

Fig:- Prometheus: Memory Usage Ratio

![ Prometheus: Memory Usage Ratio](https://global-uploads.webflow.com/5d2dd7e1b4a76d8b803ac1aa/5f561016bc9949575159bbb3\_Kf7LMOXTmtPn_NrxsSIbsjL\_6baG11si32tl-SEMx8YgxrL71MP3LsYd-pV861V6MBq4BF7PBcaX0PsrW3WD_plKk-W0rfySEAZrm7-JHNCUh7B65CZV21-9cbqWw0dHZ3ByMw1T.png)

**E. Stress Loading Pods**

Let’s recreate the deployment with memory request and limit set to _**2000Mi**_ and _**"--vm-bytes", "500M"**_.

Gradually stress load one of these pods to increase its memory utilization.\
You can login to the pod and run _**stress --vm 1 --vm-bytes 1400M --timeout 120000s**_**.**

CODE: [https://gist.github.com/velotiotech/82c62a92355455571f3d82e3ccf1ca1f](https://gist.github.com/velotiotech/82c62a92355455571f3d82e3ccf1ca1f).js

Fig:- Prometheus memory utilized by each Replica

![Prometheus Memory Utilized by each Replica](https://global-uploads.webflow.com/5d2dd7e1b4a76d8b803ac1aa/5f5610151c27e75e343aa171\_sZaYX4DCUzcW-79NopfdKHHA8kokZOUnl9ichTO7CobBi0SccQ348MeQENlWCcXJi0Z_fpnM4NS1cLPJ06Ab4DjoAZ1zouZqtkGMH5fNrEXrrucEA5k0-qymn0KZSPKPglPDwx6U.png)

You will notice that the VPA recommendation is also calculated accordingly and applied to all replicas.

CODE: [https://gist.github.com/velotiotech/4f99a9a8c81d8116a4b5d7cd77926b56](https://gist.github.com/velotiotech/4f99a9a8c81d8116a4b5d7cd77926b56).js

**Limits v/s Request**\
****VPA always works with the requests defined for a container and not the limits. So, the VPA recommendations are also applied to the container requests, and it maintains a limit to request ratio specified for all containers.

For example, if the initial container configuration defines a 100m Memory Request and 300m Memory Limit, then when the VPA target recommendation is 150m Memory, the container Memory Request will be updated to 150m and Memory Limit to 450m.

### ****
