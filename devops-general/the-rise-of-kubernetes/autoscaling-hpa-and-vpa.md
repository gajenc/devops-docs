# Autoscaling (HPA & VPA)

Auto Scaling works based on the metrics set 

Controller Manager compare the metrics received from Metrics server API in regular intervals and scales the deployment once the defined threshold is crossed. HPA is a control loop as defined by the parameter `--horizontal-pod-autoscaler-sync-period( in our case it's 10 seconds, by default it's 15 seconds)`



HPA Architecture

![](<../../.gitbook/assets/image (7).png>)

In this post , we will see as how we can scale Kubernetes pods using Horizontal Pod Autoscaler(HPA) based on CPU and Memory. Support for scaling on memory and custom metrics, can be found in `autoscaling/v2beta2`

We will see as how HPA can be implemented on Minikube .

**Step-1 : Enable Minikube with the following settings**

_minikube start — extra-config=controller-manager.horizontal-pod-autoscaler-upscale-delay=1m — extra-config=controller-manager.horizontal-pod-autoscaler-downscale-delay=1m — extra-config=controller-manager.horizontal-pod-autoscaler-sync-period=10s — extra-config=controller-manager.horizontal-pod-autoscaler-downscale-stabilization=1m_

In our deployment we will override the default values as above , so we can observe the autoscale and downscale bit early as compared to the default values.

Minikube with settings enabled

_minikube add-ons enable metrics-server_

Minikube with metrics-server enabled

**Step-2 : Build the docker image and push it to the repo**

_sudo docker build -t vamsijakkula/php-hpa ._

_sudo docker push vamsijakkula/php-hpa_

This image is a sample php deployment which has some basic compute intensive tasks. For details, please refer to the source code given below.

**Step-3 : Deploy the manifest **_**kubectl create -f php-apache.yml**_

PHP Deployment

**Step-4: Create HPA( CPU Utilization)— kubectl create -f hpa.yml**

We can even define a HPA as _kubectl autoscale deployment php-apache — cpu-percent=50 — min=1 — max=10_

For HPA we are defining to auto-scale based on CPU Utilization of 50 % and max replicas to be 10 pods and min to be 1 post downscale. Pods will be downscaled after a stabilization period of 1m.

HPA Deployment

**Step-5: Generate Load , so the pods will scale based on threshold and down-scale after a min**.

_kubectl run — generator=run-pod/v1 -it — rm load-generator — image=busybox /bin/sh_\
_while true; do wget -q -O-_ [_http://php-apache_](http://php-apache)_; done_

Load Generation

**Step-6 : Verify HPA and Deployment**

![Image for post](https://miro.medium.com/max/60/1\*hDCWOfEC0W1PuP2QAj6IVw.png?q=20)

HPA Threshold Increase

Pods Auto-scaled based on CPU Utilization from 1 to 4 and it’s in increasing mode.

![Image for post](https://miro.medium.com/max/60/1\*ftu9oHJ-q368ZmRZRFIIpw.png?q=20)

**Step:7 Stop the load generation process and watch the down-scale of the pods**.

All the pods got downscaled to 1 and CPU Utilization came down as well.

![Image for post](https://miro.medium.com/max/60/1\*MSdgiSZXV2Jj9RsKRKXU3Q.png?q=20)

CPU Utilization Came Down and Rest of the Pods Terminated

**Step-8: Create a new HPA based on Memory Utilization.**

Create HPA , mention the avgUtilization value as 10Mi as each Pod is currently running at 12–13 Mi.

![Image for post](https://miro.medium.com/max/60/1\*gnju1GrH68YFFSFGgE6Z5A.png?q=20)

Current Pod Memory UtilizationMemory Based HPA Manifest

**Step 9 : Generate load as above and watch the HPA**

HPA will scale the pods

![Image for post](https://miro.medium.com/max/60/1\*cMCLkqlbP3v3JyL\_4OMRpQ.png?q=20)

Pods Scaling

![Image for post](https://miro.medium.com/max/60/1\*CQ34Lv2QjEtUcScNYEN7ww.png?q=20)

HPA Based on Memory
