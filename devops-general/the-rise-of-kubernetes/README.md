---
description: Origin, Present & Future
---

# The Rise of Kubernetes

**T**he world is moving ahead with an exceptionally rapid pace, and the world of Computers or if I may say, Information Technology is no exception. Today if you keenly follow the digital world every now and then a new process gets introduced, a practice is developed, an automation approach is built, a platform is in making, and a new open-source is checked in to be branched, forked and developed.

From physical servers in a data center to the introduction of virtualization on them and from setting up the application back end on the cloud to containerize them, the world has seen some tremendous changes that would have looked anachronous while people at Microsoft or Apple were in the making of Operating Systems

![Deployment Shift from Traditional to Containers](https://media-exp1.licdn.com/dms/image/C4D12AQFed8fWp-FA2w/article-inline_image-shrink\_1000\_1488/0/1592152664431?e=1616025600\&v=beta\&t=\_HpPul44y56fkqCoYSdcH2CByfe80DXNAMzNv4dTh3k)

And similar was the moment of bewilderment for the Head of Technical Infrastructure and Chief Architect of many of Google’s most important network innovations Urs Hölzle, the current SVP Engineering at Google when three Googlers: Joe Beda, Brendan Burns, and Craig McLuckie pitched him the idea of "Seven of Nine". And out of oblivion his immediate reaction to the idea was,

> So, let me get this straight. You want to build an external version of the Borg task scheduler. One of our most important competitive advantages. The one we don’t even talk about externally. And, on top of that, you want to open source it?

You can clearly hear the disbelief in his first reaction, but little did he knew the room was discussing something that was not just a wave but a storm that will revolutionize the way a containerized application is developed, deployed, and managed.

If you are working with the containerized application, Kubernetes is not a new term for you and you must have already been aware of what potential it carries. The official site of Kubernetes defines it as an open-source system for automating deployment, scaling, and management of containerized applications. There are many write-ups, blogs and official documents that define Kubernetes in multiple ways revolving around its orchestration capabilities and easy to use fundamentals, however, there is little documentation available about how Kubernetes was developed, how it came into existence and why is it was open-sourced when the developers could have made astronomical profits. Today I tried to summarize some answers to these questions here and share my perspective about the concept Kubernetes which is also referred to as “ From Google to the World ”.

#### THE ORIGIN - HOW IT STARTED

It was the Summer of 2013, three STAR TREK fans in a broad daylight at the Google Headquarters settled down in a meeting room to pitch the idea to have an open-source container management system. The idea that they cultivated while working on something Google used to have them work with - Google Borg Systems.

![Google Borg System](https://media-exp1.licdn.com/dms/image/C4D12AQEqdjocWIQFug/article-inline_image-shrink\_1000\_1488/0/1592152799292?e=1616025600\&v=beta\&t=6iW0rxISJZh_i4iHbn96QgWi8gIy7wRC4XLIl7vQVGQ)

In the early days of Google, the operations team working closely on the backend infrastructure supporting multiple services like Google Search, Gmail and others identified a flaw in the design which indicated not the entire setup of machines present are being utilized to fullest of their potentials, and that observation led to the setup of a redesigned infrastructure backend. Inspired by the Star Trek movies franchise that narrates The Borg as an alien group that appears as recurring antagonists in the movie Google named this new setup as - The Borg Systems. Google Borg Systems - a scheduler that runs hundreds of thousands of jobs, from many thousands of different applications, across several clusters each with up to tens of thousands of machines. The prime goal of Borg Systems was to wring every possible ounce of performance out of the servers. Fun Fact: This same Borg’s infrastructure is used to deliver the present Google's cloud service

While working with Borg Systems Joe Beda, Brendan Burns, and Craig McLuckie through years of trial and error knew the trick was a great container management system. And that’s what they pitched project Seven of Nine (another reference to a Star Trek character of the same name that is a "friendlier" Borg, and also the reason why Kubernetes logo has seven sides) which later became the today’s Kubernetes (κυβερνήτης, Greek for "helmsman" or "pilot" or "governor").

Initially, the project Seven of Nine was rejected by Urs Hölzle, but a turning point was a fateful shuttle ride where Craig McLucki found himself sitting next to Eric Brewer, VP of Cloud at Google, and one of Urs Hölzle's key strategists. Craig had an uninterrupted chunk of time to explain the idea to Eric, and he was convinced. Soon after, they got the green light from Urs Hölzle and within three months combining with an elegant, simple, and easy-touse UI the first prototype was ready to share.

#### KUBERNETES TODAY - THE PRESENT DAY CLUSTER

Kubernetes now came a long way, it has become easy-to-use and easy-to-learn, and this has been the key features of the wide-spread acceptance everywhere. Kubernetes today has existence ranging from small proof of concept project to a massive enterprise-level production setup in different organizations.

In a high-level description of how Kubernetes looks today is below. The Kubernetes Cluster consists of Master Nodes: The Brain and the Worker Nodes: The one where container runs.

![Kubernetes Cluster](https://media-exp1.licdn.com/dms/image/C4D12AQFi28tbirOkaw/article-inline_image-shrink\_1000\_1488/0/1592153006240?e=1616025600\&v=beta\&t=54WgajBBr5tJI2-XomDxCtSyseybaZ0yCJHXAZLp5RI)

Starting off from Brog, a basic task scheduler spanning across multiple machines Kubernetes today has become a complete suite for container management and orchestration. It empowers the organizations to span their containers across multiple platforms, scale-on-demand, self-heal ensuring a fault-tolerant and highly available environment.

#### WHY GOOGLE OPEN SOURCED KUBERNETES

Kubernetes today is one of the most contributed repositories in GitHub. According to the Cloud Native Computing Foundations recent data compared to the 1.5 million projects on GitHub, Kubernetes is **ninth **for commits and second for authors/issues, **second **only to Linux. These numbers itself speak about the popularity and the enormous adaptation by the enterprises globally, in fact, global organizations like _Google, Uber, Box, Atlassian, Bloomberg, Blackrock, BlaBlaCar, The New York Times, Lyft, eBay, Buffer, Ancestry, GolfNow, Goldman Sachs_ and many others use Kubernetes in production at massive scale, three of the largest cloud providers offer their own managed Kubernetes services today. Looking at this colossal amount of usability, it should amaze everyone why Google never made money out of this and why opted to go open-source.

![Top Kubernetes Contributors](https://media-exp1.licdn.com/dms/image/C4D12AQGvmwnnruGh8w/article-inline_image-shrink\_1000\_1488/0/1592153074846?e=1616025600\&v=beta\&t=EqQVsEJFyiTFGQVwBetUiJF_nFhax8BOu6VnJMEzYyE)

The answer is in the belief with which Joe Beda, Brendan Burns, and Craig McLuckie pitched the idea of Kubernetes.

They believed that open-sourcing Kubernetes was the right way to go, bringing many benefits to the project. For one, feedback loops were essentially instantaneous — if there was a problem or something didn’t work quite right, they knew about it immediately. But most importantly, they were able to work with lots of great engineers, many of whom really understood the needs of businesses that would benefit from deploying containers. It was a virtuous cycle: the work of talented engineers led to more interest in the project, which further increased the rate of improvement and usage.![The Open Source Contribution At A Glance](https://media-exp1.licdn.com/dms/image/C4D12AQHrmviAekbUWA/article-inline_image-shrink\_1000\_1488/0/1592153120892?e=1616025600\&v=beta\&t=fH3Q-tHJZkxUYLgX897BqNNJIJo7-EN8jCFZ0auWQy0)

And the numbers here show how correct the three minds were since the very first commit to Kubernetes which occurred on June 6, 2014, and then later donated to the CNCF five years ago, there have been more than 35k individual contributors, 148k code commits, 83k pull requests, 1.1 million contributions to the code base and more than 2,000 contributing companies.

#### THE FUTURE AHEAD - BRIGHT AND CONTAINERIZED

Containers are rapidly taking over the world of software development, and the momentum behind Kubernetes is accelerating. It has become the go-to container orchestrator through its deep expertise, enterprise adoption, and robust ecosystem. As we learned about with a growing number of contributors and service providers backing it, Kubernetes will continue to improve and expand upon its functionality, the types of applications it can support, and integrations with the overarching ecosystem. The combination of these factors will further accelerate and enhance the use of containers and microservices to fundamentally reshape the way in which software is developed, deployed, and improved upon.

Further, as container orchestration and microservices continue to mature, they will open the door to the adoption of new deployment patterns, development practices, and business models.

The latest Kubernetes stable version is 1.18 launched on 25th March 2020 and we can also contribute to the next stable release And there can't be a better time to start the journey with Kubernetes than today and maybe one of our commits can be present in the next stable release.
