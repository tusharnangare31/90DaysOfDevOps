# Dockerizing a Full-Stack Hotel Booking Application with Docker & Docker Compose

Building containers is one thing. Deploying a complete production-ready application is another.

For **Day 36 of the #90DaysOfDevOps challenge**, I decided to Dockerize one of my own projects instead of following another tutorial.

The project I chose was **QuickStay**, a full-stack hotel booking application consisting of:

* React Frontend
* Node.js + Express Backend
* MongoDB Database
* Clerk Authentication
* Cloudinary Image Storage

The objective was simple:

> Convert the complete application into production-ready Docker containers, orchestrate everything with Docker Compose, optimize the images, and publish them on Docker Hub.

---

# Why I Chose My Own Project

Most Docker tutorials containerize a simple "Hello World" application.

Real DevOps work is very different.

Applications usually contain:

* Frontend
* Backend
* Database
* Environment Variables
* External Services
* Networks
* Persistent Storage

I wanted to experience those challenges instead of containerizing another demo project.

---

# QuickStay Architecture

The application consists of three major components.

```
                User
                  │
                  ▼
         React Frontend
          (Nginx Container)
                  │
        REST API Requests
                  │
                  ▼
      Node.js Express Backend
                  │
        ┌─────────┴─────────┐
        ▼                   ▼
    MongoDB            Cloudinary
 Database Storage      Image Storage

        │
        ▼
 Clerk Authentication
```

Each component runs independently while communicating over Docker networking.

---

# Technologies Used

## Frontend

* React
* Vite
* Tailwind CSS
* Axios
* Clerk Authentication

---

## Backend

* Node.js
* Express.js
* MongoDB
* Mongoose
* Cloudinary

---

## DevOps

* Docker
* Docker Compose
* Docker Hub
* Multi-stage Builds
* Docker Networks
* Docker Volumes

---

# Project Structure

```
QuickStay/

frontend/
 ├── Dockerfile
 ├── .dockerignore
 ├── nginx.conf
 └── React App

backend/
 ├── Dockerfile
 ├── .dockerignore
 └── Express API

docker-compose.yml

.env

README.md
```

Everything required to deploy the application is stored inside the repository.

---

# Dockerizing the Frontend

The frontend was built using **React + Vite**.

Instead of serving the application using Node.js in production, I created a **multi-stage Dockerfile**.

## Stage 1

Uses Node.js to:

* Install dependencies
* Build the React application

```
npm install

npm run build
```

---

## Stage 2

Uses **Nginx Alpine**

Only the generated build files are copied.

Advantages:

* Smaller image
* Faster startup
* Better security
* Production ready

---

# Dockerizing the Backend

The backend runs an Express API.

The Dockerfile performs:

* Copy source code
* Install dependencies
* Expose port 5000
* Start the application

The backend reads all configuration using environment variables instead of hardcoding secrets.

---

# Managing Environment Variables

Both frontend and backend require configuration.

Frontend

```
VITE_CLERK_PUBLISHABLE_KEY

VITE_API_URL

VITE_CURRENCY
```

Backend

```
MONGODB_URL

CLERK_SECRET_KEY

CLOUDINARY_KEYS

JWT_SECRET
```

Instead of placing secrets inside the Dockerfile, everything is loaded through an `.env` file.

This keeps images reusable across different environments.

---

# Docker Compose

Running multiple containers manually becomes difficult very quickly.

Docker Compose solved that problem.

One file defines:

* Frontend
* Backend
* MongoDB
* Network
* Volumes
* Environment Variables

Starting the complete application became as simple as:

```bash
docker compose up -d
```

Stopping everything:

```bash
docker compose down
```

---

# Custom Network

A custom bridge network was created.

```
quickstay-nw
```

Every service joins this network automatically.

Instead of using IP addresses, services communicate using container names.

Example:

```
backend

mongodb
```

Docker's internal DNS resolves these names automatically.

---

# Persistent Storage

MongoDB stores booking information.

Without persistent storage, all data disappears whenever the container is removed.

To solve this, a named volume was created.

```
mongo-data
```

The database now survives container restarts.

