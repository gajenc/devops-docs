---
description: How Chaos Engineering plays a vital role in Devops Success
---

# Chaos Engineering

Before we get deeper into “Chaos Engineering”, let’s get some idea about the importance of testing in the software development cycle.

Typically, any organization’s goal is never to let their software crash. It needs to be available every time it is required. Software failures can cost outages for companies. It eventually leads to a bad customer experience for customers trying to shop, transact business, and get work done. This is where, organisations needed a robust solution to solve this challenge. That is how Chaos Engineering came into picture.

Netflix is a leader in Chaos Engineering. In fact, the concept of Chaos engineering was introduced by Netflix. Chaos Engineering is a disciplined approach to identifying failures before they become outages.

> _As per definition from Wikipedia, “**Chaos engineering is the discipline of experimenting on a software system in production in order to build confidence in the system’s capability to withstand turbulent and unexpected conditions”**_

By proactively testing how a system responds under stress, you can identify and fix failures before they end up in the news. Chaos Engineering lets you compare what you think will happen to what happens in your system.

**Requirements of Chaos Engineering**\
 \* It improves the resilience of the system.\
 \* You will get to know the weakness of the system.\
 \* It is proactive, as opposed to the reactive nature of traditional testing.\
 \* It exposes hidden threats and minimizes the risk.

**History of Chaos Engineering |Infrastructure Testing In Netflix Way**

Chaos Engineering first became relevant at internet companies that were pioneering large scale systems. The Netflix engineering team created Chaos Monkey in 2010. Chaos Monkey was developed as Netflix moved from physical infrastructure to cloud infrastructure provided by AWS. They wanted to make sure that a loss of an Amazon instance wouldn’t affect the Netflix streaming. After the success of Chaos Monkey, the Netflix team created a suite of tools that supports Chaos Engineering principles, named the Simian Army. Simian Army added additional failure injection modes on top of Chaos Monkey that would allow testing of a more complete suite of failure states.

Later in 2014, Netflix decided to create a new role called the _Chaos Engineer._ In October 2014, Kolton Andrew’s team at Netflix announced Failure Injection Testing(FIT). It was a new tool built on the concepts of the Simian Army. FIT gave developers more granular control over the blast radius of their failure injection. This awesome tool also gave the developers control over the scope of their failure. so, they could realize the insights of Chaos Engineering, but mitigate the potential downside.

> _The reason behind running the Chaos Monkey tool in the Netflix system is simple:_ **The cloud is all about redundancy and fault-tolerance. Since no single component can guarantee 100% uptime (and even the most expensive hardware eventually fails), we have to design a cloud architecture where individual components can fail without affecting the availability of the entire system. In effect, we have to be stronger than our weakest link.** — [Netflix Tech Blog](https://medium.com/netflix-techblog/the-netflix-simian-army-16e57fbab116)

Some other popular Chaos Engineering tools are:

* Latency Monkey
* Doctor Monkey
* Conformity Monkey
* Janitor monkey
* Security Monkey
* Chaos Gorilla
* 10–18 Monkey

These Chaos Engineering tools are constantly testing the system against all kinds of failures, it helps to build a higher level of confidence in the system’s ability to survive.

**Benefits of Chaos Engineering**

When you stream using Netflix, and your service fails, you may switch to a Youtube video. Netflix loses money because they were unable to retain your attention. A company’s reputation decrease when their services go down. This cost can be calculated as a dollar-per-hour metric and has become common in many company’s KPIs.

Here are some of the additional benefits of Chaos Engineering.

**1. Technical Benefits:** The Chaos experiment insights can mean a reduction in incidents, reduction in on-call burden, a better understanding of system failures, improved system design, faster mean time to detect SEVs, and reduction in repeated SEVs.

**2. Customer Benefits:** The increased availability and durability of service lead to no outages disrupt their day-to-day lives.

**3. Business Benefits:** Chaos Engineering will prevent huge losses in revenue & maintenance costs, which results in happier & more engaged engineers. This will also improve on-call training for engineering teams & improve the SEV management program for the entire organization.

Chaos Engineering is a strategy for discovering vulnerabilities and it allows an Admin to do the following things:\
 \* Identify the poor points in a system.\
 \* Check how a system responds to pressure in real-time.\
 \* Make the team ready for real possible failures.\
 \* Identify the bugs that are yet to cause system-wide problems.

**Here is a list of wider benefits of Chaos Engineering:**\
 \* Simulating high load of CPU.\
 \* It adds instructions to a program and allows fault injection.\
 \* It disrupts syncs between system clocks.\
 \* Turning a virtual machine off to check dependency reaction.\
 \* It stimulates the failure of micro-component.\
 \* It injects latency between services.\
 \* It executes a routine in driver code emulating I/O errors.\
 \* It performs function-based chaos like randomly causing functions to throw exceptions.

_Chaos Engineering is more than a preventive mechanism. Chaos Engineering will make your system more resilient and will increase the confidence in the system’s capabilities._ There are a plethora of tools for Chaos Engineering you can experiment with different tools & techniques to make it more mature & useful. An organization can achieve long-term software resiliency by intentionally creating Chaos in the system.

![](<../.gitbook/assets/image (46).png>)

**Chaos Engineering and DevOps: How it can help DevOps?**

It would be best to leverage a DevOps strategy that can work on different factors to make a system resilient to any breakdown. By testing a system with random failures, DevOps teams get to understand their system’s weaknesses. This lets the team make informed decisions around prioritising tasks to upgrade their systems.

Combining Chaos Engineering with DevOps not only detects any turbulence effectively but also helps in fixing it in a phased manner.Chaos engineering leverages observability to discover and overcome system weaknesses.Without observability, there is no chaos engineering.

Anyone can implement Chaos Engineering in DevOps with 5 simple steps:\
 1\. Define the resilience parameters\
 2\. Create a resilience strategy\
 3\. Execute the resilience strategy\
 4\. Compare the metrics within a group\
 5\. Fix and minimize the blast radius

**Conclusion**

Implementing Chaos Engineering within your system needs thinking. At the same time, it will also evoke confidence. n conclusion, Chaos Engineering tries to discover the failure points and identify what will happen in the case of resource or object unavailability. This is a very suitable practice in modern software development approaches like DevOps and microservices architectures.

By deliberately injecting failure into your application or infrastructure setup , the chaos engineering paradox helps harden organizations against failures.

Companies other than Netflix, who use Chaos Engineering are Facebook, LinkedIn, Google, Amazon, Microsoft, etc. So, what are your thoughts on Chaos Engineering? Please share you comments below and do not forget to share the post with your network.
