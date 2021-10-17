---
description: Kubernetes autoscaling Using HPA and VPA
---

# DIGIT - Infra Autoscaling

Autoscaling, a key feature of Kubernetes, it helps improving the resource utilization of the cluster by automatically adjusting the application’s resources or replicas depending on the load at that time.

Here we'll understand about Pod Autoscaling in DIGIT Infra and how to setup and configure autoscalers to optimize the resource utilization of services in DIGIT.

DIGIT is deployed and orchestrated on Kubernetes because of many key strengths, One of the strengths of Kubernetes as a container orchestrator lies in its ability to manage and respond to dynamic environments. One example is Kubernetes’ native capability to perform effective autoscaling of resources. 

Kubernetes offer 3 ways to scale the infra based on the dynamic workloads using the custom metrics target a marker of pod usage like CPU usage, such as network traffic, memory, or a value relating to a service running inside a pod itself or an external metrics measure values that do not correlate to a pod. For example, an external metric could track the number of Incoming traffic TPS.

1. **Horizontal Pod Autoscaling (HPA)**
2. **Vertical Pod Autoscaling (VPA)**
3. **Cluster Autoscaler**

## 1. **Horizontal Pod Autoscaling**

For many DIGIT services with usage that varies over time, you may want to add or remove pod replicas in response to changes in demand for those specific workloads. The [Horizontal Pod Autoscaler (HPA)](https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/) can manage scaling these workloads automatically.

**Use Cases: **The HPA is ideal for scaling stateless applications, although it can also support scaling stateful sets. Using HPA in combination with cluster autoscaling (see below) can help you achieve resource optimization/cost savings for workloads that see regular changes in demand by reducing the number of active nodes as the number of pods decreases.

