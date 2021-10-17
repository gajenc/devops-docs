# Partner Relationship Policy

As part of DIGIT partner relationship policy we provide comprehensive support for the platform and services. Certain agreements also apply to these products.

While DIGIT Platform is open sourced to the world, the source code of all the services are maintained in eGov's GitHub under MIT License. At the same DIGIT allows the state/partner teams to manage the localisation, customization, mdms and configuration as separate private repos in their respective git system. 

**DIGIT Platform - Public Git Repos**

* [**https://github.com/egovernments/core-servic**](https://github.com/egovernments/core-services)[e](https://github.com/egovernments/municipal-services)
* [h**ttps://github.com/egovernments/municipal-services**](https://github.com/egovernments/municipal-services)
* [**https://github.com/egovernments/business-services**](https://github.com/egovernments/business-services)
* [**https://github.com/egovernments/utilities**](https://github.com/egovernments/utilities)****
* ****[**https://github.com/egovernments/frontend**](https://github.com/egovernments/frontend)****
* ****[**https://github.com/egovernments/egov-coexistence**](https://github.com/egovernments/egov-coexistence)****

**State/Partner - Customization Repos (To be created by the state from the Template and add their implementation, CI Pipeline etc)**

* **MDMS**
* **Customization**

DIGIT also helps partners by providing the CI-as-code with which one can replicate the same CI/CD pipelines. The corresponding DevOps repos are also made public to take advantage of our work by cloning and replicating it. But this does not provide any support, it is just sharing our work/experities that we built. Partners can either make use of these repos to build the CI/CD system, or they can have their own CI/CD System and pipeline.

* ****[**https://github.com/egovernments/Train-InfraOps**](https://github.com/egovernments/Train-InfraOps)****
* ****[**https://github.com/egovernments/CIOps**](https://github.com/egovernments/CIOps)****

As DIGIT platform continues to evolve with the new development/enhancements/fixes we keep releasing/publishing the artifact versions along with the changelog that can help the partners to plan the update/upgrade. The release artefacts will be the docker images with the specific versions for all those services like backbone, core, business, municipal and frontend. These images will be available only in **eGov dockerhub repo publically**.  

If in case the partners want to change the code/logic of any of the DIGIT services, partners will have an option to clone/copy any desired git repo into their own git system and modify as their own copy of the service and setup a CI pipeline to build, bake the images, push into **partner's docker image repository** and deploy, in this case the **warranty and support of that service and the dependant changes will become void**. Upon new DIGIT releases partners can still optionally take the code pull down from public repos and merge the delta to partners changed version of service. But the entire ownership of the functionality will be of the partner.   
