# Day 30 – Docker Images & Container Lifecycle

## Introduction

Today I explored one of the most important Docker concepts: **Docker Images and Container Lifecycle Management**.

Containers are created from images, and understanding how images are built, stored, and executed is fundamental for every DevOps Engineer. In this hands-on session, I worked with Docker Hub images, inspected image layers, managed container states, explored running containers, and cleaned up Docker resources.

---

# Understanding Docker Images

A Docker image is a lightweight, portable, and executable package that contains everything required to run an application:

* Application code
* Runtime
* Libraries
* Dependencies
* Configuration files

Images are read-only templates used to create containers.

### Pulling Images from Docker Hub

I downloaded three popular images:

```bash
docker pull nginx
docker pull ubuntu
docker pull alpine
```

### Viewing Downloaded Images

```bash
docker images
```

Output showed:

| Image  | Approx Size |
| ------ | ----------- |
| alpine | 13 MB       |
| ubuntu | 160 MB      |
| nginx  | 241 MB      |

---

# Ubuntu vs Alpine

One interesting observation was the huge size difference between Ubuntu and Alpine.

### Alpine Linux

* Minimal Linux distribution
* Built specifically for containers
* Very small footprint
* Faster downloads
* Smaller attack surface

### Ubuntu

* Full-featured Linux distribution
* More packages and utilities included
* Easier for beginners
* Larger image size

This explains why Alpine is commonly used in production containers.

---

# Docker Image Layers

To understand image construction, I inspected the Nginx image history.

```bash
docker image history nginx
```

The output displayed multiple layers such as:

```text
CMD
EXPOSE
COPY
RUN
ENV
LABEL
```

Some layers consumed storage while others showed:

```text
0B
```

### Why Some Layers Show 0B

Instructions like:

```dockerfile
ENV
CMD
LABEL
EXPOSE
```

only modify metadata and do not change the filesystem.

Therefore Docker stores them as metadata-only layers.

---

# What Are Docker Layers?

Docker images are built using layered filesystems.

Example:

```text
Base OS Layer
      ↓
Package Installation Layer
      ↓
Application Layer
      ↓
Configuration Layer
```

Benefits:

* Faster builds
* Layer reuse
* Reduced storage usage
* Efficient downloads
* Better caching

When an image is updated, Docker downloads only changed layers instead of the entire image.

---

# Container Lifecycle

A container goes through several states during its lifetime.

```text
Create
   ↓
Start
   ↓
Running
   ↓
Pause
   ↓
Unpause
   ↓
Stop
   ↓
Restart
   ↓
Kill
   ↓
Remove
```

I practiced each state manually.

---

## Creating a Container

Create a container without starting it:

```bash
docker create -p 80:80 nginx
```

Status:

```text
Created
```

The container exists but is not running.

---

## Starting a Container

```bash
docker start <container-id>
```

Status:

```text
Up
```

The container starts executing.

---

## Checking Running Containers

```bash
docker ps
```

Shows only active containers.

---

## Viewing All Containers

```bash
docker ps -a
```

Shows both running and stopped containers.

---

## Pausing a Container

```bash
docker pause <container-id>
```

Status:

```text
Paused
```

The process remains in memory but execution is temporarily suspended.

---

## Unpausing a Container

```bash
docker unpause <container-id>
```

Status:

```text
Running
```

Container resumes execution.

---

## Stopping a Container

```bash
docker stop <container-id>
```

Status:

```text
Exited
```

Docker gracefully terminates the container.

---

## Restarting a Container

```bash
docker restart <container-id>
```

Docker automatically stops and starts the container again.

---

## Killing a Container

```bash
docker kill <container-id>
```

Status:

```text
Exited (137)
```

Unlike stop, kill immediately terminates the process without graceful shutdown.

---

# Running Containers in Detached Mode

Detached mode allows containers to run in the background.

```bash
docker run -d -p 80:80 nginx
```

Options used:

| Flag | Meaning       |
| ---- | ------------- |
| -d   | Detached mode |
| -p   | Port mapping  |

This mapped:

```text
Host Port 80 → Container Port 80
```

and allowed browser access to Nginx.

---

# Viewing Container Logs

To inspect runtime activity:

```bash
docker logs <container-id>
```

Logs showed:

* Nginx startup sequence
* Worker processes
* HTTP requests
* Access logs

---

# Following Logs in Real Time

```bash
docker logs -f <container-id>
```

The `-f` option continuously streams logs.

Useful for:

* Monitoring applications
* Troubleshooting
* Production debugging

---

# Accessing a Running Container

Interactive shell access:

```bash
docker exec -it <container-id> bash
```

Inside the container I explored:

```bash
ls
cat /etc/os-release
```

The Nginx image was based on:

```text
Debian GNU/Linux 13 (Trixie)
```

This demonstrated that containers are isolated environments running their own filesystem.

---

# Running Single Commands Inside Containers

Without entering interactive mode:

```bash
docker exec <container-id> ls
```

Useful for automation and scripting.

---

# Inspecting Container Details

```bash
docker inspect <container-id>
```

This command provides extensive information including:

* Container ID
* Network configuration
* Port mappings
* Environment variables
* Mounts
* Runtime status
* IP address

One of the most useful troubleshooting commands in Docker.

---

# Docker Cleanup

Containers and images consume disk space.

Cleaning unused resources is important.

---

## Remove Stopped Containers

```bash
docker rm $(docker ps -aq)
```

Deletes all stopped containers.

---

## Remove Unused Images

```bash
docker image prune -a
```

Docker reclaimed approximately:

```text
115.2 MB
```

of disk space.

---

## Verify Remaining Images

```bash
docker images
```

No images remained after cleanup.

---

## Check Docker Storage Usage

```bash
docker system df
```

Output displayed:

```text
Images      0
Containers  0
Volumes     0
Build Cache 0
```

All Docker resources were successfully removed.

---

# Commands Learned Today

```bash
docker pull nginx
docker pull ubuntu
docker pull alpine

docker images

docker image history nginx

docker create

docker start

docker ps

docker ps -a

docker pause

docker unpause

docker stop

docker restart

docker kill

docker run -d

docker logs

docker logs -f

docker exec -it

docker inspect

docker rm

docker image prune -a

docker system df
```

---

# Key Takeaways

* Docker Images are templates used to create containers.
* Alpine is significantly smaller than Ubuntu because it contains fewer packages.
* Docker Images are built using reusable layers.
* Containers move through different lifecycle states.
* Logs and inspect commands are essential for troubleshooting.
* Detached containers run in the background.
* Docker cleanup commands help reclaim storage efficiently.

---

# Conclusion

Day 30 provided a deeper understanding of Docker internals by exploring image layers, container states, logging, inspection, and cleanup operations. These concepts form the foundation for building custom Docker images, writing Dockerfiles, and eventually working with container orchestration platforms like Kubernetes.

This hands-on practice strengthened my understanding of how Docker manages applications and prepared me for the next stage of containerization in my DevOps journey.
