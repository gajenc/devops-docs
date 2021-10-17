# Distributed Tracing

## Introduction <a href="introduction" id="introduction"></a>

Distributed tracing is a method used to profile and monitor applications, especially those built using a microservices architecture. Distributed tracing helps pinpoint where failures occur and what causes poor performance.

​[OpenTracing](https://opentracing.io) has been a key capability when it comes to microservices-based distributed systems like DIGIT. We’ll start with the introduction of OpenTracing, explaining what it is and why it is important We shall also set up Jaeger and learn to use it for monitoring and troubleshooting.

## Drift to Microservice Architecture <a href="drift-to-microservice-architecture" id="drift-to-microservice-architecture"></a>

​[Microservice](https://en.wikipedia.org/wiki/Microservices) Architecture has now become the obvious choice for application developers. In the Microservice Architecture, a monolithic application is broken down into a group of independently deployed services. In simple words, an application is more like a collection of microservices. When we have millions of such intertwined microservices working together, it’s almost impossible to map the inter-dependencies of these services and understand the execution of a request.

In case of a failure in a monolithic application, it is much easier to understand the path of a transaction and do the root cause analysis with the help of logging frameworks. But in a microservice architecture, logging alone fails to deliver the complete picture.

Is this service the first one in the call chain? How do I span all these services to get insight into the application? With questions like these, it becomes a significantly larger problem to debug a set of interdependent distributed services in comparison to a single monolithic application, making OpenTracing more and more popular.

## OpenTracing <a href="opentracing" id="opentracing"></a>

The _OpenTracing_ API provides a standard, vendor-neutral framework for instrumentation. This means that if a developer wants to try out a different distributed tracing system, then instead of repeating the whole instrumentation process for the new distributed tracing system, the developer can simply change the configuration of the Tracer.

Here are some basic terminologies of Opentracing:

**Span —** It represents a logical unit of work that has an operation name, the start time of the operation, and the duration.

**Trace —** A Trace tells the story of a transaction or workflow as it propagates through a distributed system. It is simply a set of spans sharing a _TraceID_. Each component in a distributed system contributes its own span.

![](https://gblobscdn.gitbook.com/assets%2F-MERG_iQW5oN4ukgXP8K%2Fsync%2F98632f53e25e1531a5adb1fa442b0ca7fb890f80.png?alt=media)

OpenTracing is a way for services to “_describe and propagate distributed traces without knowledge of the underlying OpenTracing implementation._”

Let us take the example of a service like egov-property service (or any other DIGIT service). A service like this requires many other microservices to check that the location is available, proper payment credentials are received, and enough details exist for the ULB to process the property tac. If either one of those microservice fails, then the entire transaction fails. In such a case, having logs just for the main property service wouldn’t be very useful for debugging. However, if you were able to analyze each service you wouldn’t have to scratch your head to troubleshoot which microservice failed and what made it fail.

In real life, applications are even more complex and with the increasing complexity of applications, monitoring the applications has been a tedious task. Opentracing helps us to easily monitor:

* Spans of services
* Time taken by each service
* Latency between the services
* Hierarchy of services
* Errors or exceptions during execution of each service.

## Jaeger: A Distributed Tracing System by Uber <a href="jaeger-a-distributed-tracing-system-by-uber" id="jaeger-a-distributed-tracing-system-by-uber"></a>

_Jaeger_ is used for monitoring and troubleshooting microservices-based distributed systems, including:

* Distributed transaction monitoring
* Performance and latency optimization
* Root cause analysis
* Service dependency analysis
* Distributed context propagation

### Major Components of Jaeger <a href="major-components-of-jaeger" id="major-components-of-jaeger"></a>

**Jaeger Client Libraries** — Jaeger clients are language-specific implementations of the [OpenTracing API](http://opentracing.io).

**Agent** — The Jaeger agent is a network daemon that listens for spans sent over UDP, which it batches and sends to the collector. It is designed to be deployed to all hosts as an infrastructure component. The agent abstracts the routing and discovery of the collectors away from the client.

**Collector** — The Jaeger collector receives traces from Jaeger agents and runs them through a processing pipeline. Currently, the pipeline validates traces, indexes them, performs transformations, and finally, stores them. Jaeger’s storage is a pluggable component which currently supports [Cassandra](https://www.jaegertracing.io/docs/1.8/deployment#cassandra), [Elasticsearch](https://www.jaegertracing.io/docs/1.8/deployment#elasticsearch), and [Kafka](https://www.jaegertracing.io/docs/1.8/deployment#kafka).

**Query** — Query is a service that retrieves traces from storage and hosts a UI to display them.

**Ingester** — Ingester is a service that reads from Kafka topic and writes to another storage backend (Cassandra, Elasticsearch).

### Running Jaeger in a Docker Container <a href="running-jaeger-in-a-docker-container" id="running-jaeger-in-a-docker-container"></a>

1. First, install Jaeger Client on your machine:
2. Now, let’s run Jaeger backend as an all-in-one Docker image. The image launches the Jaeger UI, collector, query, and agent:

**TIP**: To check if the docker container is running, use: _**Docker ps.**_

Once the container starts, open [_http://localhost:16686/_](http://127.0.0.1:16686) to access the Jaeger UI. The container runs the Jaeger backend with an in-memory store, which is initially empty, so there is not much we can do with the UI right now since the store has no traces.

## Creating Traces on Jaeger UI <a href="creating-traces-on-jaeger-ui" id="creating-traces-on-jaeger-ui"></a>

**1. Create a Python program to create Traces**

Let’s generate some traces using a simple python program. You can clone the _Jaeger-Opentracing_ repository given below for a sample program that is used in this blog_._

The Python program takes a movie name as an argument and calls three functions that get the cinema details, movie showtime details, and finally, book a movie ticket.

It creates some random delays in all the functions to make it more interesting, as, in reality, the functions would take a certain time to get the details. Also, the function throws random errors to give us a feel of how the traces of a real-life application may look like in case of failures.

Here is a brief description of how OpenTracing has been used in the program:

* Initializing a tracer:
* Using the tracer instance:
* Starting new child spans using start_span:
* Using Tags:
* Using Logs:

**2. Run the python program**

Now, check your Jaeger UI, you can see a new service “booking” added. Select the service and click on “Find Traces” to see the traces of your service. Every time you run the program a new trace will be created.

![](https://gblobscdn.gitbook.com/assets%2F-MERG_iQW5oN4ukgXP8K%2Fsync%2F6ca9157cbfc7cdf99ef14c92b8e1dce58702e1af.png?alt=media)

You can now compare the duration of traces through the graph shown above. You can also filter traces using “Tags” section under “Find Traces”. For example, Setting “error=true” tag will filter out all the jobs that have errors.

To view the detailed trace, you can select a specific trace instance and check details like the time taken by each service, errors during execution and logs.

![](https://gblobscdn.gitbook.com/assets%2F-MERG_iQW5oN4ukgXP8K%2Fsync%2F5560ec50f10b7e1d1c797dda1c4888c4cce39920.png?alt=media)

## Conclusion <a href="conclusion" id="conclusion"></a>

In this blog, we’ve described the importance and benefits of OpenTracing, one of the core pillars of modern applications. We also explored how distributed tracer Jaeger collect and store traces while revealing inefficient portions of our applications. It is fully compatible with OpenTracing API and has a number of clients for different programming languages including Java, Go, Node.js, Python, PHP, and more.



## References

* [https://www.jaegertracing.io/docs/1.9/](https://www.jaegertracing.io/docs/1.9/)
* [https://opentracing.io/docs/](https://opentracing.io/docs/)
