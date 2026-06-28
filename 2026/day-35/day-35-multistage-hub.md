# Day 35 – Multi-Stage Docker Builds & Docker Hub: Reducing a 1.51 GB Image to Just 62 MB

One of the biggest mistakes developers make when learning Docker is assuming that if a container works, it's production-ready.

In reality, production containers should be **small, secure, fast to deploy, and easy to distribute**. Large images slow down CI/CD pipelines, consume more storage, increase network transfer time, and expose unnecessary dependencies.

On **Day 35** of my **#90DaysOfDevOps** journey, I learned how **Multi-Stage Docker Builds** solve these problems. Instead of using a basic "Hello World" application, I optimized my own **DevBoard React Frontend**, compared different base images, and published the final optimized image to Docker Hub.

---

# Why Multi-Stage Builds?

A typical frontend application needs Node.js only during the build process. Once the React application is compiled, Node.js is no longer required.

However, many beginners deploy the entire build environment to production.

That means shipping:

* Node.js
* npm
* Source code
* Development dependencies
* Build cache
* Production files

Production only needs the compiled application.

That's where Multi-Stage Builds help.

```text
React Source Code
        │
        ▼
 Builder Stage
(Node.js + npm)
        │
        ▼
 npm run build
        │
        ▼
 dist/
        │
        ▼
Production Stage
(Nginx Alpine)
```

The final image contains only the compiled application and a lightweight web server.

---

# Task 1 – Building a Single-Stage Image

For the first task, I created a traditional Dockerfile for my DevBoard frontend.

### Single Stage Dockerfile

```dockerfile
FROM node:26

WORKDIR /app

COPY package*.json .

RUN npm install

COPY . .

RUN npm run build

CMD ["npm","run","preview"]
```

### Build Command

```bash
docker build -t devboard-fe-full .
```

### Result

Image Size:

```
1.51 GB
```

Although the application worked perfectly, the image included:

* Node.js runtime
* npm
* package cache
* source code
* node_modules
* production build

Clearly, this is far from production-ready.

---

# Task 2 – Multi-Stage Build

Next, I separated the build process from the runtime environment.

## Stage 1 – Builder

The first stage installs dependencies and builds the React application.

```dockerfile
FROM node:26 AS builder

WORKDIR /app

COPY package*.json .

RUN npm install --legacy-peer-deps

COPY . .

RUN npm run build
```

---

## Stage 2 – Production

Instead of shipping Node.js, I copied only the compiled application into an Nginx container.

```dockerfile
FROM nginx:latest

COPY --from=builder /app/dist /usr/share/nginx/html

EXPOSE 80

CMD ["nginx","-g","daemon off;"]
```

### Build

```bash
docker build -t devboard-fe-optimized .
```

### Result

```
162 MB
```

The image size dropped dramatically simply by removing unnecessary development dependencies.

---

# Further Optimization Using Alpine

To optimize it even further, I replaced the standard Nginx image with the Alpine version.

```dockerfile
FROM nginx:alpine

COPY --from=builder /app/dist /usr/share/nginx/html

EXPOSE 80

CMD ["nginx","-g","daemon off;"]
```

### Final Image Size

```
62.5 MB
```

This is more than **95% smaller** than the original single-stage build.

---

# Image Size Comparison

| Build Type                        |  Image Size |
| --------------------------------- | ----------: |
| Single Stage (Node.js)            | **1.51 GB** |
| Multi-Stage (Node + Nginx)        |  **162 MB** |
| Multi-Stage (Node + Nginx Alpine) | **62.5 MB** |

The difference is huge.

A smaller image means:

* Faster downloads
* Faster deployments
* Lower bandwidth usage
* Better CI/CD performance
* Reduced attack surface

---

# Comparing Base Images

I also compared the size of different Docker base images.

| Base Image    | Disk Usage | Content Size |
| ------------- | ---------: | -----------: |
| Ubuntu Latest |     160 MB |      45.3 MB |
| Nginx Latest  |     241 MB |        66 MB |
| Nginx Alpine  |  **13 MB** |  **3.93 MB** |

