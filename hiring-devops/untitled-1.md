---
description: Key skills needed to hire a DevOps resources.
---

# Skills Needed

## Hiring DevOps Resources <a href="hiring-devops-resources" id="hiring-devops-resources"></a>

Anyone involved in hiring DevOps engineers will realize that it is hard to find prospective candidates who have all the skills listed in this section.

Ultimately, the skill set needed for an incoming DevOps engineer would depend on the current and short-term focus of the operations team. A brand new team that is rolling out a new software service would require someone with good experience in infrastructure provisioning, deployment automation, and monitoring. A team that supports a stable product might require the service of an expert who could migrate home-grown automation projects to tools and processes around standard configuration management and continuous integration tools.

DevOps practice is a glue between engineering disciplines. An experienced DevOps engineer would end up working in a very broad swath of technology landscapes that overlaps with software development, system integration, and operations engineering.

An experienced DevOps engineer would be able to describe most of the technologies that is described in the following sections. This is a comprehensive list of DevOps skills for comparing one’s expertise and a reference template for acquiring new skills.

In theory, a template like this should be used only for assessing the current experience of a prospective hire. The needed skills can be picked up on the jobs that demand deep knowledge in certain areas. Therefore, the focus should be to hire smart engineers who have a track record of picking up new skills, rolling out innovative projects at work, and contributing to reputed open-source projects.

## 1. Knowledge of Infrastructure <a href="1-knowledge-of-infrastructure" id="1-knowledge-of-infrastructure"></a>

A DevOps engineer should have a good understanding of both classic (data centre-based) and cloud infrastructure components, even if the team has a dedicated infrastructure team.

### **A. Classic Infrastructure** <a href="a-classic-infrastructure" id="a-classic-infrastructure"></a>

This involves how real hardware (servers and storage devices) are racked, networked, and accessed from both the corporate network and the internet. It also involves the provisioning of shared storage to be used across multiple servers and the methods available for that, as well as infrastructure and methods for load balancing.

#### **Virtualization Basics**

* Hypervisors.
* Virtual machines.
* Object storage.
* Running virtual machines on PC and Mac (Vagrant, VMWare, etc.).

### **B. Cloud Infrastructure**

Cloud infrastructure has to do with core cloud computing and storage components as they are implemented in one of the popular virtualization technologies (VMWare or OpenStack). It also involves the idea of elastic infrastructure and options available to implement it.

### **C. Networking Basics** <a href="networking-basics" id="networking-basics"></a>

* Network layers
* Routers, domain controllers, etc.
* Networks and subnets
* IP address
* VPN
* DNS
* Firewall
* IP tables
* Network access between applications (ACL)
* Networking in the cloud (i.e., Amazon AWS)

### **D. Load Balancing** <a href="load-balancing" id="load-balancing"></a>

* Load balancing infrastructure and methods
* Geographical load balancing
* Understanding of CDN
* Load balancing in the cloud

A DevOps engineer should have experience using specialized tools for implementing various DevOps processes. While Jenkins, Dockers, Kubernetes, Terraform, Ansible, and the like are known to most DevOps guys, other tools might be obscure or not very obvious (such as the importance of knowing one major monitoring tool in and out). Some tools like, source code control systems, are shared with development teams.

The list here has only examples of basic tools. An experienced DevOps engineer would have used some application or tool from all or most of these categories.

## **2. Source Code Management (SCM) System** <a href="source-code-management-scm-system" id="source-code-management-scm-system"></a>

* Expert-level knowledge of an SCM system such as Git or Subversion.
* Knowledge of code branching best practices, such as Git-Flow.
* Knowledge of the importance of checking in Ops code to the SCM system.
* Experience using GitHub.

## **3. Bug Management System** <a href="bug-management-system" id="bug-management-system"></a>

* Experience using a major bug management system such as Bugzilla or Jira.
* Ability to have a workflow related to the bug filing and resolution process.
* Experience integrating SCM systems with the bug resolution process and using triggers or REST APIs.

## **4. Collaborative Documentation System** <a href="collaborative-documentation-system" id="collaborative-documentation-system"></a>

* Knowledge of Wiki basics.
* Experience using MediaWiki, Confluence, etc.
* Knowledge of why DevOps projects have to be documented.
* Knowledge of how documents were organized on a Wiki-based system.

## **5. Build and CI** <a href="build-and-ci" id="build-and-ci"></a>

### A. CI

* Basics of compilers such as node.js and Javac.
* Make and Makefile, npm, Maven, Gradle, etc.
* Code libraries in Node, Java, Python, React etc.
* Build artefacts such as JAR, WAR and node modules.
* Setting up Jenkins and relevant plugins.
* Experience building on Jenkins standalone, or dockerized.
* Experience using Jenkins as a Continuous Integration (CI) platform.
* CI/CD pipeline scripting using groovy
* Experience with CI platform features such as:
  * Integration with SCM systems.
  * Secret management and SSH-based access management.
  * Scheduling and chaining of build jobs.
  * Source-code change based triggers.
  * Worker and slave nodes.
  * REST API support and Notification management.



