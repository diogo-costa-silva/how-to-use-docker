# How To Use Docker - A Complete Guide

## What is Docker?

Docker is a tool that lets you deploy your applications in what are known as containers, i.e. isolated environments on a host machine. Containers allow you to isolate all the dependencies of an application and run it without generating conflicts with your system.
Docker is a development tool and a virtualization technology used to simplify and automate the deployment of applications by putting it, along with its dependencies and settings, into a tidy container. A container, on the other hand, is a standard way to package an application that contains all the configuration files and other dependencies to operate the application. This eliminates problems like conflicting versions, inconsistent environments, and frustrating nights spent chasing mysterious bugs.
Docker is a tool designed to make it easier to create, deploy, and run applications by using containers. Containers allow a developer to package up an application with all of the parts it needs, such as libraries and other dependencies, and ship it all out as one package.

## Why use Docker?

> **Portability:**  Docker containers run consistently across various environments, from development to production.

> **Efficiency:**  Containers share the host OS kernel, minimizing overhead and maximizing resource utilization.

> **Isolation:**  Each container encapsulates its dependencies, ensuring application independence and preventing conflicts.

> **Scalability:**  Docker enables easy scaling by swiftly replicating containers, and responding dynamically to workload demands.

> **Version Control:**  Docker’s versioning system allows easy rollback to previous container states, promoting code stability and reproducibility.

## Docker Containers vs. Virtual Machines

Virtual Machines (VMs) and containers are both technologies used to isolate and run applications, but they do so in fundamentally different ways. Here's a breakdown to clarify their differences.

**Virtual Machines: The Independent Apartments**

Imagine you want to run applications that require different operating systems. Using a VM is like renting an entire apartment building for each application. Inside a VM, the application operates within a guest operating system, which sits on a virtual hardware layer. This setup is hosted on your actual machine (the host). For example, you can run a Windows VM on a Linux host to use Windows-specific software. However, this independence comes with a cost: VMs are heavy, consuming substantial amounts of your computer's resources like memory and processing power.

**Docker Containers: The Shared Living Space**

In contrast, containers offer a more resource-efficient approach. Think of a container as sharing an apartment building; each application has its own space, but they share common infrastructure, like the heating system or water supply. Technically, this means containers share the host machine's operating system kernel, which allows them to be lighter and faster than VMs. They provide isolated environments for applications but don't require a separate operating system for each. However, because of this shared environment, a container's operating system must be compatible with the host's — for instance, a Linux host can run Linux containers but not Windows containers.

**Summary: Choosing the Right Approach**

Choosing between VMs and containers depends on your needs. If you require full isolation with a complete guest operating system, a VM is the way to go. But if you're looking for a lightweight solution that's quick to deploy and doesn't sap as many resources, containers could be your best bet. Understanding these differences is crucial for making informed decisions in your technology strategies.


## Docker’s Terminology

> **Container:**  An isolated unit that encapsulates an application along with its dependencies and runtime.

> **Image:**  A lightweight, standalone, and executable package that includes everything needed to run an application, including the code, runtime, libraries, and system tools.

> **Dockerfile:**  A script containing instructions for building a Docker image.

> **Registry:**  A centralized repository for storing and distributing Docker images.

> **Compose:** A tool for defining and running multi-container Docker applications using a YAML file.

> **Volume:** A persistent data storage mechanism in Docker that allows data to persist beyond the lifecycle of a container. etc.


## Docker's Architecture

Docker operates through a client-server architecture, a model that is key to its functionality and efficiency. Understanding this architecture is crucial for effective Docker usage.

**The Two Main Components: Client and Server**

1. **Docker Client:** This is the interface through which users interact with Docker. When you issue a command in the Docker command-line interface (CLI), the Docker client sends these commands to the Docker server, or Engine, which then executes them.

2. **Docker Server (Docker Engine):** This is the heart of Docker, running the heavy lifting of building, running, and managing Docker containers. The Docker Engine is akin to a hypervisor in traditional VM setups but tailored for container execution. It creates a layer over the host system, enabling containers to use the operating system kernel while remaining isolated from each other and the host.

**Setting Up and Verifying Your Installation**

