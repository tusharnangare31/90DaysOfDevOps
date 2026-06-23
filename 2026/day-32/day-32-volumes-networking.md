# Day 32 – Docker Networking & Volumes: Connecting Containers Like Real Applications

## Introduction

Containers are powerful on their own, but real-world applications rarely run as a single container.

A typical application consists of multiple services:

* Frontend Container
* Backend Container
* Database Container

For these services to work together, containers must communicate with each other and persist data even when containers are deleted.

Today I explored Docker Networking and Volumes and learned how containers communicate using custom networks and how Docker volumes provide persistent storage.

---

# Task 1: Understanding Docker Networks

Docker provides networking capabilities that allow containers to communicate with each other.

### List Available Networks

```bash
docker network ls
```

Example Output:

```bash
NETWORK ID     NAME      DRIVER
xxxxxxx        bridge    bridge
xxxxxxx        host      host
xxxxxxx        none      null
```

### Default Networks

| Network | Purpose                        |
| ------- | ------------------------------ |
| bridge  | Default network for containers |
| host    | Shares host network            |
| none    | No networking                  |

---

# Task 2: Creating a Custom Network

Create a custom bridge network:

```bash
docker network create my-app-net
```

Verify:

```bash
docker network ls
```

Output:

```bash
my-app-net
```

Custom networks are preferred because Docker automatically provides DNS resolution between containers.

---

# Task 3: Container-to-Container Communication

Launch two Ubuntu containers on the same network.

### Container 1

```bash
docker run -dit \
--name container1 \
--network my-app-net \
ubuntu bash
```

### Container 2

```bash
docker run -dit \
--name container2 \
--network my-app-net \
ubuntu bash
```

Install ping utility:

```bash
apt update
apt install iputils-ping -y
```

Test communication:

```bash
ping container1
```

Result:

```bash
PING container1 (172.18.0.2)
64 bytes from container1
```

Containers successfully communicated using container names instead of IP addresses.

---

# Why Does This Work?

### Default Bridge Network

Containers communicate using IP addresses.

Example:

```bash
172.17.0.2
172.17.0.3
```

No automatic DNS-based discovery.

---

### Custom Bridge Network

Docker automatically creates an internal DNS service.

Containers can communicate using:

```bash
container1
container2
mysql-db
app-container
```

instead of remembering IP addresses.

This makes multi-container applications much easier to manage.

---

# Task 4: Docker Volumes

Volumes are used for persistent storage.

Without volumes:

* Container deleted = data lost

With volumes:

* Container deleted = data remains

Create a volume:

```bash
docker volume create mysql-data
```

Verify:

```bash
docker volume ls
```

---

# Task 5: Database Container with Persistent Storage

Run MySQL container using the volume.

```bash
docker run -d \
--name mysql-db \
--network project-net \
-v mysql-data:/var/lib/mysql \
-e MYSQL_ROOT_PASSWORD=root123 \
mysql:8
```

Explanation:

| Option              | Purpose               |
| ------------------- | --------------------- |
| -v                  | Mount volume          |
| mysql-data          | Volume name           |
| /var/lib/mysql      | Database storage path |
| MYSQL_ROOT_PASSWORD | Root password         |

This ensures database data survives container recreation.

---

# Task 6: Application + Database Communication

Create a dedicated application network:

```bash
docker network create project-net
```

Run application container:

```bash
docker run -dit \
--name app-container \
--network project-net \
ubuntu bash
```

Install ping:

```bash
apt update
apt install iputils-ping -y
```

Verify connectivity:

```bash
ping mysql-db
```

Output:

```bash
PING mysql-db (172.19.0.2)
```

The application container successfully reached the database container using its container name.

---

# Inspecting the Network

To see connected containers:

```bash
docker network inspect project-net
```

Output shows:

```bash
app-container
mysql-db
```

along with their assigned IP addresses.

This confirms both containers are attached to the same network.

---

# Key Concepts Learned

## Docker Network

A virtual network that allows containers to communicate.

### Create

```bash
docker network create my-app-net
```

### List

```bash
docker network ls
```

### Inspect

```bash
docker network inspect my-app-net
```

---

## Docker Volume

Persistent storage managed by Docker.

### Create

```bash
docker volume create mysql-data
```

### List

```bash
docker volume ls
```

### Inspect

```bash
docker volume inspect mysql-data
```

---

# Real-World Use Case

A typical production setup might look like:

```text
Frontend Container
        |
        v
Backend Container
        |
        v
Database Container
```

All containers communicate through Docker networks while volumes ensure important data remains safe.

This is the foundation of containerized applications and prepares the path toward Docker Compose, Kubernetes, and production-grade deployments.

---

# Conclusion

Today I learned how Docker networking enables seamless communication between containers and how Docker volumes provide persistent storage.

Key takeaways:

* Created custom Docker networks
* Connected containers using container names
* Used Docker's built-in DNS resolution
* Created and mounted persistent volumes
* Connected an application container to a database container
* Inspected network configurations and container IPs

These concepts are essential for building real-world multi-container applications and form the foundation for Docker Compose and Kubernetes.
