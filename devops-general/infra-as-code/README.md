# Infra-as-code(IaC)

Infrastructure as code is the ability to provision and manage infrastructure using a configuration language. Infrastructure as code (IAC) brings the repeatability, transparency, and testing of modern software development to the management of infrastructure such as networks, load balancers, virtual machines, Kubernetes clusters, and monitoring. A primary goal of IAC is to reduce error and configuration drift, while allowing engineers to spend time on higher value tasks. IAC defines what the end state of your infrastructure looks like, instead of defining a series of steps to be executed - IAC tools like Terraform can be run multiple times against your infrastructure, producing the same desired result.

Using a cloud user-interface to create a managed Kubernetes cluster is a relatively straightforward process, however using infrastructure as code helps standardize cluster configuration and manage add-ons like network policy, maintenance windows, and Identity and Access Management (IAM) for cluster nodes and workloads.

In DIGIT, we are 100% Kubernetes focused and use infrastructure as code to ensure our managed Kubernetes customers achieve all of the benefits outlined above. We use IaC for prerequisite cloud resources like networks, Kubernetes clusters, monitoring and alerting, and tooling that runs on top of Kubernetes to manage DNS records, HTTP routing, and SSL certificates.

## Mutable Infrastructure vs Immutable Infrastructure <a href="b264" id="b264"></a>

Configuration management tools such as Chef, Puppet, Ansible, and SaltStack typically default to a **mutable infrastructure** paradigm. For example, if you tell Chef to install a new version of OpenSSL, it’ll run the software update on your existing servers and the changes will happen in-place. Over time, as you apply more and more updates, each server builds up a unique history of changes. This often leads to a phenomenon known as _configuration drift_, where each server becomes slightly different than all the others, leading to subtle configuration bugs that are difficult to diagnose and nearly impossible to reproduce.

If you’re using a provisioning tool such as Terraform to deploy machine images created by Docker or Packer, then every “change” is actually a deployment of a new server (just like every “change” to a variable in functional programming actually returns a new variable). For example, to deploy a new version of OpenSSL, you would create a new image using Packer or Docker with the new version of OpenSSL already installed, deploy that image across a set of totally new servers, and then undeploy the old servers. This approach reduces the likelihood of configuration drift bugs, makes it easier to know exactly what software is running on a server, and allows you to trivially deploy any previous version of the software at any time. Of course, it’s possible to force configuration management tools to do immutable deployments too, but it’s not the idiomatic approach for those tools, whereas it’s a natural way to use provisioning tools.

**Example of a mutable Infrastructure vs. immutable Infrastructure**

![](https://miro.medium.com/max/2160/0\*L-0xZ2NsOqhP_Ddw)