See [Install Docker Engine](https://docs.docker.com/engine/install/) in Docker documentation. Once you've installed Docker, a good way to verify a successful installation is by running the `docker version` command. This command should output information about both the Docker client and the Docker server (Docker Engine). If information for only one of these components appears, or if the output is incomplete, this typically signals an issue with the installation that needs to be addressed.
After that, you can check if Docker is working with this command: `docker run hello-world`

**Why Docker Engine Matters**

The Docker Engine is essential for running Docker containers and represents the core of Docker's power and simplicity. Unlike traditional VM-based environments that require a full operating system for each virtual machine, Docker allows your applications to run in separate containers without the overhead of extra dependencies. All you need is Docker Engine installed on your machine, and you're set to run your applications in a portable, isolated environment. This simplifies development, improves deployment speed, and increases the scalability of applications.

## What is a Docker image and where can I find them?

To function, a container needs an image, a simple file containing code that Docker Engine understands in order to create a container. The image file contains the various commands to be executed and the files to be installed in order to run your application correctly.

It’s the equivalent of a snapshot for a VM: <u>a file that lets you recreate your entire VM.</u>

There are several ways to find Docker images. Firstly, you can find them in what are known as registries. [**Docker Hub**](https://hub.docker.com) is one such registry.

To get images from a registry, you just have to use  `docker pull`  . For example,  [here is a Firefox image](https://hub.docker.com/r/linuxserver/firefox)  allowing you to use Firefox in a container. Here is how to pull it:

`docker pull linuxserver/firefox`

Another way to find images is to <u>create them</u>.

## How to create a Docker image

This is done using what’s known as a **Dockerfile**, a file containing instructions for building a Docker image.

In a Dockerfile, various commands are employed to construct a seamless recipe for containerization. These commands, akin to building blocks, shape the entire process from start to finish. Here are some frequently used ones:

> **FROM:**  This sets the base image, providing a foundation for subsequent actions. It’s akin to selecting the canvas on which your masterpiece will unfold.

> **RUN:** This executes commands within the image, allowing you to install dependencies, run scripts, or perform any necessary tasks  **_during image creation_**. Think of it as the artist’s brushstroke, adding layers to your creation.

> **COPY/ADD:** These commands bring files into the image, whether it’s your application code, configurations, or other essential elements. It’s like placing ingredients onto the canvas, ensuring everything needed is within reach.

> **_WORKDIR:_**  This sets the working directory within the container, guiding subsequent commands to execute in a specific location. It’s akin to organizing your workspace, making sure the creative process flows smoothly.

> **EXPOSE:**  This command informs Docker that the container will listen on specific network ports during runtime. It’s like opening windows in your artwork, allowing the outside world to interact with your creation.

> **CMD/ENTRYPOINT:**  These commands specify the  **default command to run when the container starts.**  They define the heart of your creation, the core functionality that makes your container unique.

> **ENV:**  This sets environment variables within the container, allowing you to configure the environment for your application. It’s akin to adjusting the lighting and ambiance in your creative space.

Together, these commands, woven into the fabric of a Dockerfile, transform a series of instructions into a deployable and reproducible containerized masterpiece.

Let’s create a Docker image of a simple Python program. The advantage of Docker is that you don’t even need Python to create this image. Create a file hello.py that contains:

`print("Hello, World!")`

In the same directory, create a file called “Dockerfile” with the following content:

```
# FROM is used to specify the base image. In this case, we are using the official Python 3 image.
FROM python:3.11

# This command sets the working directory inside the container to /app,
# creating a designated space for subsequent commands (RUN, CMD, ENTRYPOINT, COPY and ADD).
WORKDIR /app

# This copies the contents of the current directory (where the Dockerfile is located) into the /app directory within the container.
COPY . /app

# This updates the pip package manager to the latest version within the container
RUN pip install --upgrade pip

# This command installs the Python dependencies listed in the requirements.txt file.
# It ensures that the required libraries for your application are installed within the container.
RUN pip install -r requirements.txt

# This specifies the default command to run when the container starts.
CMD ["python", "hello.py"]
```



The conventional format for tagging Docker images is `<registry>/<repository>:<tag>`.

Here’s a breakdown of how it works 👇:

-   **Registry:**  This is the server or location where Docker images are stored. Examples include Docker Hub, Google Container Registry, Amazon Elastic Container Registry (ECR), and others. If you specify a registry, Docker will attempt to push the image to that registry.

-   **Repository:**  This is the name of your Docker image. It’s similar to a project or application name. For example, if you’re building an image for your web application, the repository might be the name of that application.

-   **Tag:**  The tag is a version or label assigned to the image. It helps identify different versions of the same image. If a tag is not explicitly provided during the image build, Docker defaults to tagging it as the  **latest**.

From the directory containing the Dockerfile, execute the following command in your local terminal:

`docker build -t dumboai:v1 .`

Let’s understand this. The  `-t`  flag is used to specify a name and optionally a tag for the image being built. In this case, the image will be named  `dumboai`  with a tag  `v1`. The dot  `.`  at the end of the command represents the build context. It specifies that the build should use the current directory (where the Dockerfile is located) as the build context. The build context includes all files and directories in the specified path, and they can be referenced in the Dockerfile.

We’ve effortlessly created a Docker image. Now, let’s take a look at it. Simply run the `docker images` command in your terminal, or if you have Docker Desktop installed, open it to view the list of images.


## Running a container

Now that we’ve got our image, all we need to do is create a container from it. 

Running a Docker image involves using the  `docker run`  command to create and start a container based on the specified image. The command includes options such as  `-it`  for interactive mode,  `-p`  for port mapping, and the name or ID of the Docker image. If the image is not present locally, Docker automatically attempts to pull it from the default registry, typically Docker Hub.

The syntax for port mapping is  `-p <host_ip>:<host_port>`. The  `docker run`  command allows customization of container behavior, such as executing a specific command or mapping ports between the host and the container. Containers launched from the same image are isolated instances, each with its own filesystem and runtime environment. This approach enables consistent deployment across different environments and simplifies application management by encapsulating dependencies and configurations within containers.

Let’s run our image as following:

`docker run -p 0.0.0.0:1234:8501 dumbai:v1`

In the command `docker run -p 0.0.0.0:1234:8501 dumbai:v1`, it maps port **_1234_** on the host to port **8501** inside the container(which is the default streamlit port), with `**0.0.0.0**` allowing access from any IP address. This enables universal accessibility to the application running in the container from the specified host port. Let’s test it out opening the following address in the browser:

`http://localhost:1234`



## Persisting Data

An important concept to understand with these containers is that they are **ephemeral**. When you use  `docker run`, the container is created, executed and then disappears without a trace when the execution of its command is complete. Even if you use the same image to run the container. This is normal, as containers are designed to be isolated and leave no trace.

On the other hand, it is possible to provide containers with files and folders outside the isolation environment, enabling data to be persisted or files to be shared between the host machine and the container.

Update the previous **Dockerfile** to now have persistent data:

```
FROM python:3.11  
  
WORKDIR /app  
  
# Create a directory to store data inside the container  
RUN mkdir /app/data  
  
COPY ./hello.py /app  
  
CMD ["python", "hello.py"]
```

Let's recreate the Docker image:

`docker build -t hello-python .`

And then run our container the way we’ve done it previously:

`docker run hello-python`

To run our app again without creating another container, we have to find the id or name of our container and use  `docker start`. We use  `docker ps`  with the  `--all`  flag to list all our containers.

`docker ps --all`

We can just copy the id of the desired container and start it:

`docker start 064e0abc0dda`

This time, we can’t see the output of our appliocation. It’s because the output of  `docker start`  is not the output of our application. We have to use  `docker container logs` .

`docker container logs 064e0abc0dda`

Now, if we recreate the same container, you’ll see data is kept isolated into the containers because it will reset the counter, and not use the previous values:

`docker run hello-python`


To persist data, we can use **volume mappings**. They allow to map a volume on the host machine to a location in the container. Here is an example:

`docker run -v $(pwd)/data:/app/data hello-python`



## Section 4: Docker Push

Now that we’ve successfully built our Docker image, the next step is to push it to Docker Hub for broader accessibility. Pushing the image to Docker Hub involves tagging it appropriately with your Docker Hub username and repository name, followed by executing the  `docker push`  command. This action uploads your image to your Docker Hub account, allowing others to easily pull and use your containerized application. This seamless process ensures that your Docker image is accessible to the wider community, fostering collaboration and sharing within the Docker ecosystem. Here’s a step-by-step explanation:

1.  [**Create a Docker Hub Account**](https://hub.docker.com/)

2.  **Create a Repository on Docker Hub:** This repository will be the online space where your Docker image will be stored (After Push).

3.  **Tag Your Local Image:** In your local environment, tag your Docker image with the repository information on Docker Hub. This ensures a connection between your local image and the repository you’ve created. Run here  `docker tag dumbai:v1 dccsilva/dumbaihub:v01`. So, after running this command, the image previously named  `dumbai`with the tag  `v1`  is now tagged as  `dccsilva/dumbaihub:v01`.

4.  **Sign In to Docker Hub via CLI:** Before pushing your image, sign in to Docker Hub via the command line interface using`docker login`.

5.  **Push Your Image to Docker Hub:** Once authenticated, use the  `docker push`  command to upload your tagged image to Docker Hub.

`docker push dccsilva/dumbaihub:v01`


## Section 5: Pulling and Running the Dockerized App from Docker Hub

If the same tag is already present locally, the system attempts to run it from the local environment. However, if the local instance is not found, Docker automatically pulls the required image from the repository and initiates the run process. This ensures a streamlined experience, prioritizing local resources while seamlessly resorting to the repository if needed.

To confirm this, let's delete the local image, and then we’ll observe the outcome when attempting to run an image that is not present locally.

`docker images`

`docker image rm --force dccsilva/dumbaihub:v01`

`docker images`

`docker run -p 1234:8501 dccsilva/dumbaihub:v01`

Initially, it attempted to locate the image locally. However, encountering a failure, it promptly fetched the image from Docker Hub and executed it.