### **B.  Packaging**

* **Packaging files:** ZIP, TAR, GZIP, etc.
* **Packaging for deployment:** RPM, Debian, DNF, Zypper, etc.
* **Packaging for the cloud:** AWS AMI, VMWare template, etc.
* Use of Packer.
* Docker and containers for microservices.

### **C. Artefacts Management**

* **Use of artefacts repository:** Distribution and release of builds; meeting build and deployment dependencies
* Serving artefacts from a shared storage volume
* Mounting locations from cloud storage services such as AWS S3
* Artifactory as artefacts server

## **6. Configuration Management** <a href="configuration-management" id="configuration-management"></a>

* Should be able to explain configuration management.
* Experience using any Configuration Management Database (CMDB) system.
* Experience using open-source tools such as Cobbler for inventory management.
* Ability to do both agent-less and agent-driven enforcement of configuration.
* Experience using Ansible, Puppet, Chef, Cobbler, etc.

## **7. Orchestration and Deployment** <a href="orchestration-and-deployment" id="orchestration-and-deployment"></a>

* Knowledge of the workflow of released code getting into production.
* Ability to push code to production with the use of SSH-based tools such as Ansible.
* Ability to perform on-demand or Continuous Delivery (CD) of code from Jenkins.
* Ability to perform agent-driven code pull to update the production environment.
* Knowledge of deployment strategies, with or without an impact on the software service.
* Knowledge of code deployment in the cloud (using auto-scaling groups, machine images, etc.).

## **8. Monitoring** <a href="monitoring" id="monitoring"></a>

* Knowledge of all monitoring categories: system, platform, application, business, last-mile, log management, and meta-monitoring.
* Status-based monitoring with Nagios.
* Data-driven monitoring with Zabbix.
* Experience with last-mile monitoring, as done by Pingdom or Catchpoint.
* Experience doing log management with ELK.
* Experience monitoring SaaS solutions (i.e., Datadog and Loggly).

To get an automation project up and running, a DevOps engineer builds new things such as configuration objects in an application and code snippets of full-blown programs. However, a major part of the work is glueing many things together at the system level on the given infrastructure. Such efforts are not different from traditional system integration work and, in my opinion, the ingenuity of an engineer at this level determines his or her real value on the team. It is easy to find cookbooks, recipes, and best practices for vendor-supported tools, but it would take experience working on diverse projects to gain the necessary skill set to implement robust integrations that have to work reliably in production.

Important system-level tools and techniques are listed here. The engineer should have knowledge about the following.

## **9. Access Management** <a href="access-management" id="access-management"></a>

* Users and groups on Linux.
* Use of service accounts for automation.
* Sudo commands, /etc/sudoers files, and passwordless access.
* Using LDAP and AD for access management.
* Remote access using SSH.
  * SSH keys and related topics.
  * SCP, SFTP, and related tools.
  * SSH key formats.
* Managing access using configuration management tools.

## **10. Password Management** <a href="password-management" id="password-management"></a>

* Use of GPG for password encryption.
* Tools for password management such as KeePass.
* MD5, KMS for encryption/decryption.
* Remote access with authentication from automation scripts.
* Managing API keys.
* Jenkins plugins for password management.

## **11. File Transfer** <a href="file-transfer" id="file-transfer"></a>

* Typical uses of the find, DF, DU, etc.
* SCP, Rsync, FTP, and SSL counterparts
* Via shared storage
* File transfer with cloud storage services such as AWS S3

## **12. Job Management** <a href="job-management" id="job-management"></a>

* Use of crontab.
* Running jobs in the background; use of Nohup.
* Use of screen to launch long-running jobs.
* Jenkins as a process manager.

## **13. Linux Distributions** <a href="linux-distributions" id="linux-distributions"></a>

* A comparison of popular distributions.
* Checking OS release and system info.
* Package management differences.
* OS Internals and Commands

## **14. Text Processing** <a href="text-processing" id="text-processing"></a>

* Typical uses of SED, AWK, GREP, TR, etc.
* Scripting using Perl, Python.
* Regular expressions.
* Support for regular expressions in Perl and Python.
* NC
* Netstat
* Traceroute
* VMStat
* LSOF
* Top
* NSLookup
* Ping
* TCPDump
* Dig
* Sar
* Uptime
* IFConfig
* Route

## 15. Programming Primer for DevOps <a href="programming-primer-for-devops" id="programming-primer-for-devops"></a>

One of the attributes that helps differentiate a DevOps engineer from other members in the operations team, like sysadmins, DBAs, and operations support staff, is his or her ability to write code. The coding and scripting skill is just one of the tools in the DevOps toolbox, but it's a powerful one that a DevOps engineer would maintain as part of practising his or her trade.

Coding is the last resort when things cannot be integrated by configuring and tweaking the applications and tools that are used in an automation project.

