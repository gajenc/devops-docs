---
description: Insights on services (pods) readiness, liveness and how to set.
---

# Readiness & Liveness

## What is Probes <a href="what-is-probes" id="what-is-probes"></a>

Determining the state of a service based on readiness, liveness, and startup to detect and deal with unhealthy situations. It may happen that if the application needs to initialize some state, make database connections, or load data before handling application logic. This gap in time between when the application is actually ready versus when Kubernetes thinks is ready becomes an issue when the deployment begins to scale and unready applications receive traffic and send back 500 errors.

Many developers assume that when basic pod setup is adequate, especially when the application inside the pod is configured with daemon process managers (e.g. PM2 for Node.js). However, since Kubernetes deems a pod as healthy and ready for requests as soon as all the containers start, the application may receive traffic before it is actually ready.

## Kubernetes Probes <a href="kubernetes-probes" id="kubernetes-probes"></a>

Kubernetes supports readiness and liveness probes for versions ≤ 1.15. Startup probes were added in 1.16 as an alpha feature and graduated to beta in 1.18 (_WARNING: 1.16 deprecated several Kubernetes APIs. Use this_ [_migration guide_](https://medium.com/dev-genius/upgrading-to-kubernetes-1-16-ad977933694d) _to check for compatibility_).

All the probe have the following parameters:

* `initialDelaySeconds` : number of seconds to wait before initiating liveness or readiness probes
* `periodSeconds`: how often to check the probe
* `timeoutSeconds`: number of seconds before marking the probe as timing out (failing the health check)
* `successThreshold` : minimum number of consecutive successful checks for the probe to pass
* `failureThreshold` : number of retries before marking the probe as failed. For liveness probes, this will lead to the pod restarting. For readiness probes, this will mark the pod as unready.

## Readiness Probes <a href="readiness-probes" id="readiness-probes"></a>

Readiness probes are used to let kubelet know when the application is ready to accept new traffic. If the application needs some time to initialize state after the process has started, configure the readiness probe to tell Kubernetes to wait before sending new traffic. A primary use case for readiness probes is directing traffic to deployments behind a service.

![](https://miro.medium.com/max/60/0\*AvaYbgMkeHJ0Pis8.GIF?q=20)

Kubernetes Readiness Probe

One important thing to note with readiness probes is that it **runs during the pod’s entire lifecycle**. This means that readiness probes will run not only at startup but repeatedly throughout as long as the pod is running. This is to deal with situations where the application is temporarily unavailable (i.e. loading large data, waiting on external connections). In this case, we don’t want to necessarily kill the application but wait for it to recover. Readiness probes are used to detect this scenario and not send traffic to these pods until it passes the readiness check again.

## Liveness Probes <a href="29ef" id="29ef"></a>

On the other hand, liveness probes are used to restart unhealthy containers. The kubelet periodically pings the liveness probe, determines the health, and kills the pod if it fails the liveness check. Liveness checks can help the application recover from a deadlock situation. Without liveness checks, Kubernetes deems a deadlocked pod healthy since the underlying process continues to run from Kubernetes’s perspective. By configuring the liveness probe, the kubelet can detect that the application is in a bad state and restarts the pod to restore availability.

![](https://miro.medium.com/max/60/0\*yicsIyLNZJlDlIsf.GIF?q=20)

Kubernetes Liveness Probes

## **Startup Probes** <a href="1b53" id="1b53"></a>

Startup probes are similar to readiness probes but only executed at startup. They are optimized for slow starting containers or applications with unpredictable initialization processes. With readiness probes, we can configure the `initialDelaySeconds` to determine how long to wait before probing for readiness. Now consider an application where it occasionally needs to download large amounts of data or do an expensive operation at the start of the process. Since `initialDelaySeconds` is a static number, we are forced to always take the worst-case scenario (or extend the `failureThreshold`that may affect long-running behaviour) and wait for a long time even when that application does not need to carry out long-running initialization steps. With startup probes, we can instead configure `failureThreshold` and `periodSeconds` to model this uncertainty better. For example, setting `failureThreshold` to 15 and `periodSeconds` to 5 means the application will get 10 x 5 = 75s to startup before it fails.

## Configuring Probe Actions <a href="configuring-probe-actions" id="configuring-probe-actions"></a>

Now that we understand the different types of probes, we can examine the three different ways to configure each probe.

## **HTTP** <a href="http" id="http"></a>

The kubelet sends an HTTP GET request to an endpoint and checks for a 2xx or 3xx response. You can reuse an existing HTTP endpoint or set up a lightweight HTTP server for probing purposes (e.g. an Express server with `/healthz` endpoint).

HTTP probes take in additional parameters:

* `host` : hostname to connect to (default: pod’s IP)
* `scheme` : HTTP (default) or HTTPS
* `path` : path on the HTTP/S server
* `httpHeaders` : custom headers if you need header values for authentication, CORS settings, etc
* `port` : name or number of the port to access the server

```
livenessProbe:   httpGet:     path: /healthz     port: 8080
```

## TCP <a href="ed8f" id="ed8f"></a>

If you just need to check whether or not a TCP connection can be made, you can specify a TCP probe. The pod is marked healthy if can establish a TCP connection. Using a TCP probe may be useful for a gRPC or FTP server where HTTP calls may not be suitable.

```
readinessProbe:   tcpSocket:     port: 21
```

## Command <a href="0a7d" id="0a7d"></a>

Finally, a probe can be configured to run a shell command. The check passes if the command returns with exit code 0; otherwise, the pod is marked as unhealthy. This type of probe may be useful if it is not desirable to expose an HTTP server/port or if it is easier to check initialization steps via command (e.g. check if a configuration file has been created, run a CLI command).

```
readinessProbe:   exec:     command: ["/bin/sh", "-ec", "vault status -tls-skip-verify"]
```

## Best Practices <a href="best-practices" id="best-practices"></a>

The exact parameters for the probes depend on your application, but here are some general best practices to get started:

* For older (≤ 1.15) Kubernetes clusters, use a readiness probe with an initial delay to deal with the container startup phase (use p99 times for this). But make this check lightweight, since the readiness probe will execute throughout the entire lifecycle of the pod. We don’t want the probe to timeout because the readiness check takes a long time to compute.
* For newer (≥ 1.16) Kubernetes clusters, use a startup probe for applications with unpredictable or variable startup times. The startup probe may share the same endpoint (e.g. `/healthz` ) as the readiness and liveness probes, but set the `failureThreshold` higher than the other probes to account for longer start times, but more reasonable time to failure for liveness and readiness checks.
* Readiness and liveness probes may share the same endpoint if the readiness probes aren’t used for other signalling purposes. If there’s only one pod (i.e. using a Vertical Pod Autoscaler), set the readiness probe to address the startup behaviour and use the liveness probe to determine health. In this case, marking the pod unhealthy means downtime.
* Readiness checks can be used in various ways to signal system degradation. For example, if the application loses connection to the database, readiness probes may be used to temporarily block new requests and allow the system to reconnect. It can also be used to load balance work to other pods by marking busy pods as not ready.

In short, well-defined probes generally lead to better resilience and availability. Be sure to observe the startup times and system behaviour to tweak the probe settings as the applications change.

Finally, given the importance of Kubernetes probes, you can use a Kubernetes resource analysis tool to detect missing probes. These tools can be run against existing clusters or be baked into the CI/CD process to automatically reject workloads without properly configured resources.

* ​[**Polaris**](https://github.com/FairwindsOps/polaris): a resource analysis tool with a nice dashboard that can also be used as a validating webhook or CLI tool.
* ​[**Kube-score**](https://github.com/zegl/kube-score): a static code analysis tool that works with Helm, Kustomize, and standard YAML files.
* ​[**Popeye**](https://github.com/derailed/popeye)**:** read-only utility tool that scans Kubernetes clusters and reports potential issues with configurations.
