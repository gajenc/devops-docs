# Public & Private Clouds

Since the time the public cloud has become prominent, there have been multiple attempts to bring parity between on-premises infrastructure and cloud infrastructure.

Open Source projects like [OpenStack](https://www.openstack.org), [CloudStack](https://cloudstack.apache.org) and [Eucalyptus](https://buy.hpe.com/in/en/enterprise-solutions/cloud-solutions/helion-cloud/helion-openstack-cloud-software/hpe-helion-openstack-cloud-software/p/1010838414) aimed to become the hybrid cloud platforms for seamlessly integrating enterprise data centers with the public cloud. 

Due to the disparity between the hypervisors and the virtual machine managers running in on-premises and the cloud, workload portability was never easy. Cloud bursting, the ability to effortlessly scale the infrastructure and applications to the cloud remained a pipedream of infrastructure architects.

![Hypervisors and VMMs](https://specials-images.forbesimg.com/imageserve/5df854b04e29170007833b85/960x0.jpg?fit=scale)

Since 2015, two major trends started to change the face of the hybrid cloud - containers and Kubernetes.

The container runtime became the lowest common denominator to run workloads across physical machines, private cloud and the public cloud. Container images have become the preferred deployment units of software. In a lot of ways, Docker and container runtimes became an alternative to hypervisors. A containerized application developed on macOS could be easily deployed in Amazon EC2, Google Compute Engine or Azure VMs with absolutely no changes to the code and configuration.

If Docker is the new hypervisor, Kubernetes became the replacement for proprietary virtual machine managers. With containers as the deployment unit and Kubernetes as the orchestration manager, the industry finally agreed on a standard infrastructure layer. 

Red Hat, VMware, Canonical, Mirantis, Rancher and other vendors offer Kubernetes-based platforms that can run in both enterprise data centers and the public cloud. The rise of Kubernetes forced hyperscale cloud vendors such as Alibaba, AWS, IBM, Google, Huawei, Microsoft and Oracle to offer managed Kubernetes services.

The [Cloud Native Computing Foundation](https://www.cncf.io), the governance body that manages Kubernetes, played a key role in making sure that the commercial implementations conform to a standard. The [Certified Kubernetes Conformance Program](https://www.cncf.io/certification/software-conformance/) ensures that every vendor’s version of Kubernetes supports the required APIs, as do open source community versions. For organizations using Kubernetes, conformance enables interoperability from one Kubernetes installation to the next. It allows them the flexibility to choose between vendors.

CNCF also manages the [containerd](https://containerd.io) project, the standard that defines the container runtimes. As long as the container runtime adheres to the containerd specification, Kubernetes can orchestrate the workloads. The combination of containers and Kubernetes has become the foundation of modern infrastructure.

![Containers and Kubernetes ](https://specials-images.forbesimg.com/imageserve/5df85505e961e10007393aec/960x0.jpg?fit=scale)

Thanks to the standardization efforts and the conformance program, a developer developing and testing containerized software on his desktop can confidently deploy it in a production environment running Kubernetes. This guaranteed compatibility of Kubernetes across different environments and distributions resulted in the rapid adoption across startups, mid-sized companies, and large enterprises. 

With container runtime and Kubernetes becoming the gold standard of modern infrastructure, the original promise of the hybrid cloud is no more a distant dream. 

This year, we have seen the launch of Kubernetes-based hybrid cloud platforms from almost all the major infrastructure vendors. These new offerings not only manage clusters running on-premises and in their own cloud platforms but any Kubernetes cluster including those that are deployed in other cloud environments.

IBM took the plunge by announcing [IBM Cloud Paks](https://www.forbes.com/sites/janakirammsv/2019/10/30/how-ibm-cloud-paks-enable-enterprises-to-modernize-their-workloads/) (formerly IBM Cloud Private) followed by Google which launched [Anthos](https://www.forbes.com/sites/janakirammsv/2019/04/14/everything-you-want-to-know-about-anthos-googles-hybrid-and-multi-cloud-platform/) at Cloud NEXT 2019 event. At VMworld 2019, VMware announced [Project Pacific and Tanzu Mission Control](https://www.forbes.com/sites/janakirammsv/2019/09/03/how-vmware-leveraged-kubernetes-for-its-cloud-native-and-multi-cloud-strategy/#1d648f4f67c5) - a platform that brings the best of Kubernetes and vSphere. More recently, Microsoft launched [Azure Arc](https://www.forbes.com/sites/janakirammsv/2019/11/05/why-azure-arc-is-a-game-changer-for-microsoft/) that can manage Azure’s own hosted Kubernetes services, AKS along with Kubernetes clusters running outside of Azure. 

What’s common in these platforms is that Kubernetes is at the front and center of its hybrid strategy. Thanks to Kubernetes, these hybrid cloud platforms not only enable workload portability but also deliver the ability to scale the workloads across disparate environments.

![Kubernetes-based Hybrid Cloud](https://specials-images.forbesimg.com/imageserve/5df8623f4e29170007833c4e/960x0.jpg?fit=scale)

Going forward, Kubernetes will become the universal control plane that can manage containers, virtual machines, legacy workloads, and modern applications.
