# DIGIT DevOps overview

Let's talk about an end-to-end example of the DevOps practice in product/platform [**DIGIT**](https://docs.digit.org/digit-overview), which is a open source and a microservices platform come product, it is built for a scale and cloud agnostic, that means it can be run any cloud/on-premise data centers, etc. DIGIT in its entire architecture and technology, uses completely free and open source tools to stand out as a completely free Open source platform. 

#### **Let's understand the tools/process Involved to coordinate and collaborate internally during the development, testing, packaging and releasing it to production.**

To start with the product roadmap/planning, we use **Atlassian JIRA** to manage the Agile planning, and **Jenkins (CI/CD)** as a build and release tool. The source code is maintained in **GitHub**. The platform/product services are baked into **Docker** containers,  deployed-and-orchestrated on **Kubernetes**. These tools and interlinking of these tools play a vital role in achieving DevOps practice.

The goal here is to simulate a real-life situation using DIGIT DevOps while incorporating all of the moving parts of a true production system. This allows us to focus on the DevOps process and de-emphasize the code details. The following topics are included:

* **Version Control** – We use Git as our version control repository. All source code for the DIGIT is available here [**GitHub**](https://github.com/egovernments).
* **Build Process** – Whenever source code is pushed to the shared Git remote repository, it is compiled and unit/build tests are run. A set of artifacts are created that are used in the qa/release process to deploy DIGIT Services across respective environments.
* **Vulnerability/Code assessment -** as code changes/committed certain validations are mandated to ensure efficient review checks happen before the approval to ensure the code quality and vulnerabilities are eliminated.
* **Unit/Build Test** – DIGIT uses a set of unit tests to validate the code as it is being developed. Some unit tests are mocked and executed every time the code is checked into version control. Other unit tests hit the database. However, some tests may be too lengthy and slow to be run every time the code is checked in. We refer to these tests as integration tests, and they can be run nightly or on-demand.
* **Automated QA Testing** – A set of automated QA tests has been developed to ensure that we have not broken any existing features. These tests are run in the build environment  each night as part of the scheduled time window.
* **Load Testing** – As we proceed with the development for a release, we run a set of load tests in the build environment to ensure that we are meeting our performance requirements. This also helps us fine-tune infra sizing and capacity for the proper cost estimation and capacity planning. 
* **Environment** 
  * The **production** env is where DIGIT is deployed by our customers/partners. 
  * **UAT** is a production-like env used for ensuring by our delivery teams. 
  * **QA** environment is used for integration testing, UAT, automated QA testing, and load testing. Software is released into the **QA** environment each day, where these tests are then executed. 
  * **Dev** environment is for the developers to frequently integrate changes from feature branches and DevTest before releasing it to QA. 
* **Infrastructure as Code** – DIGIT runs on kubernetes, and the base infra to provision kubernetes can be on any cloud, we use **Infra-As-Code** (IaC) tools like terraform or ansible to provision infra by defining compute, storage, DB and network infrastructure through source code that can then be treated just like any software system. Such code can be kept in source control to allow auditability.
* **Release Process** – A release pipeline is used to gather the artifacts created in the build process and to deploy them to the respective environments.

In this section, we discuss one possible approach to delivering software via DevOps . It is important that you understand this so that you can see why we do what we do in DevOps. The diagram below depicts our DevOps implementation, and we will be referring to this diagram throughout.

![Versioning & CICD](<../.gitbook/assets/image (6).png>)

### Environments in Details:

The required machines, network, storage, database, and other infrastructure elements are defined in the source code and version controlled. This approach is known as **Infrastructure as Code** and is a key part of a DevOps implementation. Since we are using a variety of cloud-based solutions, it is very easy for us to provision environments. Since everything is defined in the source code, provisioning an environment can be done in a snap . We use the following environments:

* **Dev** – Each day, the code base from the **Feature** branch is deployed to this environment. Integration tests are run here, since this environment allows them to hit infrastructure with continuous changes from other developers (**Dev inprogress)**. 
* **QA** – Upon working feature complete, the code base from the **Develop** branch is deployed here upon **DevDone**. This is done on demand by the QA engineers conducting the testing. Automated QA tests are also run nightly in this environment.
* **UAT** - Upon **QA Sign-off**, the code base from the **master** branch is deployed here on demand for the UAT Approval. This is the environment used by product owners/Delivery teams to the acceptance testing.
* **Production** – The code base from the **master** branch is deployed here upon release testing sign off with the overall release items. This is the environment used by our end users.

![](<../.gitbook/assets/image (11).png>)

### Version control & Branching

We follow an Agile approach using Scrum and two-week sprints. We typically release changes to UAT at the end of each sprint and the main release to production would be once in every quarter.

Each release uses a version number. We use [Semantic Version](http://semver.org) numbers for our services, which gives us version numbers like **1.0.2**. Each time a build is run, the build number is incremented and added to the end of the version number. Using the example above, the fifth build  would result in a version number of **1.0.2-#5**. This version number is displayed on the services. 

This means that we always know the version of the software we are running and can trace it back to version control. 

Any release is a bundle of services with specific versions and finally release with the larger release platform/product level version number. (Eg: DIGIT 2.0 (15-June-2020) and subsequent releases within the same year gets incremented. 



![](<../.gitbook/assets/image (9).png>)

We use Git for our version control. The **master** branch is where our production code resides, and we have a **develop** branch for the sprint items. When the sprint ends, we perform a pull request from the **Develop** branch into the **master** branch.

Each user story has a **feature** branch as well. When a developer has finished working on a user story, a pull request is made into the **develop** branch.

![Branching & Gitflow](<../.gitbook/assets/image (10).png>)

### Development & Testing Flow

![](https://lh4.googleusercontent.com/X3POEf3aQAwSH6VPHWdHbkjgRyHnwMnKbPcS-JxjRMyg0MJHyaFNtHNk7O25V4sP8wFmIhoJGvuIIVoxwMWp2LyzrfH5QfCtZUviGONfWYuNzsFW9ptLU1yqUnBC8sxEF2VBmO5d)

When work is begun on a story, a new branch is created from the develop branch. All development is done on the developer's local workstation, including the development of unit and integration tests.

A Unit Test is a quick running test that is typically mocked so it can run quickly. Integration tests are implemented using the unit test framework but typically include infrastructure such as the database. Integration tests can be run nightly to save build time.

When a story is ready for QA testing, a pull request into the develop branch is made. This is where peer **code reviews **are conducted, also the **static code analysis**, **vulnerability assessment,** etc. Any validation failures are addressed here and update the PR.

Once the pull request is approved against above mentioned checks, the code is merged into the **develop** branch, and the **continuous integration build** is invoked. Any broken unit tests are addressed at this point.

The changes can optionally be tested by the developers with ongoing development of dependent changes in  **Dev** environment. The **QA can** **reviews** if the story is Dev Done. If there are issues to address, the developer makes the required changes.

The story is actually tested in the **QA** environment. The **product owner** also **reviews** the story implementation as soon as possible. If there are issues to address, the developer makes the required changes and QA Tests it here.

At the **end of the sprint**, a pull request is made from the **develop branch to the master** branch. This gives the team the chance to review all code that was changed in the sprint. All changes will be accumulated in **UAT** for the Release testing. Once all the Sprints for a release are completed and changes are merged to master, the release testing happens here. Any Alpha/Beta testing can also happen from this env.

### Manual & Automated Testing

As the sprint progresses, a set of manual tests are written. These tests are attached to the story as a test document.

Stories are tested during the sprint and are considered part of the **Definition of Done**.

A suite of automated regression tests are maintained as the sprint unfolds. Over time, the automated regression tests grow and become an important part of product quality.

We use Postman tests/Selenium/Cypress as our automated testing framework because it is open source. We use cloud resources to host our automated QA test execution agent.

### Build and Release pipelines

This is where we implement **Continuous Integration** and **Continuous Deployment**.

We considered a CI/CD flow that mainly follows these steps:

* A _Feature Branch_ (FB) is created from develop branch. The developer will add the code and periodically synchronize and merge the other changes from develop to FB until the desired code changes are implemented.
* A _Pull Request_ (PR) is created when all the work in a _FB_ is done
* The _PR_ is reviewed and validated when it passes all the unit tests, integration, e2e, etc.
* Finally, if the _PR_ gets the green light after passing all the validation, the _FB_ is merged to develop and the new version is deployed in QA.

With that flow we wanted the developer not to have to worry about anything and that the CI/CD was transparent and just worry about the _PR_ status to obtain a green flag to merge.

We also wanted to take into account the following for each _PR_:

* A new CI pipeline is triggered to build the code and run all the tests ensuring to keep the _FB_ in a ready to merge state
* The_ PR_ is deployed to a **Dev** (_Preview Environment) _that gives you faster feedback for your changes before they are merged and released and allows you to avoid having human approval inside the  pipeline to speed up delivery of changes merged to develop.

When a _PR_ is merged to the Develop branch:

* A new/intended semantic version number is generated
* New versioned artifacts are published including: docker images, helm charts and any language specific artifacts (e.g. libraries, jar files, npm packages, etc)
* The new version is promoted to QA.
* When all the testing is done, the changes are merged to master.

When a _PR_ is merged to the Master branch:

* New release version artifacts from the master branch are created, tested, published and deployed on UAT. 
* When the release testing and certification is done, the changes are merged to master.
* The new version is subsequently promoted to Prod.
* The new version is tagged from master branch in git repository.

A build is configured for the master and develop branches. The build receives the latest version of the source code, compiles it, and creates artifacts. These artifacts are used to release into various environments. We do not deploy from version control; we deploy using artifacts. Builds also run unit tests.

## More on CI/CD:

![](https://lh3.googleusercontent.com/B_UxLt1mE3vmptMZMY5hpU60xpv2mHUZJcBOveDqHp1iRzzJUKX40\_G3kd-E5VADhp9ziOFudQ73q1UtjwbwpmUQTVvbdqht3EWdUnx-FwOvAQBNM5qwqeBHqqyFDKG3BHdtAaY8)

When a team has successfully completed a set of user stories that meet the definition of done and are potentially shippable, it is time to actually deploy the software. This step can be a challenge to many firms.

Agile, Scrum and Kanban combined Business Analysts, Developers and Quality Assurance Engineers into a single team whose focus is to get small batches of features truly **Done** and ready to ship. DevOps extends this idea and includes the following points:

* Software Operations is included as part of the delivery team throughout the whole development process. 
* A high level of automation is used as part of the delivery cycle from the very day development starts.
* A mindset change is made to accept an investment in these principles.

DevOps strives for software releases without fear that are easy and can be done at virtually any time.

Much of implementing a successful DevOps process involves automation of as many processes as possible. Anything that is left manual is error prone and slows the process down. 

A typical DevOps Pipeline is shown in the diagram. This is an automated process that is performed day-by-day as the sprint progresses. Each time code is checked in, a Build is executed that Compiles the codebase, runs unit tests, Code analysis and, when successful, packages the code into a Release Artifact. This is known as **Continuous Integration (CI)**.

On a scheduled basis, the Release Artifact is deployed from master branch into a Production Like Environment known as the **UAT** Environment. Here more tests are executed that include Integration tests that hit a database, Automated QA Tests, performance, security and Load Tests. 

When all tests pass, and when we have a Release Candidate, we are ready to deploy to production. This may require additional Environments, manual steps and approvals. This is known as **Continuous Deployment (CD).**

The crucial point here is the Release Artifact that was developed in the sprint, was compiled, and unit tested hundreds of times. This same artifact was deployed to the QA Environment where it was Integration Tested, QA Tested and Load Tested dozens of times. This exact same artifact following the same process is ultimately deployed to UAT and Production. 

If you have a war room every time you do a production deployment, it means you are expecting problems and have deployments that are far too “exciting”. The DevOps movement is focused on solving this problem. 
