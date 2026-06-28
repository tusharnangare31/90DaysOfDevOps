# Day 34 of #90DaysOfDevOps – Docker Compose for Real-World Multi-Container Applications

Docker Compose is one of the most important tools in a DevOps engineer's toolkit. While Docker allows us to containerize individual applications, real-world software consists of multiple services that must work together. A typical web application includes a frontend, backend API, database, cache, reverse proxy, and background workers. Managing each service manually becomes difficult as the application grows.

In this project, I explored **Docker Compose** by deploying Docker's official **Example Voting App**, a production-like multi-container application that demonstrates how multiple services communicate using Docker Compose.

Unlike simple "Hello World" examples, this project uses multiple containers, networking, persistent storage, service dependencies, restart policies, and scaling—concepts used in real production environments.

---

# What is Docker Compose?

Docker Compose is a tool that allows you to define and run multi-container Docker applications using a single YAML configuration file.

Instead of starting every container manually with long `docker run` commands, you describe your infrastructure inside a `docker-compose.yml` file.

Then deploy everything with one command:

```bash
docker compose up -d
```

Compose automatically:

* Creates required networks
* Creates persistent volumes
* Starts containers
* Connects services together
* Applies environment variables
* Manages dependencies

This approach follows the concept of **Infrastructure as Code (IaC)**.

---

# Application Used

For this project, I deployed Docker's official **Example Voting App**.

Architecture:

```text
                User
                  │
                  ▼
         Vote Frontend (Python)
                  │
                  ▼
              Redis Cache
                  │
                  ▼
            Worker Service
                  │
                  ▼
          PostgreSQL Database
                  │
                  ▼
        Result Frontend (.NET)
```

Every container has a dedicated responsibility.

---

# Services Used

## Vote

Python web application where users cast votes.

---

## Redis

Temporary message broker.

Stores votes before processing.

Benefits:

* Fast
* Lightweight
* Excellent for caching and queues

---

## Worker

Reads data from Redis and inserts it into PostgreSQL.

Acts as a background processing service.

---

## PostgreSQL

Stores voting results permanently.

Unlike Redis, data remains available after restarting containers.

---

## Result

Displays live voting results by reading data from PostgreSQL.

---

# Task 1 – Build Your Own App Stack

The objective was to understand how a real multi-container application works.

The Example Voting App consists of:

* Frontend
* Worker
* Redis
* PostgreSQL
* Result Service

Each service runs inside its own container and communicates over Docker Compose networking.

This architecture demonstrates the microservices pattern commonly used in production.

---

# Task 2 – depends_on & Healthchecks

A common problem is that the application may start before the database is ready.

Docker Compose solves this using:

```yaml
depends_on:
  db:
    condition: service_healthy
```

Healthcheck example:

```yaml
healthcheck:
  test: ["CMD","pg_isready","-U","postgres"]
  interval: 10s
  timeout: 5s
  retries: 5
```

## Why use Healthchecks?

Starting a container does not guarantee that the application inside it is ready.

Healthchecks verify the service is actually accepting requests.

Without healthchecks:

```text
App starts
↓

Database still initializing

↓

Connection failed
```

With healthchecks:

```text
Database starts

↓

Healthcheck passes

↓

Application starts
```

This creates a much more reliable deployment.

---

# Task 3 – Restart Policies

Restart policies define how Docker should behave when a container exits unexpectedly.

Example:

```yaml
restart: always
```

## Types

### always

Container restarts regardless of why it stopped.

Used for:

* Databases
* APIs
* Production services

---

### on-failure

Container restarts only when it exits with an error.

Useful for:

* Batch jobs
* Scripts
* Worker processes

---

### unless-stopped

Keeps restarting unless manually stopped.

Ideal for development environments.

---

### no

No automatic restart.

Suitable for testing.

---

# Task 4 – Build Images Using Dockerfiles

Instead of pulling pre-built images, Compose can build images directly.

Example:

```yaml
services:
  vote:
    build: ./vote
```

Benefits:

