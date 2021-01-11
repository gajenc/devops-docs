---
description: What is container and Docker
---

# Docker

Now that we understand the evolution of IT software infrastructure, we might want to know how we can achieve things like `Microservices Architecture` and `Containerization` that we discussed earlier? A simple answer to this could be, `Docker`.

[Docker](https://www.docker.com/) is the world's leading software containerization platform. It encapsulates your microservice into what we call as `Docker container` which can then be independently maintained and deployed. Each of these containers will be responsible for one specific business functionality.

To understand Docker in a little more depth, let us take the same example of an ecommerce website that we discussed earlier. We know that it will have multiple operations and services such as account creation, displaying product catalog, building and validation shopping cart and so on. In a microservice architecture, all these can be treated as microservices and encapsulated in a Docker container. But why would you do that?

One of the reasons would be to ensure consistency between development and production environment. For instance, consider that three developers are working on this application. Each of them having their environments. So one developer might be running `Windows` OS on his machine, while the second developer could be running `Mac` OS and the third developer would prefer a `Linux` based OS. Each of them would take hours of efforts to install the application in their respective development environments and an additional set of efforts to later deploy the same app on the cloud. This is not smooth, and there is a lot of friction to port such applications to cloud infrastructure.

With Docker, you can make your application independent of the host environment. Since you have microservices architecture, you can now encapsulate each of them in Docker containers. Docker containers are lightweight, resource isolated environments through which you can build, maintain, ship and deploy your application.

**Advantages**

1. Docker is a popular evolving software with excellent community support and built for microservices
2. It is lightweight when compared to VMs making it cost and resource effective
3. It provides uniformity across development and production environments making it a suitable fit for building cloud-native applications
4. It provides facilities for continuous integration and deployment
5. Docker is not going anywhere; it provides integration with popular tools and services such as AWS, Microsoft Azure, Ansible, Kubernetes, Istio and lot more

**Conclusion:**

Containers have become the standard output of the development process, and Kubernetes has emerged as the standard for container orchestration platforms. With Kubernetes, containers can be managed by clusters in public cloud, hybrid cloud, and even in a multi-cloud environment.