**How It Works:** For workloads with HPA configured, the HPA controller monitors the workload’s pods to determine if it needs to change the number of pod replicas. In most cases, where the controller takes the mean of a **per-pod metric value**, [it calculates whether adding or removing replicas would move the current value closer to the target value](https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/#algorithm-details). Eg: If a  _**property-service**_ have a target CPU utilization of 50%. If 2 pod replicas are currently running and the mean CPU utilization is 75%, the controller will add 2 replicas to move the pod average closer to 50%.

**How to Use It:  **To configure the HPA controller to manage a workload, create a `HorizontalPodAutoscaler` object. HPA can also be configured with the [`kubectl autoscale`](https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale-walkthrough/#create-horizontal-pod-autoscaler) subcommand.

```
apiVersion: autoscaling/v1
kind: HorizontalPodAutoscaler
metadata:
  name: property-service
  namespace: digit
spec:
  maxReplicas: 10
  minReplicas: 1
  scaleTargetRef:
    apiVersion: extensions/v1beta1
    kind: Deployment
    name: property-service
  targetCPUUtilizationPercentage: 50
```

This manifest would configure the HPA controller to monitor the CPU utilization of pods in the `property-service` deployment. The controller could add or remove pods, keeping a minimum of one and a maximum of ten, to target a mean CPU utilization of 50%.

**Prerequisite**: 

1. The HPA controller runs, by default, as part of the standard `kube-controller-manager` daemon. It can manage only those pods created by a replication controller, such as deployments, replica sets, or stateful sets.
2. The controller requires a metrics source. For scaling based on CPU usage, it depends on the [`metrics-server`](https://github.com/kubernetes-sigs/metrics-server). Scaling based on custom or external metrics requires deploying a service that implements the `custom.metrics.k8s.io` or `external.metrics.k8s.io` API to provide an interface with the monitoring service or alternate metrics source.
3. For workloads using the standard CPU metric, containers **must have CPU resource limits **configured in the **pod spec**.
4. Verify that the metrics-server is already deployed and running using the command below, or deploy it using instructions [here](https://github.com/kubernetes-sigs/metrics-server).

## 2. Vertical Pod Autoscaling

The default Kubernetes scheduler overcommits CPU and memory reservations on a node, with the philosophy that most containers will stick closer to their initial requests than to their requested upper limit. The [Vertical Pod Autoscaler (VPA)](https://github.com/kubernetes/autoscaler/tree/master/vertical-pod-autoscaler) can increase and decrease the CPU and memory resource requests of pod containers to better match the allocated cluster resource allotment to actual usage.

**Use Cases: **The VPA service can set container resource limits based on live data, rather than human guesswork or benchmarks that are only occasionally run.

Alternatively, some workloads may be prone to occasional periods of very high utilization, but permanently increasing their request limits would waste mostly unused CPU or memory resources and limit the nodes that can run them. While Horizontal Pod Autoscaling can help in many of those cases, sometimes the workload cannot easily be spread across multiple instances of an application.

**How It Works:** A VPA deployment has three components: [Recommender](https://github.com/kubernetes/autoscaler/blob/master/vertical-pod-autoscaler/pkg/recommender/README.md), which monitors resource utilization and computes target values; [Updater](https://github.com/kubernetes/autoscaler/blob/master/vertical-pod-autoscaler/pkg/updater/README.md), which evicts pods that need to be updated with new resource limits, and the [Admission Controller](https://github.com/kubernetes/autoscaler/blob/master/vertical-pod-autoscaler/pkg/admission-controller/README.md), which uses a [mutating admission webhook](https://kubernetes.io/docs/reference/access-authn-authz/admission-controllers/#mutatingadmissionwebhook) to overwrite the resource requests of pods at creation time.

Because Kubernetes does not support dynamically changing the resource limits of a running pod, the VPA cannot update existing pods with new limits. It terminates pods that are using outdated limits, and when the pod’s controller requests the replacement from the Kubernetes API service, the VPA admission controller injects the updated resource request and limit values into the new pod’s specification.

VPA can also be run in recommendation mode only. In this mode, the VPA Recommender will update the `status` field of the workload’s `VerticalPodAutoscaler` resource with its suggested values but will not terminate pods or alter pod API requests.

**How To Use It:** If your Kubernetes provider does not support VPA as an add-on, you can [install it](https://github.com/kubernetes/autoscaler/tree/master/vertical-pod-autoscaler#installation) in your cluster directly.

VPA uses the custom resource `VerticalPodAutoscaler` to configure the scaling for a deployment or replica set. If I wanted VPA to manage resource requests for my `hello-world` deployment, I might apply this manifest:

```
apiVersion: "autoscaling.k8s.io/v1beta2"
kind: VerticalPodAutoscaler
metadata:
  name: property-service
spec:
  targetRef:
    apiVersion: "apps/v1"
    kind: Deployment
    name: property-service
  resourcePolicy:
    containerPolicies:
      - containerName: '*'
        controlledResources: ["cpu", "memory"]
        minAllowed:
          cpu: 200m
          memory: 50Mi
        maxAllowed:
          cpu: 500m
          memory: 500Mi
  updatePolicy:
    updateMode: "Auto"
```

Now VPA will watch the pods in my deployment and try to set the appropriate resource value, between 200 and 500 milliCPUss with between 50 and 500 mebibytes of memory. If VPA computes new resource values for the deployment’s pods, it will evict existing pods sequentially and update the replacements with the new values.

**Prerequisite:**

1. VPA can replace only pods managed by a replication controller, such as deployments. It requires the Kubernetes [`metrics-server`](https://github.com/kubernetes-sigs/metrics-server).
2. VPA and HPA should only be used simultaneously to manage a given workload if the HPA configuration does not use CPU or memory to determine scaling targets.
3. VPA also has some [other limitations and caveats](https://github.com/kubernetes/autoscaler/tree/master/vertical-pod-autoscaler#known-limitations).
4. These autoscaling options demonstrate a small but powerful piece of the flexibility of Kubernetes. Using one or more forms of autoscaling removes a good deal of the overhead in managing dynamic production environments and improves efficiency of infrastructure utilization.\
   \




## **Conclusion**

Both the Horizontal Pod Autoscaler and the Vertical Pod Autoscaler serve different purposes and one can be more useful than the other depending on your application’s requirement.

The HPA can be useful when, for example, your application is serving a large number of lightweight (low resource-consuming) requests. In that case, scaling number of replicas can distribute the workload on each of the pod. The VPA, on the other hand, can be useful when your application serves heavyweight requests, which requires higher resources.