* Build custom applications
* Automatically rebuild after code changes
* Easy deployment

Rebuild:

```bash
docker compose up --build
```

---

# Task 5 – Named Networks & Volumes

Compose automatically creates a default network, but production applications often use explicit networks.

Example:

```yaml
networks:
  frontend:
  backend:
```

Benefits:

* Better isolation
* Improved security
* Easier troubleshooting

---

## Named Volumes

Example:

```yaml
volumes:
  postgres-data:
```

Used in PostgreSQL:

```yaml
volumes:
  - postgres-data:/var/lib/postgresql/data
```

Advantages:

* Persistent storage
* Data survives container recreation
* Suitable for databases

---

## Labels

Labels help organize services.

Example:

```yaml
labels:
  project: voting-app
  environment: development
```

Useful for monitoring and automation.

---

# Task 6 – Scaling Services

Docker Compose allows scaling using:

```bash
docker compose up --scale vote=3
```

Compose launches three containers:

```text
vote_1
vote_2
vote_3
```

## What Happens?

Requests are distributed among containers.

However, simple scaling has limitations.

### Why doesn't port mapping work?

Each container cannot bind the same host port.

Example:

```text
vote_1 → 5000

vote_2 → 5000 ❌

vote_3 → 5000 ❌
```

Only one container can own a host port.

Production environments solve this using:

* Nginx
* Traefik
* HAProxy
* Kubernetes Services

These act as load balancers.

---

# Useful Docker Compose Commands

Start application

```bash
docker compose up
```

Detached mode

```bash
docker compose up -d
```

Stop services

```bash
docker compose stop
```

Remove everything

```bash
docker compose down
```

Rebuild images

```bash
docker compose up --build
```

View containers

```bash
docker compose ps
```

View logs

```bash
docker compose logs
```

Logs for one service

```bash
docker compose logs vote
```

Follow logs

```bash
docker compose logs -f
```

Scale services

```bash
docker compose up --scale vote=3
```

---

# Interview Questions

## What is Docker Compose?

Docker Compose is a tool used to define and manage multi-container Docker applications using a YAML configuration file.

---

## Why use Compose?

* Simplifies deployments
* Infrastructure as Code
* Automatic networking
* Persistent volumes
* Easy environment management

---

## What is depends_on?

Controls service startup order.

---

## Why use Healthchecks?

Ensures a service is actually ready before dependent services start.

---

## Difference between restart policies?

| Policy         | Purpose                        |
| -------------- | ------------------------------ |
| always         | Always restart                 |
| on-failure     | Restart only after failure     |
| unless-stopped | Restart until manually stopped |
| no             | Never restart                  |

---

## Why use Named Volumes?

To persist application data independently of containers.

---

## Why use Networks?

Allows secure communication between services using service names instead of IP addresses.

---

# Real-World Applications

Docker Compose is widely used for:

* WordPress + MySQL
* Django + PostgreSQL
* MERN Stack
* React + Node.js + Redis
* Flask + PostgreSQL
* ELK Stack
* Monitoring (Prometheus + Grafana)

It provides a simple way to replicate production-like environments for development and testing.

---

# Key Takeaways

* Docker Compose simplifies multi-container deployments.
* Service definitions are stored in a single YAML file.
* Networks are created automatically.
* Volumes ensure persistent storage.
* Healthchecks improve reliability.
* Restart policies enhance fault tolerance.
* Custom Dockerfiles enable application-specific builds.
* Scaling demonstrates the limitations of host port mapping and introduces the need for load balancing.

---

# Conclusion

Docker Compose bridges the gap between running individual containers and managing complete application stacks. Through the Example Voting App, I learned how multiple services communicate, how persistent storage works, how healthchecks and restart policies improve reliability, and why Compose is such an important step before learning Kubernetes.

Understanding Docker Compose makes it much easier to build, test, and deploy modern applications. The concepts of service discovery, networking, volumes, and declarative configuration carry directly into container orchestration platforms, making Compose an essential skill for every DevOps engineer.