This clearly explains why Alpine images are commonly used in production environments.

---

# Why Multi-Stage Builds Are Better

Instead of deploying everything, Multi-Stage Builds copy only the required production files.

The final image contains:

* Compiled HTML
* CSS
* JavaScript
* Lightweight Nginx Server

It excludes:

* Source code
* npm
* Node.js
* package cache
* Development dependencies

This makes the image smaller, faster, and more secure.

---

# Task 3 – Publishing to Docker Hub

Once the optimized image was ready, I uploaded it to Docker Hub.

### Login

```bash
docker login
```

### Tag the Image

```bash
docker tag devboard-fe-optimized-alpine \
tusharnangare31/devboard-fe-optimized-alpine:latest
```

### Push

```bash
docker push tusharnangare31/devboard-fe-optimized-alpine:latest
```

---

# Verifying the Image

To ensure the image was correctly published, I pulled it on another environment.

```bash
docker pull tusharnangare31/devboard-fe-optimized-alpine:latest
```

The image downloaded successfully, confirming the upload was successful.

---

# Task 4 – Exploring Docker Hub

After publishing the image, I explored Docker Hub and completed the following tasks:

* Created a public repository
* Added a repository description
* Verified the `latest` tag
* Explored repository tags
* Successfully pulled the published image

Repository:

```
tusharnangare31/devboard-fe-optimized-alpine
```

---

# Task 5 – Docker Image Best Practices

Today's session also covered several production best practices.

## Use Lightweight Base Images

Instead of:

```dockerfile
FROM ubuntu:latest
```

Prefer:

```dockerfile
FROM nginx:alpine
```

---

## Use Multi-Stage Builds

Separate the build environment from the runtime environment.

---

## Reduce Layers

Instead of:

```dockerfile
RUN apt update
RUN apt install curl
```

Use:

```dockerfile
RUN apt update && \
    apt install -y curl
```

Fewer layers produce cleaner images.

---

## Avoid `latest` in Production

Instead of:

```dockerfile
FROM nginx:latest
```

Use version-specific tags:

```dockerfile
FROM nginx:1.29-alpine
```

Version pinning ensures consistent deployments.

---

## Run Containers as a Non-Root User

Running containers as non-root users improves security and follows Docker best practices.

---

# What I Learned

Before today, I thought Docker optimization was only about reducing image size.

Now I understand it's much more than that.

A well-optimized Docker image improves:

* CI/CD pipeline speed
* Deployment time
* Infrastructure cost
* Security
* Scalability
* Developer productivity

The most satisfying part of today's challenge was seeing my DevBoard frontend shrink from **1.51 GB** to **62.5 MB** using Multi-Stage Builds and Nginx Alpine.

---

# Useful Docker Commands

Build Image

```bash
docker build -t image-name .
```

View Images

```bash
docker images
```

Login to Docker Hub

```bash
docker login
```

Tag an Image

```bash
docker tag local-image username/repository:tag
```

Push an Image

```bash
docker push username/repository:tag
```

Pull an Image

```bash
docker pull username/repository:tag
```

---

# Conclusion

Building Docker images is easy. Building **production-ready Docker images** requires optimization.

Multi-Stage Builds eliminate unnecessary files, lightweight base images reduce storage and bandwidth, and Docker Hub makes it easy to distribute applications across teams and environments.

Using my own DevBoard project instead of a simple demo application made this learning much more practical. Seeing the image size shrink from **1.51 GB** to **62.5 MB** clearly demonstrated why Multi-Stage Builds are considered a best practice in modern DevOps workflows.

Every MB saved reduces deployment time, improves efficiency, and makes applications easier to ship.

---

**Docker Hub Repository:**
**tusharnangare31/devboard-fe-optimized-alpine**

If you're learning Docker, don't stop after getting your application to run. Learn how to optimize it—that's what makes the difference between a development container and a production-ready one.

**#90DaysOfDevOps #Docker #DockerHub #MultiStageBuild #React #Nginx #Alpine #DevOps #CloudComputing #Containerization #TrainWithShubham #DevOpsKaJosh**
