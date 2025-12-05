<!---
{
  "id": "24b25804-5bb5-443f-a78a-1bd485bebed8",
  "teaches": "Introduction to Docker",
  "depends_on": ["fcef696e-079c-4d83-b611-7b378bb8ac07","b9e34253-61e2-4fb7-bd36-388c370fa765"],
  "author": "Stephan Bökelmann",
  "first_used": "2025-05-13",
  "keywords": ["docker", "containerization", "linux", "development"]
}
--->

# Introduction to Docker

> In this exercise you will learn how to run and manage basic Docker containers. Furthermore we will explore how Docker images are built, saved, transferred, and executed across different systems.

### Introduction

Docker is a powerful containerization platform that enables developers to package applications and their dependencies into isolated environments called containers. Unlike virtual machines that replicate full operating systems, containers share the host's kernel, making them more efficient and lightweight.

The key benefit of Docker lies in consistency: a container will run the same regardless of the underlying host, so long as the architecture matches. Docker images are blueprints that define how containers are instantiated — they are immutable and stored in registries, such as Docker Hub. The Docker Engine is responsible for managing the lifecycle of containers.

This exercise guides you through the basics of pulling, building, exporting, and transferring Docker images, focusing on clarity and minimalism by using the small `alpine` base image. You will also explore why Docker images are portable and when they need to be rebuilt.

### Further Readings and Other Sources

* [Docker Documentation](https://docs.docker.com/get-started/)
* ["What is Docker?" by Red Hat](https://www.redhat.com/en/topics/containers/what-is-docker)
* [Dockerfile reference](https://docs.docker.com/engine/reference/builder/)

## Tasks

### 1. Installing Docker on Linux

Before starting with Docker, you need to install the Docker Engine on your system.

#### On Linux (Debian/Ubuntu):

```sh
$ sudo apt update
$ sudo apt install apt-transport-https ca-certificates curl software-properties-common
$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
$ echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
$ sudo apt update
$ sudo apt install docker-ce
```

#### After installation, verify Docker is running:

```sh
$ docker --version
```

You may need to log out and back in or run `sudo` to execute Docker commands if permission is denied.

### 2. Hello World in Docker

Run a simple test container to verify that Docker is working correctly:

```sh
$ docker run hello-world
```

**Step-by-step explanation:**

* `docker run` attempts to find the `hello-world` image locally.
* If it's not present, Docker automatically pulls it from Docker Hub.
* It then creates a container from the image and executes its default command.
* The container prints a success message and then exits.

Check the output for confirmation that Docker is properly installed.

### 3. Clean Up

Remove unused containers and images to keep your system clean:

```sh
$ docker ps -a
$ docker rm <container_id>
$ docker rmi hello-world
```

**Explanation:**

* `docker ps -a` lists all containers, including those that have exited.
* `docker rm` removes a specific container by ID.
* `docker rmi` deletes the image from your local system.
* Use `docker images` to ensure the image has been removed.

### 4. Build a Minimal Hello World Image with Alpine

Create a lightweight image that prints a message using Alpine Linux:

**Dockerfile:**

```Dockerfile
FROM alpine:latest
CMD ["echo", "Hello from a minimal Docker image!"]
```

Build and execute it:

```sh
$ docker build -t alpine-hello .
$ docker run alpine-hello
```

**Explanation:**

* `FROM alpine:latest` sets the base image to Alpine Linux.
* `CMD` defines the command to run when the container starts.
* `docker build` creates the image using the Dockerfile in the current directory.
* `docker run` executes the container, which prints the message.

### 5. Export and Run Image on Another Machine

Save the image to a `.tar` archive and move it to another system:

```sh
$ docker save alpine-hello -o alpine-hello.tar
```

Transfer it using any method, e.g. `scp`:

```sh
$ scp alpine-hello.tar user@remote:/tmp/
```

On the target machine:

```sh
$ docker load -i /tmp/alpine-hello.tar
$ docker run alpine-hello
```

**Explanation:**

* `docker save` exports the image into a file.
* The file is copied to another system.
* `docker load` imports the image on the new system.
* You can now run the container without rebuilding.

Docker images are portable because they encapsulate all required layers and metadata. As long as the target machine uses the same CPU architecture (e.g., x86\_64), the container will run identically. Rebuilding is only necessary if the destination architecture differs, such as ARM vs x86. Multi-architecture support can be handled using Docker Buildx.

## Questions

1. What is the difference between a Docker image and a container?
2. How does Docker differ from traditional virtual machines?
3. What happens internally when you run `docker run hello-world`?
4. Why can the same Docker image run on different machines without rebuilding?
5. When would you need to rebuild a Docker image for another system?

## Advice

The best way to understand Docker is by experimenting. Try modifying the Dockerfile, adding files, or changing commands to see how images behave. Always clean up your environment to avoid unnecessary clutter. As you grow more confident, explore how to use Docker Compose or orchestrate containers with Kubernetes. For deeper insights, check out related sheets like [Docker Networking](#) and [Multi-Stage Builds](#). Docker is a cornerstone of modern DevOps — learning it well opens many opportunities.
