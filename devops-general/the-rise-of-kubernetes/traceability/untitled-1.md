# Logging

## Introduction <a href="introduction" id="introduction"></a>

The logging concern is one of the most complicated parts of our microservices. Microservices should stay as pure as possible. So, we shouldn’t use any library if we can (like logging, monitoring, resilience library dependencies). It means, every dependency can change any time and then usually, we must do that change for the other microservices. There is a lot of work here. Instead of that, we need to handle these dependencies with a more generic way. For logging, **the way is the stdout logging**. For most of the programming languages, logging to stdout is the default way and probably no additional change required at the beginning.

## What is needed to build a meaningful logging system in MSA? <a href="what-is-needed-to-build-a-meaningful-logging-system-in-msa" id="what-is-needed-to-build-a-meaningful-logging-system-in-msa"></a>

## **1. Use a Unique Id to correlate Requests** <a href="1-use-a-unique-id-to-correlate-requests" id="1-use-a-unique-id-to-correlate-requests"></a>

In MSA, services interact with each other through an HTTP endpoint. End users only know about API Contract (Request/Response), and don’t know how exactly do services work.

“A service” will call “B service” and “C service”. Once the request chain is complete, “X service” might be able to respond to the end-user who initiated the request. Let’s say you already have a logging system that captures error logs for each service. If you find an error in “X service”, it would be better if you know exactly whether the error was caused by “A service” or “C service”. If the error is informative enough for you. But if that isn’t the case, the correct way to reproduce that error is to know all requests and services that involved. Once you implement Correlation Id, you only need to look for that ID in the logging system. And you will get all logs from services that were part of the main request to the system.

## **2. Centralise Logging data in one place** <a href="2-centralise-logging-data-in-one-place" id="2-centralise-logging-data-in-one-place"></a>

![](https://miro.medium.com/max/1600/1\*DgjE3\_C6GISqbznXN8fXjA.png)

The application usually adds more features as time goes by. Go along with this, there are so many services will be created new (my project started with 12 services, and now we have 20). These services could be hosted on different servers. Let’s imagine, what will happen if you store logging on different servers? — you will have to access to each individual server to read logs, then trying to correlate problems. Instead, you have everything that you need in one dashboard by centralized logging data in one place. If would save your time so much.

## **3. Define the format for logging** <a href="3-define-the-format-for-logging" id="3-define-the-format-for-logging"></a>

Applying MSA allows you to use different technology stacks for each service. For example, you can use .Net Core for Buy service, Java for Shipping service and Python for Inventory service. However, it also impacts to log format of each service. It’s even more complicated as some logs need more fields than others.

Based on my experience, I’d like to suggest JSON as a standard format for logging data. JSON allows you to have multiple levels for your data so that, when necessary, you can get more semantic info in a single log event.

## **4. Log useful/meaningful data** <a href="4-log-useful-meaningful-data" id="4-log-useful-meaningful-data"></a>

When we see the log one would want to know everything! What? When? Where?… even Who? — don’t think that we need to know exactly which person causes the problem to blame them :) Because, contacting the right person also helps you to resolve issues quicker. You can log all the data that you get. However, let us give some specific fields. This might help to figure out what really need to log.

* **When? — Time (with full date format):** It doesn’t require using UTC format. But the timezone has to be the same for everyone that needs to look at the logs.
* **What? — Stack errors**: All exception objects should be passed to the logging system.
* **Where?** — Besides service name as we using MSA. We also need function name, class or file name where the error occurred. — Don’t guess anything, it might waste your time.
* **Who? —** The IP address of the client and user name if any. Make sure don’t use this information to blame your teammates :)

Bear in mind that, logging system is not only for developers. It’s also used by others (system admin, tester…) So, you should consider logging data that everyone can use and understand.

## **5. Consider storing Personally identifiable information (PII) of your end-users** <a href="5-consider-storing-personally-identifiable-information-pii-of-your-end-users" id="5-consider-storing-personally-identifiable-information-pii-of-your-end-users"></a>

Sometimes, you log requests from end-users that contain PII. We need to be careful, it might violate [GDPR](https://gdpr-info.eu).

## Logging approaches in MSA <a href="logging-approaches-in-msa" id="logging-approaches-in-msa"></a>

There are two techniques for logging in MSA. Each service will implement the logging mechanism by itself and using one logging service for all services. Both of them have Good and Not Good points. — _I’m using both these approaches in my project._

1. **Implement Logging in each service**

![](https://miro.medium.com/max/566/1\*4C-xD6SxmfVbZPABmpDyFA.png)

Image for post

With this approach, we can easily define the logging strategy/library for each service. For example, with service written by java we can use Log4j.

The problem with this approach is that it requires each service to implement its own logging methods. Not only is this redundant, but it also adds complexity and increases the difficulty of changing logging behaviour across multiple services.

**2. Implement central Logging service**

![](https://miro.medium.com/max/572/1\*Gw-vGPXh4dABab1Hi-ZRJQ.png)

If you don’t want to implement logging in each service separately. You can consider implementing a central service for logging. This service will help you with processing, formatting and storing log data.

This approach might help to reduce the complexity of your application. However, you might get lost your log data if that service is down.