---

# Running Containers Manually

Although Docker Compose is recommended, every container can also run individually.

## Create Network

```bash
docker network create quickstay-nw
```

---

## MongoDB

```bash
docker run -d \
--name mongodb \
--network quickstay-nw \
-v mongo-data:/data/db \
-p 27017:27017 \
mongo:8
```

---

## Backend

```bash
docker run -d \
--name hotel-be \
--network quickstay-nw \
--env-file .env \
-p 5000:5000 \
hotel-be
```

---

## Frontend

```bash
docker run -d \
--name hotel-fe \
--network quickstay-nw \
--env-file .env \
-p 80:80 \
hotel-fe
```

Running containers manually helped me understand networking, environment variables, and service dependencies much better.

---

# Why Docker Compose?

Using only `docker run` works.

But Docker Compose provides:

* Infrastructure as Code
* Repeatable deployments
* Easier collaboration
* Automatic networking
* Automatic volume creation
* Cleaner configuration
* Single command deployment

This is why Compose is widely used in development environments.

---

# Docker Images

Two production images were created.

## Frontend Image

* React Build
* Nginx Alpine
* Multi-stage Build

---

## Backend Image

* Express Server
* Node.js
* Optimized Production Image

Both images were successfully tested before deployment.

---

# Publishing to Docker Hub

After verifying everything locally, both images were pushed to Docker Hub.

Anyone can now deploy the application without rebuilding.

Example:

```bash
docker pull yourusername/hotel-fe

docker pull yourusername/hotel-be
```

This makes the application portable across different systems.

---

# Challenges Faced

## Environment Variables

Managing frontend and backend variables separately required careful configuration.

Solution:

Separate `.env` files and Docker Compose variable mapping.

---

## Frontend Routing

Refreshing React routes returned 404 errors.

Solution:

Configured Nginx properly for SPA routing.

---

## Container Networking

Initially the backend couldn't communicate with MongoDB.

Solution:

Used a custom Docker network and connected services using container names.

---

## MongoDB Persistence

Database was resetting after container recreation.

Solution:

Added a named Docker volume.

---

## Docker Image Optimization

Initial frontend images were unnecessarily large.

Solution:

Used:

* Multi-stage Build
* Nginx Alpine
* Removed build dependencies from runtime image

The final production image became significantly smaller.

---

# Production Improvements Applied

✔ Multi-stage Dockerfile

✔ Nginx Alpine

✔ Environment Variables

✔ Docker Compose

✔ Named Volumes

✔ Custom Network

✔ Docker Hub

✔ Separate Frontend & Backend Images

✔ Optimized Build

✔ Production Ready Deployment

---

# Commands Used Frequently

Build images

```bash
docker build -t hotel-fe .

docker build -t hotel-be .
```

Run manually

```bash
docker run
```

Compose

```bash
docker compose up -d

docker compose down

docker compose logs

docker compose ps
```

Push to Docker Hub

```bash
docker login

docker tag hotel-fe username/hotel-fe

docker push username/hotel-fe
```

---

# What I Learned

Docker is much more than packaging an application.

A real deployment requires understanding:

* Dockerfiles
* Multi-stage Builds
* Image Optimization
* Networks
* Volumes
* Environment Variables
* Container Communication
* Docker Compose
* Image Distribution
* Production Deployment

This project gave me practical experience with the complete lifecycle of containerizing a full-stack application—from source code to a production-ready deployment.

---

# Final Thoughts

Dockerizing QuickStay was the closest experience I've had to a real DevOps workflow.

Instead of focusing on individual commands, I learned how multiple services work together as a single application.

From writing optimized Dockerfiles to orchestrating services with Docker Compose, managing environment variables, networking containers, persisting databases, and publishing images to Docker Hub, this project tied together many of the Docker concepts I had been learning over the past few weeks.

This wasn't just about making an application run inside containers—it was about understanding how modern applications are packaged, deployed, and maintained consistently across different environments.

The next step in this journey is taking these same containers and deploying them on Kubernetes, where orchestration, scaling, rolling updates, and production-grade deployments come into the picture.