### **A. Bash Scripting Essentials** <a href="bash-scripting-essentials" id="bash-scripting-essentials"></a>

Many times, a few lines of bash script could be the best glue code integrating two components in the whole software system. DevOps engineer should have basic shell scripting skills and Bash is the most popular right now.

### **B. Python** <a href="python" id="python"></a>

If a script has to deal with external systems and components or it's more than just a few lines of command-lines and dealing with fairly complex logic, it might be better to write that script in an advanced scripting language like Python, Perl, or Ruby.

Knowledge of Python would make your life easier when dealing with DevOps applications such as Ansible, which uses Python syntax to define data structures and implement conditionals for defining configurations.

### **C. Web Programming** <a href="web-programming" id="web-programming"></a>

One of the categories of projects a DevOps engineer would end up doing is building dashboards. Though dashboarding features are found with most of the DevOps tools, those are specific to the application, and there will be a time when you may require to have a general-purpose dashboard with more dynamic content than just static links and text.

Another requirement is to build web UI for provisioning tools to present those as self-service tools to user groups.

In both these cases, deep web programming skills are not required. Knowledge of a web programming friendly language such as PHP and a JavaScript/CSS/HTML library like Composer would be enough to get things started. It is also important for the DevOps engineer to know the full-stack, in this case, LAMP, for building and running the web apps.

### **D. Configuration Languages** <a href="configuration-languages" id="configuration-languages"></a>

Almost every application and tool that is used for building, deploying, and maintaining software systems use configuration files. While manual reading of these files might not require any expertise, a DevOps engineer should know how config files in such formats are created and parsed programmatically.

A DevOps engineer should have a good understanding of these formats:

* INI.
* XML.
* JSON.
* YAML.

The engineer should also know how these formats are parsed in his/her favourite scripting language.

### **E. REST API**

The wide acceptance of REST API as a standard to expose features that other applications can use for system integration made it a feature requirement for any application that wants to be taken seriously. The knowledge of using REST API has become an important skill for DevOps engineer.

* **HTTP/HTTPS:** REST APIs are based on HTTP/HTTPS protocol and a solid understanding of its working is required. Knowledge of HTTP headers, status codes, and main verbs GET, POST, and PUT.
* **REST API basics:** Normal layout of APIs defined for an application.
* **Curl and Wget**: Command-line tools to access REST API and HTTP URLs. Some knowledge of the support available for HTTP protocol in scripting languages will be useful and that would be an indication of working with REST APIs.
* **Authentication methods:** Cookie-based and OAuth authentication; API keys; use of If-Match and If-None-Match set of HTTP headers for updates.
* **API management tools:** If the application you support provides an API for the users, most probably, its usage will be managed by some API Gateway tool. Though not an essential skill, experience in this area would be good if one works on the API provider side.

### **F. Programming With Data Repositories**

There was a time when mere knowledge of programming with RDBMS was enough for an application developer and system integrator to manage application data. Now with the wide adoption of Big Data platform like Hadoop and NOSQL systems to process and store data, a DevOps engineer needs varied requirements, from one project to another. Core skills are the following:

* **RDBMS:** MySQL, Postgres, etc. knowledge of one or more is important.
* **Setting up and configuring PostGres:** As an open-source database used with many other tools in the DevOps toolchain, consider this as a basic requirement for a DevOps engineer. If one hasn’t done this, he or she might not have done enough yet.
* **Running queries from a Bash script:** How to run a database query via a database client from a Bash script and use the output. MySQL is a good example.
* **Database access from Perl/PHP/Python:** All the major scripting languages provide modules to access databases and that can be used to write robust automation scripts. Examples are Perl DBI and Python’s MySQLdb module.
* **DB Backups:** Migration, Logging, monitoring and cleanup.

### **G. Programming for Cloud**

Those who have built cloud infrastructure with a focus on automation and versioning should know some of these (or similar) tools:

* **cloud-init:** Cloud-init can be used to configure a virtual machine when it is spun up. This is very useful when a node is spun up from a machine image with baseline or even application software already baked in.
* **AWS/Azure/GCloud CLI:** If the application runs on Commercial cloud, knowledge of CLI is needed, which would be handy to put together simple automation scripts.
* **Terraform:** HashiCorp’s Terraform is an important tool if the focus would be to provision infrastructure as code (IaaS). Using this, infrastructure can be configured independently of the target cloud or virtualization platform.
* **Ansible:** It can be used to build machine images for a variety of virtualization technologies and cloud platforms, it is useful if the infrastructure is provisioned in a mixed or hybrid cloud environment.

### **H. Error Handling**

In a rush to get things rolled out, one of the things left half-done is adding enough error handling in scripts. Automation scripts that are not robust can cause major production issues, which could impact the credibility of DevOps efforts itself. A DevOps engineer should be aware of the following best practices in error handling and logging:

* The importance of error handling in automated scripts.
* Error handling in Bash.
* Error handling in Python.
* Logging errors in application and system logs.
