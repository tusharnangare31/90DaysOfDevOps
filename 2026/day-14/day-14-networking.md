# Day 14 – Networking Fundamentals & Hands-on Checks

## Introduction

Today I explored one of the most important foundations of modern IT infrastructure:

> Networking Fundamentals

Whether it's cloud computing, Linux administration, DevOps, containers, CI/CD pipelines, or web applications, everything depends on networking.

A server is only useful if it can communicate with other systems. Understanding how data travels across networks and knowing the right commands to troubleshoot connectivity issues are essential skills for every DevOps engineer.

Today's goal was to understand networking concepts and practice real-world troubleshooting commands that are commonly used in production environments.

---

# Understanding Network Models

Before jumping into commands, I reviewed the two networking models that explain how communication happens between devices.

## OSI Model

The OSI (Open Systems Interconnection) model contains seven layers.

```text
Layer 7 – Application
Layer 6 – Presentation
Layer 5 – Session
Layer 4 – Transport
Layer 3 – Network
Layer 2 – Data Link
Layer 1 – Physical
```

Each layer performs a specific task and works together to deliver data from one system to another.

---

## TCP/IP Model

The TCP/IP model is the networking model used in real-world systems.

```text
Application
Transport
Internet
Link
```

Most networking tools, protocols, and internet communication follow this model.

---

# Where Common Protocols Fit

Understanding where protocols operate helps identify problems faster.

| Protocol     | Layer       |
| ------------ | ----------- |
| HTTP / HTTPS | Application |
| DNS          | Application |
| TCP          | Transport   |
| UDP          | Transport   |
| IP           | Internet    |
| Ethernet     | Link        |

Real Example:

```text
curl https://google.com

HTTPS
  ↓
TCP
  ↓
IP
```

This helped me visualize how a request travels through the networking stack.

---

# Checking Network Identity

The first step in troubleshooting any network issue is knowing your machine's IP address.

Command:

```bash
hostname -I
```

Alternative:

```bash
ip addr show
```

### Observation

* Displays IP addresses assigned to the machine.
* Helps identify the active network interface.
* Useful when verifying server connectivity.

---

# Testing Connectivity with Ping

One of the fastest troubleshooting commands is `ping`.

Command:

```bash
ping google.com
```

### Observation

* Successfully reached the destination host.
* Measured latency between systems.
* Verified network connectivity.
* Observed packet loss statistics.

Example:

```text
Packet Loss: 0%
Average Latency: Low
```

This command immediately tells whether the target host is reachable.

---

# Understanding the Network Path

To see how traffic travels across the internet, I used traceroute.

Command:

```bash
traceroute google.com
```

### Observation

* Traffic passed through multiple routers.
* Each hop represented a network device.
* Helped identify routing paths between source and destination.

Traceroute is useful when connectivity exists but response times are unusually high.

---

# Viewing Listening Services

Next, I checked which services were actively listening on the machine.

Command:

```bash
ss -tulpn
```

### Observation

* Displayed listening TCP and UDP ports.
* Showed active services.
* Helped identify open ports on the system.

Examples:

```text
SSH    → Port 22
Nginx  → Port 80
```

This command is extremely useful during troubleshooting and security audits.

---

# DNS Resolution Testing

DNS converts human-readable domain names into IP addresses.

Commands used:

```bash
nslookup google.com
```

and

```bash
dig google.com
```

### Observation

* Successfully resolved the domain name.
* Returned Google's public IP addresses.
* Confirmed that DNS services were functioning correctly.

Without DNS, websites cannot be reached using domain names.

---

# HTTP Connectivity Check

To verify application-layer communication, I used curl.

Command:

```bash
curl -I https://google.com
```

### Observation

* Successfully received an HTTP response.
* Verified HTTPS communication.
* Confirmed application-level connectivity.

Example:

```text
HTTP/2 200 OK
```

or

```text
HTTP/1.1 301 Moved Permanently
```

This command is frequently used when troubleshooting web applications and APIs.

---

# Network Connection Snapshot

To inspect current connections:

Command:

```bash
netstat -an | head
```

### Observation

* Displayed active connections.
* Showed listening ports.
* Provided a quick overview of network activity.

This is useful for identifying suspicious or unexpected connections.

---

# Mini Task – Port Probe

After identifying listening services, I tested port accessibility.

Example:

```bash
nc -zv localhost 22
```

or

```bash
nc -zv localhost 80
```

### Observation

* Confirmed whether the port was reachable.
* Verified that the service was actively listening.

If the port was not reachable, the next checks would be:

```bash
systemctl status nginx
```

```bash
ss -tulpn
```

```bash
journalctl -u nginx
```

These commands help determine whether the issue is related to the service, firewall, or configuration.

---

# Important Commands Practiced

```bash
hostname -I

ip addr show

ping google.com

traceroute google.com

ss -tulpn

nslookup google.com

dig google.com

curl -I https://google.com

netstat -an | head

nc -zv localhost 22
```

---

# Reflection

## Which Command Gives the Fastest Signal?

For basic connectivity issues:

```bash
ping
```

It immediately confirms whether a target host is reachable.

---

## If DNS Fails, What Would I Check?

DNS belongs to the Application Layer.

Commands:

```bash
nslookup
```

```bash
dig
```

These help verify whether domain names are resolving correctly.

---

## If HTTP 500 Appears?

HTTP errors occur at the Application Layer.

Commands:

```bash
systemctl status nginx
```

```bash
journalctl -u nginx
```

These help identify server-side application issues.

---

## Two Follow-Up Checks During an Incident

```bash
ss -tulpn
```

```bash
curl -I <service-url>
```

These quickly verify service availability and application health.

---

# What I Learned

### 1. Networking Troubleshooting Starts with Simple Commands

Commands such as ping, traceroute, and curl provide immediate insight into connectivity problems.

### 2. DNS Is a Critical Service

Without DNS resolution, applications and websites become difficult to access.

### 3. Understanding Network Layers Simplifies Troubleshooting

Knowing where protocols operate helps isolate issues faster and more efficiently.

---

# Key Takeaways

* OSI and TCP/IP models explain how systems communicate.
* Ping verifies reachability.
* Traceroute reveals the network path.
* DNS translates domain names into IP addresses.
* Curl validates HTTP and HTTPS communication.
* Open ports reveal active services.
* Networking knowledge is essential for DevOps and cloud engineering.

---

# Final Thoughts

Today's networking exercises helped me connect theoretical concepts with practical troubleshooting.

By practicing commands such as ping, traceroute, ss, nslookup, dig, curl, netstat, and nc, I gained a better understanding of how systems communicate and how network issues can be diagnosed quickly.

Networking is the backbone of modern infrastructure, and developing strong networking fundamentals will make future cloud, DevOps, and system administration tasks much easier.

Day 14 completed.

Still learning. Still improving. Still building.
