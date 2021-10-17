---
description: Inside microservices
---

# Microservices

## What are microservices?

“**Microservice architectural style** is an approach to developing a single application as a **suite of small services**, each **running in its own process** and **communicating with lightweight mechanisms**, often an HTTP resource API. These services are **built around business capabilities** and **independently deployable** by fully automated deployment machinery. There is a **bare minimum of centralized management** of these services, which may be written in **different programming languages** and use **different data storage technologies**.”

### Reasons behind to use microservices:

* For a large application, it is difficult to understand the complexity and make code changes fast and correctly, sometimes it becomes hard to manage the code.
* For small change, the whole application needs to be built and deployed.
* For small change, the whole application needs to be built and deployed.

![Microservice - Middleware & Design Patterns](<../.gitbook/assets/image (2).png>)

### **Benefits of microservices:**

* **Small modules**: Application is broken into smaller modules which are easy for developers to code and maintain
* **Easier Process Adoption**: By using microservices, new Technology & Process Adoption becomes easier. You can try new technologies with the newer microservices that we use.
* **Independent scaling**: Each microservice can scale independently via X-axis scaling (cloning with more CPU or memory) and Z-axis scaling (sharding), based upon their needs.
* **Unaffected**: Large applications remain largely unaffected by the failure of a single module.
* **DURS** :Each service can be independently DURS (deployed, updated, replaced, and scaled).

### Drawbacks

* **Deployment requires more effort. **The operation of a microservice system often requires more effort, since there are more deployable units that must each be deployed and monitored. Changes to interfaces must be implemented so that an independent deployment of individual microservices is still possible.
* **Testing must be independent. **Since all microservices must be tested together, one microservice can block the test stage and prevent the deployment of the other microservices. There are more interfaces to test, and testing has to be independent for both sides of the interface.
* **Difficult to change multiple microservices. **Changes that affect multiple microservices can be more difficult to implement. In a microservice system, changes require several coordinated deployments.
