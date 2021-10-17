---
description: Exercise to evaluate the DevOps skills (3 Hours)
---

# Exercise 1: (2nd Level)

## **Questions: (0.5 Hours)**

1. What is DevOps according to you, give a sample from your past experience?
2. Explain the workflow from “code-to-production” from your past engagements and what improvements that you want to make?
3. Create an infra estimate for a production grade kubernetes cluster for 50 microservices with the avg of 0.25 Millicore CPUs and 256MB Memory each service. (Public Cloud or Private Cloud)
4. What’s your understanding on overlay networks and which CNI plugin that you have used in the past for the kubernetes cluster?  

## **Exercise: (2.5 Hours)**

**Put all the scripts into a GitHub public repo and each below Tasks into separate folders, the repo should be accessible without any login. **

1. Write a terraform script to provision a VM from any cloud provider using a terraform, commit the resultant tfstate and all related files in GitHub. (10 Marks) (Medium) (20 Min)
2. Write a ansible script to provision a kubernetes cluster, with a single master and 3 worker nodes. (10 Marks) (Medium) (20 Min)
3. Write a simple Jenkins Groovy pipeline script to build a maven project, bake the code into docker container and push the image into docker hub. (15 Marks) (Medium) (20 Min)
4. Create a sample kubernetes manifest with 2 RBAC roles, first is Cluster ReadOnly Access and 2 is only Pod Readonly access. (20 Marks) (Medium) (20 Min)
5. Write a simple dockerfile that builds a Spring boot service. (15 Marks) (Medium) (30 Min)
6. Write a sample Helm chart and add a couple of custom values, and document the Helm Readme to explain the custom configs. (15 Marks) (Medium) (20 Min)
7. Write a kubernetes manifest to allow the external traffic via a couple of approaches, one is via load balancer and another via nodeport.  (15 Marks) (Medium) (20 Min)
