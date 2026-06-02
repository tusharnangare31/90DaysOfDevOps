# Day 15 – Networking Concepts: DNS, IP, Subnets & Ports

## Introduction

Today I continued building my networking foundation by learning some of the most important concepts every DevOps engineer uses daily:

* DNS
* IP Addressing
* Public vs Private Networks
* CIDR Notation
* Subnetting
* Ports and Services

Understanding these concepts is essential because every application, server, database, API, and cloud service depends on networking.

---

# Task 1 – DNS: How Names Become IPs

## What Happens When We Type google.com in a Browser?

When I type `google.com` into a browser:

1. The browser asks a DNS server for the IP address of google.com.
2. The DNS server looks up the requested record.
3. DNS returns Google's IP address.
4. The browser connects to that IP address.
5. Google sends the webpage back to the browser.

Without DNS, we would need to remember IP addresses instead of domain names.

---

## Common DNS Record Types

### A Record

Maps a domain name to an IPv4 address.

Example:

```text
google.com → 142.250.183.78
```

---

### AAAA Record

Maps a domain name to an IPv6 address.

---

### CNAME Record

Creates an alias for another domain.

Example:

```text
www.example.com → example.com
```

---

### MX Record

Specifies mail servers responsible for email delivery.

---

### NS Record

Identifies authoritative DNS servers for a domain.

---

## DNS Lookup Using dig

Command:

```bash
dig google.com
```

Example Output:

```text
google.com.   300   IN   A   142.250.183.78
```

### Observation

* Record Type: A
* TTL: 300 seconds
* IP Address: 142.250.183.78

The TTL determines how long DNS results can be cached.

---

# Task 2 – IP Addressing

## What is an IPv4 Address?

An IPv4 address uniquely identifies a device on a network.

Example:

```text
192.168.1.10
```

IPv4 consists of:

```text
4 octets
8 bits per octet
32 bits total
```

Range:

```text
0.0.0.0 → 255.255.255.255
```

---

## Public vs Private IP Addresses

### Public IP

Accessible from the internet.

Example:

```text
8.8.8.8
```

Google Public DNS

---

### Private IP

Used inside private networks.

Example:

```text
192.168.1.100
```

Not directly reachable from the internet.

---

## Private IP Ranges

### Class A

```text
10.0.0.0 – 10.255.255.255
```

---

### Class B

```text
172.16.0.0 – 172.31.255.255
```

---

### Class C

```text
192.168.0.0 – 192.168.255.255
```

---

## Checking Local IP Addresses

Command:

```bash
ip addr show
```

### Observation

My local machine IP belongs to one of the private ranges.

Example:

```text
192.168.x.x
```

or

```text
10.x.x.x
```

Private addresses are commonly used in home networks and cloud environments.

---

# Task 3 – CIDR & Subnetting

## What Does /24 Mean?

Example:

```text
192.168.1.0/24
```

The `/24` means:

```text
24 bits = Network Portion
8 bits = Host Portion
```

Equivalent subnet mask:

```text
255.255.255.0
```

---

## Why Do We Subnet?

Subnetting helps:

* Organize networks
* Improve security
* Reduce broadcast traffic
* Efficiently use IP addresses

Large networks become easier to manage when divided into smaller subnets.

---

## CIDR Table

| CIDR | Subnet Mask     | Total IPs | Usable Hosts |
| ---- | --------------- | --------- | ------------ |
| /24  | 255.255.255.0   | 256       | 254          |
| /16  | 255.255.0.0     | 65,536    | 65,534       |
| /28  | 255.255.255.240 | 16        | 14           |

---

## Host Calculation

### /24

```text
2^8 = 256 IPs
256 - 2 = 254 usable hosts
```

---

### /16

```text
2^16 = 65,536 IPs
65,536 - 2 = 65,534 usable hosts
```

---

### /28

```text
2^4 = 16 IPs
16 - 2 = 14 usable hosts
```

---

# Task 4 – Ports: The Doors to Services

## What is a Port?

A port is a logical communication endpoint used by applications.

Ports allow multiple services to run on the same IP address.

Without ports, devices would not know which application should receive incoming traffic.

---

## Common Ports

| Port  | Service |
| ----- | ------- |
| 22    | SSH     |
| 80    | HTTP    |
| 443   | HTTPS   |
| 53    | DNS     |
| 3306  | MySQL   |
| 6379  | Redis   |
| 27017 | MongoDB |

---

## Checking Listening Ports

Command:

```bash
ss -tulpn
```

Example:

```text
tcp LISTEN 0 4096 *:22
```

```text
tcp LISTEN 0 4096 *:80
```

### Observation

Port 22:

```text
SSH Service
```

Port 80:

```text
Nginx Web Server
```

This helps identify which services are actively accepting connections.

---

# Task 5 – Putting It Together

## Scenario 1

Command:

```bash
curl http://myapp.com:8080
```

Concepts involved:

* DNS resolves `myapp.com`
* IP address identifies the destination
* Port 8080 identifies the application
* TCP establishes the connection
* HTTP transfers the request and response

---

## Scenario 2

Application cannot reach:

```text
10.0.1.50:3306
```

Checks I would perform:

1. Verify network connectivity

```bash
ping 10.0.1.50
```

2. Verify port accessibility

```bash
nc -zv 10.0.1.50 3306
```

3. Verify database service status

```bash
systemctl status mysql
```

4. Verify firewall rules

```bash
ss -tulpn
```

---

# Important Commands Practiced

```bash
dig google.com

ip addr show

ss -tulpn

ping google.com

nc -zv localhost 22

curl http://myapp.com:8080
```

---

# What I Learned

### 1. DNS Translates Names into IP Addresses

Without DNS, users would need to remember numerical IP addresses instead of domain names.

---

### 2. CIDR Determines Network Size

CIDR notation helps define how many hosts can exist inside a subnet.

---

### 3. Ports Allow Multiple Services on One Server

Different applications communicate using different ports while sharing the same IP address.

---

# Key Takeaways

* DNS converts names into IP addresses.
* IPv4 addresses identify devices on a network.
* Private IP ranges are used inside organizations and cloud environments.
* CIDR notation determines network size and host capacity.
* Ports identify services running on a machine.
* Understanding DNS, IPs, subnets, and ports is essential for troubleshooting.

---

# Final Thoughts

Today's learning helped me understand how networking works beneath the applications we use every day.

I learned how DNS resolves domain names, how IP addresses identify devices, how subnetting organizes networks, and how ports allow services to communicate efficiently.

These concepts are fundamental for Linux administration, cloud infrastructure, Kubernetes, Docker, and DevOps engineering.

A strong networking foundation makes troubleshooting faster and helps build more reliable systems.

Day 15 completed.

Still learning. Still improving. Still building.
