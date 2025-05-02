# Docker Files and Basic Commands Guide

Docker is a platform used to develop, ship, and run applications in lightweight, portable containers. This guide covers the basics of using Docker on Linux, focusing on how to build, run, stop containers, check container status, and handle Dockerfiles.

---

## General Overview

Docker allows developers to package applications and dependencies into containers that can run consistently across different environments. Docker containers are isolated from the host system and from each other, making them ideal for deploying applications in cloud environments, development environments, and production environments.

---

## Docker Basics

### Docker Image
A **Docker image** is a read-only template that contains the application and its dependencies. Images are used to create Docker containers.

### Docker Container
A **Docker container** is a runtime instance of a Docker image. It is a lightweight, standalone, and executable package that includes everything needed to run the application.

---

## Docker Commands

### Starting Docker
To start Docker on your Linux machine, use the following command:
```
sudo systemctl start docker
```

This command ensures that Docker is up and running, allowing you to build and manage containers.

### Check if Docker is Running
To verify that Docker is running, use:
```
sudo systemctl status docker
```

You should see output indicating that Docker is active and running.

---

## Building a Docker Image

### Dockerfile
A **Dockerfile** is a text file that contains a series of instructions on how to build a Docker image. A Dockerfile typically contains commands such as `FROM`, `COPY`, `RUN`, and `CMD` to specify the image's configuration and behavior.

### Building a Docker Image from a Dockerfile
To build a Docker image from a Dockerfile, navigate to the directory containing the Dockerfile and use the following command:
```
docker build -t <image_name>:<tag> .
```

- `-t <image_name>:<tag>`: Tags the image with the specified name and tag (e.g., `myapp:v1`).
- `.`: Specifies the current directory (where the Dockerfile is located).

Example:
```
docker build -t myapp:v1 .
```

This command will build an image named `myapp` with the tag `v1` from the Dockerfile in the current directory.

---

## Running a Docker Container

To run a container from an image, use the following command:
```
docker run -d --name <container_name> <image_name>:<tag>
```

- `-d`: Runs the container in detached mode (in the background).
- `--name <container_name>`: Assigns a name to the container.
- `<image_name>:<tag>`: Specifies the image and tag you want to use.

Example:
```
docker run -d --name mycontainer myapp:v1
```

This will start a container named `mycontainer` from the `myapp:v1` image.

### Exposing Ports
To map ports between your host machine and the container, use the `-p` flag:
```
docker run -d -p <host_port>:<container_port> --name <container_name> <image_name>:<tag>
```

Example:
```
docker run -d -p 8080:80 --name mycontainer myapp:v1
```

This will map port 80 inside the container to port 8080 on the host machine.

---

## Checking Running Containers

To list all the running containers on your system, use:
```
docker ps
```

This will show the container ID, names, image used, status, and other details about each running container.

To see all containers (including stopped ones), use:
```
docker ps -a
```

---

## Stopping a Docker Container

To stop a running container, use the following command:
```
docker stop <container_name_or_id>
```

Example:
```
docker stop mycontainer
```

This command will stop the `mycontainer` container.

If you want to remove a container after stopping it, use:
```
docker rm <container_name_or_id>
```

Example:
```
docker rm mycontainer
```

---

## Removing Docker Images

To remove an image that is no longer needed, use:
```
docker rmi <image_name>:<tag>
```

Example:
```
docker rmi myapp:v1
```

This will remove the `myapp:v1` image from your local system.

---

## Summary of Basic Docker Commands

### Start Docker Daemon
```
sudo systemctl start docker
```

### Build a Docker Image from Dockerfile
```
docker build -t <image_name>:<tag> .
```

### Run a Docker Container
```
docker run -d --name <container_name> <image_name>:<tag>
```

### List Running Containers
```
docker ps
```

### Stop a Running Container
```
docker stop <container_name_or_id>
```

### Remove a Docker Container
```
docker rm <container_name_or_id>
```

### Remove a Docker Image
```
docker rmi <image_name>:<tag>
```

---

## Conclusion

Docker is a powerful tool that helps you easily create, deploy, and run applications in containers. With the commands provided in this guide, you can start using Docker to build and manage your containers in Linux. Whether you’re a developer or a system administrator, Docker simplifies application deployment and environment management.
