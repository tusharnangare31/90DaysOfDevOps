# Docker Compose Explained: Run Multi-Container Applications with One Command | Day 33 of #90DaysOfDevOps

Building applications with Docker is straightforward when you're working with a single container. But real-world applications rarely consist of just one service. A typical web application includes a frontend, backend API, database, cache, and sometimes monitoring tools.

Managing all of these containers individually with `docker run` commands quickly becomes difficult.

This is exactly the problem Docker Compose solves.

In this blog, I'll explain how Docker Compose works, deploy an Nginx server, build a complete WordPress + MySQL application, configure environment variables, manage persistent storage, and learn the essential Compose commands every DevOps engineer should know.

---

# What is Docker Compose?

Docker Compose is a tool that lets you define and manage multiple Docker containers using a single YAML configuration file.

Instead of remembering long Docker commands, you describe your application's infrastructure in a file called:

```text
docker-compose.yml
```

Then start everything with a single command:

```bash
docker compose up -d
```

Compose automatically:

* Pulls required images
* Creates containers
* Creates networks
* Creates volumes
* Connects containers together
* Starts services in the correct order

Everything becomes Infrastructure as Code.

---

# Why Use Docker Compose?

Imagine deploying WordPress manually.

You would need to:

* Create a network
* Create a volume
* Start MySQL
* Configure database credentials
* Start WordPress
* Connect WordPress to MySQL
* Expose ports

Docker Compose automates all of these steps.

This makes development faster, deployments consistent, and collaboration much easier.

---

# Prerequisites

Before getting started, verify Docker Compose is installed.

```bash
docker compose version
```

Example output:

```text
Docker Compose version v2.40.3
```

---

# Project 1 — Running Nginx with Docker Compose

Create a new project.

```bash
mkdir compose-basics
cd compose-basics
```

Create a file named:

```text
docker-compose.yml
```

Add the following configuration:

```yaml
services:
  frontend:
    image: nginx:latest
    ports:
      - "80:80"
```

Start the application.

```bash
docker compose up
```

Or run it in the background.

```bash
docker compose up -d
```

Visit:

```text
http://localhost
```

or

```text
http://YOUR_SERVER_IP
```

The Nginx welcome page confirms the deployment was successful.

To stop everything:

```bash
docker compose down
```

---

# Understanding What Compose Created

Running a single command created:

* One container
* One default network
* Port mapping
* Service configuration

No manual networking or container management was required.

---

# Project 2 — Deploying WordPress with MySQL

Real applications require multiple services.

For this project I deployed:

* WordPress
* MySQL Database
* Docker Volume
* Automatic Network

Architecture:

```
                Browser
                   │
                   ▼
          WordPress Container
                   │
      Docker Compose Network
                   │
                   ▼
          MySQL Database Container
                   │
             Named Docker Volume
```

---

# docker-compose.yml

```yaml
services:
  db:
    image: mysql:8.0
    container_name: wordpress_db
    restart: always
    env_file: ".env"

    volumes:
      - db_data:/var/lib/mysql

  wordpress:
    image: wordpress:latest
    container_name: wordpress_app
    restart: always

    ports:
      - "8080:80"

    env_file: ".env"

    depends_on:
      - db

volumes:
  db_data:
```

---

# Using Environment Variables

Rather than placing passwords directly inside the Compose file, I created a separate `.env` file.

```text
MYSQL_ROOT_PASSWORD=root@123
MYSQL_DATABASE=wordpress_db
MYSQL_USER=admin
MYSQL_PASSWORD=admin@123

WORDPRESS_DB_HOST=db:3306
WORDPRESS_DB_NAME=wordpress_db
WORDPRESS_DB_USER=admin
WORDPRESS_DB_PASSWORD=admin@123
```

This approach keeps sensitive configuration separate from application configuration and makes the Compose file much cleaner.

---

# Starting the Application

Launch the complete stack.

```bash
docker compose up -d
```

Check running containers.

```bash
docker ps
```

Output:

```
wordpress_app
wordpress_db
```

Both containers start together.

---

# Access WordPress

Open your browser.

```
http://SERVER-IP:8080
```

The WordPress installation page appears.

Complete the setup wizard.

After installation you'll reach the WordPress Dashboard where you can begin creating content immediately.

---

# Automatic Networking

One of the biggest advantages of Docker Compose is networking.

I never created a network manually.

Compose automatically generated a bridge network for the project.

Both containers joined the same network.

This means WordPress communicates with MySQL using the service name:

```
db
```

instead of an IP address.

Docker automatically provides DNS resolution for every service defined in the Compose file.

---

# Persistent Storage with Volumes

The database container uses a named volume.

```yaml
volumes:
  - db_data:/var/lib/mysql
```

All MySQL data is stored inside this volume.

Even if the containers are stopped or recreated, the database remains intact.

This ensures that WordPress posts, users, and settings are not lost.

---

# Frequently Used Docker Compose Commands

Start services

```bash
docker compose up
```

Run in background

```bash
docker compose up -d
```

Stop containers

```bash
docker compose stop
```

Remove containers and network

```bash
docker compose down
```

View running services

```bash
docker compose ps
```

View logs

```bash
docker compose logs
```

Follow logs

```bash
docker compose logs -f
```

Logs for one service

```bash
docker compose logs wordpress
```

Restart services

```bash
docker compose restart
```

Rebuild after changes

```bash
docker compose up --build
```

---

# What I Learned

Throughout this exercise I understood that:

* Docker Compose manages an entire application using one YAML file.
* Services automatically communicate using their service names.
* Docker Compose creates networks automatically.
* Named volumes persist data even after containers are removed.
* Environment variables keep sensitive information separate from configuration.
* One command can deploy an entire application stack.

---

# Common Issues I Encountered

### Compose file not found

```
no configuration file provided
```

Solution:

Run the command inside the project directory or specify the file.

```bash
docker compose -f docker-compose.yml up
```

---

### Version Warning

```
the attribute 'version' is obsolete
```

Docker Compose V2 no longer requires:

```yaml
version: "3.8"
```

It can safely be removed.

---

### Database Connection Issues

If WordPress cannot connect to MySQL:

* Verify the `.env` variables
* Confirm the database service name is `db`
* Restart the application

```bash
docker compose down
docker compose up -d
```

---

# Why Every DevOps Engineer Should Learn Docker Compose

Modern applications are built from multiple services.

Before moving to Kubernetes, Docker Compose provides an excellent way to understand:

* Multi-container deployments
* Service discovery
* Container networking
* Persistent storage
* Environment configuration
* Infrastructure as Code

Many development teams still rely on Docker Compose for local development because it is simple, reliable, and easy to maintain.

---

# Final Thoughts

Docker Compose transformed the way I think about containerized applications. Instead of managing each container individually, I can now describe an entire application stack in a single YAML file and deploy it with one command.

Learning Compose also builds the foundation for Kubernetes, where the same concepts—services, networking, volumes, and declarative configuration—are used at a much larger scale.

If you're beginning your DevOps journey, Docker Compose is a skill worth mastering before moving on to container orchestration platforms.

Thanks for reading! If you're following my **#90DaysOfDevOps** journey, stay tuned as I continue exploring Docker, Kubernetes, CI/CD, and cloud-native technologies.
