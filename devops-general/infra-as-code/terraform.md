---
description: An Overview
---

# Terraform

## Pre-Requisites: <a href="a587" id="a587"></a>

Terraform is a great Infrastructure as Code tool by [Hashicorp](https://www.terraform.io) — and it can completely change the way that you work. However, many teams rush to use tools before taking a step back to analyze the circumstances surrounding the change. In order to embrace Terraform best practices, it’s vital to embrace DevOps. Let’s be honest — DevOps is the new hot paradigm: it’s the most agile, leanest, most scrummy coding environment possible. In fact, DevOps’ tagline is “CI/CD” — continuous integration/continuous delivery. How much more agile could you possibly be? …

## Why we use Terraform and not Chef, Puppet, Ansible, SaltStack, or CloudFormation <a href="feaa" id="feaa"></a>

If you search the Internet for “infrastructure-as-code”, it’s pretty easy to come up with a list of the most popular tools:

* [Chef](https://www.chef.io)
* [Puppet](https://puppet.com)
* [Ansible](https://www.ansible.com)
* [SaltStack](https://saltstack.com)
* [CloudFormation](https://aws.amazon.com/cloudformation/)
* ****[**Terraform**](https://www.terraform.io)****

In this post, we’re going to dive into some very specific reasons for why we picked Terraform over the other IAC tools. As with all technology decisions, it’s a question of trade-offs and priorities, and while your particular priorities may be different than ours, we hope that sharing our thought process will help you make your own decision. Here are the main trade-offs we considered:

* [Configuration Management vs Provisioning](https://blog.gruntwork.io/why-we-use-terraform-and-not-chef-puppet-ansible-saltstack-or-cloudformation-7989dad2865c#3f44)
* [Mutable Infrastructure vs Immutable Infrastructure](https://blog.gruntwork.io/why-we-use-terraform-and-not-chef-puppet-ansible-saltstack-or-cloudformation-7989dad2865c#b264)
* [Procedural vs Declarative](https://blog.gruntwork.io/why-we-use-terraform-and-not-chef-puppet-ansible-saltstack-or-cloudformation-7989dad2865c#f26f)
* [Master vs Masterless](https://blog.gruntwork.io/why-we-use-terraform-and-not-chef-puppet-ansible-saltstack-or-cloudformation-7989dad2865c#c7ba)
* [Agent vs Agentless](https://blog.gruntwork.io/why-we-use-terraform-and-not-chef-puppet-ansible-saltstack-or-cloudformation-7989dad2865c#64a6)
* [Large Community vs Small Community](https://blog.gruntwork.io/why-we-use-terraform-and-not-chef-puppet-ansible-saltstack-or-cloudformation-7989dad2865c#5176)
* [Mature vs Cutting Edge](https://blog.gruntwork.io/why-we-use-terraform-and-not-chef-puppet-ansible-saltstack-or-cloudformation-7989dad2865c#d105)
* [Using Multiple Tools Together](https://blog.gruntwork.io/why-we-use-terraform-and-not-chef-puppet-ansible-saltstack-or-cloudformation-7989dad2865c#1641)

Based on the above trade-offs - Chef, Puppet, Ansible, and SaltStack are all _**configuration management**_** **tools, which means they are designed to install and manage software on existing servers. CloudFormation and Terraform are actual _**infra provisioning** tools._

Terraform is revolutionizing the way organizations think of Infrastructure as Code, as well as DevOps. But, learning a new language can seem like an uphill battle if you’re late to the game. And, as DevOps organizations continue to decide to adopt it, diving into a strong Terraform tutorial can help you learn quickly and efficiently. Note: This article is intended for beginners.

Many DevOps trends have included Infrastructure as Code as areas to watch in 2020, and we’re not seeing any slowing of growth in these areas. In fact, training and learning are ranked as high-priority for DevOps organizations. 

In this article, we’ll go over resources for you to get started, including:

* Terraform Tutorial Basics: What is it?
* What are the commands to deploy infrastructure?
* Configuration Overview
* Other Resources

Let’s dive in.

## Terraform Tutorial Basics: What is it? <a href="86ee" id="86ee"></a>

Any useful Terraform tutorial starts with the question: “What is it?” 

Terraform is an open-source tool that is specifically dedicated to creating, modifying, and even deleting infrastructure on the cloud, focusing on “Infrastructure as Code.” Users who attempt to build their infrastructure use a declarative language known as [HashiCorp Configuration Language](https://github.com/hashicorp/hcl) (HCL).

You may also wonder “what is Infrastructure as Code?” Infrastructure as Code (IaC) is the management of infrastructure in a descriptive model, where the same source code generates the same binary. An Infrastructure as Code model generates the same environment every time it is applied. For a more in-depth Terraform tutorial (going over the basics), check out [HashiCorp’s introduction](https://www.terraform.io/intro/index.html).

If you’re a newcomer, it’s also important to know that like all other products, there are version-by- version releases. The latest version available is [0.14](https://github.com/hashicorp/terraform/blob/v0.14/CHANGELOG.md). However, any version from 0.12 is considered good enough for you to [download](https://www.terraform.io/downloads.html.) and have a hands-on Terraform tutorial.

## What are the commands to deploy infrastructure? <a href="df99" id="df99"></a>

Everything is controlled by a Command Line Interface (CLI) with very simple-to-remember commands. For a complete list of commands, bookmark this page.

A Terraform tutorial will refer to the following as the most-used commands:

* Init
* Plan
* Apply
* Destroy

Here’s a brief summary of the commands:

**Init:** When you have all your terraform configuration files ready in a directory, [the “init” command](https://www.terraform.io/docs/commands/init.html) can be used to initialize your working directories. Note that there is no restriction on how many times you may initialize the working directory, so you can use the command as many times as you’d like. However, remember to initialize your working directory every time you make a change to the configuration file.\
**Usage: terraform init \[options] \[DIR]**

**Plan:** The [“plan” command](https://www.terraform.io/docs/commands/plan.html) is used to set up an execution plan. It conveniently helps you to determine the actions that would be required to move to the state defined in your configuration file. What makes this command useful? It does not make changes to the real-world state but helps you see if the changes are up to your expectations and as per the configuration file. In other words, the “plan” command is your guide in setting up a strong structure to execute on.\
**Usage: terraform plan \[options] \[DIR]**

**Apply:** The [“apply” command](https://www.terraform.io/docs/commands/apply.html) is used to apply the changes to the current state to reach the desired state as per your configuration file. You may either use the command to scan the current configuration directory to apply the changes or you may specify the path to another directory where you have the configuration files. Note that confirmation is prompted when you use the command, to apply the changes.\
**Usage: terraform apply \[options] \[DIR-or-PLAN]**

**Destroy:** Any infrastructure that has been set up can be destroyed or terminated by using the command [“destroy.”](https://www.terraform.io/docs/commands/destroy.html) Note that this command prompts your confirmation before terminating the infrastructure.\
**Usage: terraform destroy \[options] \[DIR]**

## Configuration Overview <a href="089d" id="089d"></a>

Once you’ve familiarized yourself with the commands, it’s always good to know how to write configuration files. Any practical Terraform tutorial will guide you through this process.

As described earlier, there is a declarative [configuration language](https://www.terraform.io/docs/configuration/index.html), which allows you to write a description of your infrastructure requirements. All configuration files are given the .tf extension. You may also choose to write your configuration files in JSON format which will have the extension .tf.json. It is also important to know that all configuration files, irrespective of the format, should be UTF-8 encoded.

Configuration files are always a group of modules. But, what exactly are modules? Well, let’s try to explain this as simply as possible. When you define and set up an infrastructure, the most important objective is to declare resources that need to be made available. Every resource block describes objects concerning the infrastructure, such as virtual networks, DNS records, etc. Now, when such resources are grouped and relationships are defined between these resources, they form a module.

A configuration file always contains at least a root module, where the execution always begins. This root module, may or may not call other child modules.

Now that we know about [resources](https://www.terraform.io/docs/configuration/resources.html) and [modules](https://www.terraform.io/docs/configuration/modules.html), let’s consider the syntax. There are 3 very basic elements: Blocks, Arguments, and Expressions.

**Blocks:** These act like containers that hold onto the configuration of objects or resources. Each block can have its own type and also arguments. A block may even contain one or more blocks.

**Arguments: These always occur within blocks. They are just basic statements where a particular value is being assigned to a particular identifier.**

**Expressions:** Expressions are just values that are either assigned directly or by referring and combining different values. They may appear either as value for arguments or may appear as a subset of other expressions.

## Other Resources <a href="86c8" id="86c8"></a>

While reading HashiCorp’s documentation section may be helpful, there are other ways to learn as well. For example, check out [this video](https://www.youtube.com/watch?v=h970ZBgKINg\&ab_channel=HashiCorp.) where Armon Dadgar, co-founder of HashiCorp, introduces and provides a basic Terraform tutorial. For a quick, [15-minute tutorial](https://www.youtube.com/watch?v=l5k1ai_GBDE\&ab_channel=TechWorldwithNana), TechWorld with Nana has a great overview as well. Regardless of your preferred learning method, we encourage you to get out there and practice, hands-on.

