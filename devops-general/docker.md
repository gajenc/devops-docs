---
description: What is container and Docker
---

# Docker

Now that we understand the evolution of IT software infrastructure, we might want to know how we can achieve things like `Microservices Architecture` and `Containerization` that we discussed earlier? A simple answer to this could be, `Docker`.

[Docker](https://www.docker.com) is the world's leading software containerization platform. It encapsulates your microservice into what we call as `Docker container` which can then be independently maintained and deployed. Each of these containers will be responsible for one specific business functionality.

### What do microservices and Docker have to do with? <a href="article_title_0" id="article_title_0"></a>

Microservices are self-contained, independent application units that each fulfill only one specific business function, they can be considered small applications in their own right. What will happen if you create a dozen microservices for your app? And what if you decide to build several microservices with different technology stacks? Your team will soon be in trouble as developers have to manage even more environments than they would with a traditional monolithic application.

There’s a solution, though: using microservices and containers to encapsulate each microservice. Docker helps you manage those containers.

Docker is simply a containerization tool that was initially built on top of Linux Containers to provide a simpler way to handle containerized applications. We’ll review Docker’s advantages and see how it can help us implement microservices.

### Docker Benefits for Microservices in 2020 <a href="article_title_1" id="article_title_1"></a>

Containerization, as an alternative to virtualization, has always had the potential to change the way we build apps. Docker, as a containerization tool, is often compared to virtual machines.

Virtual machines (VMs) were introduced to optimize the use of computing resources. You can run several VMs on a single server and deploy each application instance on a separate virtual machine. With this model, each VM provides a stable environment for a single application instance. Unfortunately, however, when we scale our application we’ll quickly encounter issues with performance, as VMs still consume a lot of resources.

![Hypervisor and multiple virtual machines](https://rubygarage.s3.amazonaws.com/uploads/article_image/file/584/hypervisor-and-virtual-machines.jpg)

The diagram above shows that hypervisor is used to run several operating systems on the same server. In the simplest terms, hypervisor helps reduce the resources required to run several operating systems.

Because microservices are similar to small apps, we must deploy microservices to their own VM instances to ensure discrete environments. And as you can imagine, dedicating an entire virtual machine to deploying only a small part of an app isn’t the most efficient option. With Docker, however, it’s possible to reduce performance overhead and deploy thousands of microservices on the same server since Docker containers require a lot fewer computing resources than virtual machines.

So far we’ve been talking about managing environments for a single app. But let’s assume you’re developing two different projects or you need to test two different versions of the same app. Conflicts between app versions or libraries (for projects) are inevitable in this case, and supporting two different environments on the same VM is a real pain.

#### How Docker Bests Virtual Machines <a href="article_title_2" id="article_title_2"></a>

Contrary to how VMs work, with Docker we don’t need to constantly set up clean environments in the hopes of avoiding conflicts. With Docker, we know that there will be no conflicts. Docker guarantees that application microservices will run in their own environments that are completely separate from the operating system.

Thanks to Docker, there’s no need for each developer in a team to carefully follow 20 pages of operating system-specific instructions. Instead, one developer can create a stable environment with all the necessary libraries and languages and simply save this setup in the Docker Hub (we’ll talk more about the Hub later). Other developers then only need to load the setup to have the exact same environment. As you can imagine, Docker can save us a lot of time.

If you Google you’ll likely find other benefits of using Docker, such as rapid development speed and freedom of choice in terms of technology stacks. But we’d say that these other benefits that people talk about have nothing to do with Docker itself. It’s actually the microservices-based architecture that lets you rapidly develop new features and choose any technology stack you like for each microservice.

We can sum up Docker’s advantages as the following:

* Faster start time. A Docker container starts in a matter of seconds because a container is just an operating system process. A virtual machine with a complete OS can take minutes to load.
* Faster deployment. There’s no need to set up a new environment; with Docker, web development team members only need to download a Docker image to run it on a different server.
* Easier management and scaling of containers, as you can destroy and run containers faster than you can destroy and run virtual machines.
* Better usage of computing resources as you can run more containers than virtual machines on a single server.
* Support for various operating systems: you can get Docker for Windows, Mac, Debian, and other OSs.

Let’s now take a look at Docker’s architecture to find out how exactly it helps us develop microservices-based applications.

### Docker’s Architecture <a href="article_title_3" id="article_title_3"></a>

To better understand how Docker works and how to use Docker, we’ll consider a very simple microservice. There are many microservices architecture examples, but we’ve created our own for the purpose of this article:

![Microservice-based app built with Docker containers](https://rubygarage.s3.amazonaws.com/uploads/article_image/file/585/microservice-app-built-with-docker-1.jpg)

The application (microservice) shown in the diagram consists of only three services and lets you implement a blog for your website. (All the code for this small app is located on [GitHub](https://github.com/sparrow/docker_wordpress), so you can check it out for yourself). Each service – Nginx (web server), MySQL (database), and Wordpress (blogging engine) – is encapsulated in a container.

The example above doesn’t cover the entire Docker architecture, though, as containers are only one part of it. The Docker architecture includes three chief components – images, containers, and registries. We’ll review each component one by one. But before you can actually use Docker for development, you’ll need to [install Docker](https://docs.docker.com/engine/getstarted/step_one/) on your computer.

To make Docker containers work together, we must first register each of them in the docker-compose file, as Docker Compose coordinates all services.

```
version: '2'

services:
nginx: 
  build: nginx
  restart: always
  ports:
  - 8080:80
  volumes_from:
  - wordpress

wordpress:
  image: wordpress:php7.1-fpm-alpine
  environment:
  WORDPRESS_DB_HOST: mysql
  WORDPRESS_DB_PASSWORD: example

mysql:
  image: mariadb
  environment:
  MYSQL_ROOT_PASSWORD: example
  volumes:
  - ./demo-db:/var/lib/mysql
```

[view raw](https://gist.github.com/sparrow/0d4e921bbf57d4b8ac34d112637cae54/raw/4c987b6f104a0b3f91f15bcdfe64813d91be0b91/docker-compose.yml)[docker-compose.yml](https://gist.github.com/sparrow/0d4e921bbf57d4b8ac34d112637cae54#file-docker-compose-yml) hosted with ❤ by [GitHub](https://github.com)

Let’s clarify what’s going on in docker-compose.yml.

We must pay attention to three elements of this file. First, we must specify the app services – “nginx,” “wordpress,” and “mysql.” Second, we must indicate images – note the “image” attribute under services. Lastly, we must specify one more attribute – “volumes.”

#### Docker Images <a href="article_title_4" id="article_title_4"></a>

Docker containers aren’t created out of thin air. They’re instantiated from Docker images, which serve as blueprints for containers and are the second component in the Docker architecture. To run a Docker image, we have to use Dockerfiles.

Dockerfiles are just text files that explain how an image should be created. Remember how in docker-compose we didn’t specify an image for Nginx? We didn’t want to simply use a ready-made Nginx image, so we wrote “build.” By doing this, we told Docker to build the Nginx image and apply our own configurations. And to instruct Docker about what image must be used with what configurations, we use Dockerfiles.

Here’s an example of the Nginx Dockerfile in our small application:

```
FROM nginx:alpine

COPY site.conf /etc/nginx/conf.d/default.conf
```

[view raw](https://gist.github.com/sparrow/807eae74537e1df2e8f15cc5b210dc17/raw/d438f33f8a3d0c76d6ad372ecceba96168e97721/dockerfile)[dockerfile](https://gist.github.com/sparrow/807eae74537e1df2e8f15cc5b210dc17#file-dockerfile) hosted with ❤ by [GitHub](https://github.com)

As you can see, we gave only two instructions to Docker. The first line specifies the base image to create containers from. In our example, the base image for the Nginx container is “nginx:alpine.” It’s possible to create images with very specific versions of libraries or languages. The second line tells Docker where to look for configurations. Usually, we place such files in the same directory where a Dockerfile is stored.

Here’s an important detail: Docker images never change once pushed to a registry. We can only pull an image from the Docker Hub, change it, and then push back a new version. Every new container will be instantiated from the same base image we’ve specified in docker-compose.yml or the Dockerfile. Because Docker conceals the container environment from the operating system, you can use a specific version of a library or programming language that will never conflict with the system version of the same library/language on your computer.

#### Docker Volumes <a href="article_title_5" id="article_title_5"></a>

In the docker-compose file, each service also has an attribute called “volumes.” Volumes in Docker are the way we can handle persistent data that’s used by containers. Different containers can access the same volumes. In the docker-compose file, for the Nginx service we used the attribute “volumes_from,” which tells the Nginx container that it must look in the WordPress volumes.

In the event that several containers are located on different servers but still need access to the same data, they can share volumes. This is why we should use the “volumes_from” attribute. Docker will manage volumes for us.

#### Image Registries (Repositories) <a href="article_title_6" id="article_title_6"></a>

Until now we’ve only mentioned images and containers – two basic components of Docker’s architecture. But you might be wondering where those images are located. All containers in our example app are built from standard images stored at Docker Hub. Here are links to the [Nginx](https://hub.docker.com/\_/nginx/), [Wordpress](https://hub.docker.com/\_/wordpress/), and [MySQL](https://hub.docker.com/\_/mariadb/) base images.

Registries are another component of the Docker ecosystem, and the Docker Hub is a great example of a registry. Registries are the places (repositories) where all images are stored. We can push and pull images to and from a registry; build our own unique registry for a particular project; and use images from registries to build our own base images.

At this point, we’ve talked about all three basic components – containers, images, and image repositories. The Docker architecture, however, includes other important components – namespaces, control groups, and Union filesystems (UnionFS). Namespaces let us separate containers from one another so they can’t access each other’s states; control groups are necessary to manage hardware resources among containers; and UnionFS helps to create building blocks for containers.

We should mention, however, that these additional components weren’t actually created by Docker: they were available before Docker in Linux Containers. Docker just uses the same concepts for container management.

We’ve introduced a lot of new information so far, so let’s boil it down to the following key points:

* Dockerfiles contain important instructions to work with Docker images, which are used for constructing containers.
* Each app microservice must have a separate Dockerfile with specific instructions for each image.
* Docker containers are always created from the specified Docker images (this is how Docker ensures consistency across environments).
* To instantiate or delete containers, you need to run Docker commands using the Docker command line interface. You must use the Docker CLI to manage Docker, as there’s no built-in graphical user interface.
* You can use pre-built Docker images that are stored in public registries such as Docker Hub, and can change these base images via configurations to adapt them for your applications.
* You need to register all application microservices in the docker-compose file.

#### Extending the Architecture of a Microservices-Based App with Docker <a href="article_title_7" id="article_title_7"></a>

The initial application architecture we provided in our example can easily be extended with other services. Let’s say we wanted to cache responses to our app. Let’s add a service called Varnish. Varnish will let us cache HTML pages, images, CSS, and JavaScript files that are often requested by users. Our updated microservice-based app will look like this:

![Extended microservice-based app built with Docker containers](https://rubygarage.s3.amazonaws.com/uploads/article_image/file/586/microservice-app-built-with-docker-2.jpg)

The code below shows an extended Docker Compose file with a Varnish service:

```
version: '2'

services:
  varnish:
    build: varnish
    ports:
      - 80:80
    depends_on:
      - nginx

  nginx:
    build: nginx
    restart: always
    volumes_from:
      - wordpress

  wordpress:
    image: wordpress:php7.1-fpm
    environment:
      WORDPRESS_DB_HOST: mysql
      WORDPRESS_DB_PASSWORD: example
    depends_on:
      - mysql

  mysql:
    image: mariadb
    environment:
      MYSQL_ROOT_PASSWORD: example
    volumes:
      - ./demo-db:/var/lib/mysql
```

[view raw](https://gist.github.com/sparrow/39188c67c4de07fd064f491cf27df621/raw/419db978f33e108bbaf00fd67a3b58d9045f6898/docker-compose-2.yml)[docker-compose-2.yml](https://gist.github.com/sparrow/39188c67c4de07fd064f491cf27df621#file-docker-compose-2-yml) hosted with ❤ by [GitHub](https://github.com)

We need to include one more service in the docker-compose.yml file, configure it, and specify ports with volumes through which we can connect to the app. If we decide not to use Varnish, we can simply remove it from docker-compose and update our configurations. Again, you can follow along with the [simple WordPress container](https://github.com/SergeC/docker_wordpress) project on GitHub to see the updated parts of the app for yourself.

That’s nearly all you need to know to manage containers with Docker. There’s one thing that might still be bugging you, though: How can we manage hundreds or thousands of Docker containers across multiple servers? The final section will help you answer this question.

### Managing Docker-based Apps with Container Orchestration Systems <a href="article_title_8" id="article_title_8"></a>

Docker lets us deploy microservices one by one on a single host (server). A small app (like our example app) with less than a dozen services doesn’t need any complex management. But it’s best to be ready for when your app grows. If you run several servers, how can you deploy a number of containers across all of them? How can you scale those servers up and down? Docker’s ecosystem includes Container Orchestration Systems to address these problems.

A Container Orchestration System is an additional tool you should use with Docker. Until the middle of 2016, Docker didn’t actually provide any specific way to manage applications built of thousands of microservices. But now there’s Docker Swarm, a built-in framework for orchestrating containers.

Docker now comes with a special mode – swarm mode – that you can use to manage clusters of containers. Docker Swarm lets you use the Docker CLI to run swarm commands, so you can easily initialize groups of containers and add and remove containers from those groups. Besides Docker Swarm, there are several other container orchestration managers you could consider as well:

* Kubernetes, a containers cluster manager. You can run Kubernetes on your own servers or in the cloud.
* DC/OS, a special project that gives you an advanced graphical user interface to manage Docker containers.
* Nomad Project, software that can work with Docker to help you deploy and manage your applications on Amazon ECS, DigitalOcean, the Azure Container Service, or the Google Cloud Platform.

If you’re interested in cloud solutions that can help you run Dockerized applications and, more importantly, orchestrate containers, you should consider the following:

* Google Cloud Platform, with support for Kubernetes. There’s also a cloud manager called Google Container Engine that is based on Kubernetes.
* Amazon ECS. Amazon Web Services enable you to use the Elastic Compute Cloud (EC2) service to run and handle Docker containers.
* Azure Container Service is a hosting solution similar to Amazon ECS, and supports various frameworks for orchestrating dockerized applications including Kubernetes, DC/OS with Mesos, and Docker Swarm.

Although each of these cloud solutions lets you run dockerized apps in the cloud, each offers slightly different services. The Google Cloud Platform is tailored for Kubernetes, while the Azure Container Service can work with Kubernetes, DC/OS, or Docker’s standard Swarm. Amazon ECS is more like a Platform as a Service – it automatically scales and manages infrastructure for Docker containers.

Using microservices and containers is considered the proper modern way to build scalable and manageable web applications. If you don’t containerize microservices, you’ll face a lot of difficulties when deploying and managing them. That’s why we use Docker: to avoid any troubles when deploying microservices. Add in a Container Orchestration System, and you’ll be able to handle your dockerized applications with no limits.

To understand Docker in a little more depth, let us take the same example of an ecommerce website that we discussed earlier. We know that it will have multiple operations and services such as account creation, displaying product catalog, building and validation shopping cart and so on. In a microservice architecture, all these can be treated as microservices and encapsulated in a Docker container. But why would you do that?

One of the reasons would be to ensure consistency between development and production environment. For instance, consider that three developers are working on this application. Each of them having their environments. So one developer might be running `Windows` OS on his machine, while the second developer could be running `Mac` OS and the third developer would prefer a `Linux` based OS. Each of them would take hours of efforts to install the application in their respective development environments and an additional set of efforts to later deploy the same app on the cloud. This is not smooth, and there is a lot of friction to port such applications to cloud infrastructure.

With Docker, you can make your application independent of the host environment. Since you have microservices architecture, you can now encapsulate each of them in Docker containers. Docker containers are lightweight, resource isolated environments through which you can build, maintain, ship and deploy your application.

**Advantages**

1. Docker is a popular evolving software with excellent community support and built for microservices
2. It is lightweight when compared to VMs making it cost and resource effective
3. It provides uniformity across development and production environments making it a suitable fit for building cloud-native applications
4. It provides facilities for continuous integration and deployment
5. Docker is not going anywhere; it provides integration with popular tools and services such as AWS, Microsoft Azure, Ansible, Kubernetes, Istio and lot more

**Conclusion:**

Containers have become the standard output of the development process, and Kubernetes has emerged as the standard for container orchestration platforms. With Kubernetes, containers can be managed by clusters in public cloud, hybrid cloud, and even in a multi-cloud environment.
