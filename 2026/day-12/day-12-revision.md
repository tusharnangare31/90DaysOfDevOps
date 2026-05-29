# Day 12 – Revision & Reflection (Days 01–11)

## Introduction

Today was a revision day focused on reinforcing the Linux and DevOps fundamentals learned during the first 11 days of the challenge.

Instead of learning new concepts, I revisited important commands, repeated a few practical exercises, and reviewed my notes to strengthen retention.

The goal was simple:

> Build confidence through repetition.

---

# Review of Days 01–11

## Topics Covered So Far

### Day 01
- DevOps roadmap
- Learning goals
- Linux introduction

### Day 02
- Linux architecture
- Kernel
- User space
- Processes
- systemd

### Day 03
- Linux command cheat sheet
- File system commands
- Process commands
- Networking commands

### Day 04
- Process monitoring
- Services
- Logs
- Troubleshooting basics

### Day 05
- CPU monitoring
- Memory monitoring
- Log analysis
- Troubleshooting runbooks

### Day 06
- File creation
- File reading
- Redirection operators
- tee command

### Day 07
- Linux File System Hierarchy
- Important directories
- Scenario-based troubleshooting

### Day 08
- AWS EC2
- SSH
- Nginx deployment
- Cloud infrastructure

### Day 09
- User management
- Group management
- Shared directories

### Day 10
- File permissions
- chmod
- Read, write, execute permissions

### Day 11
- File ownership
- chown
- chgrp
- Recursive ownership

---

# Process & Service Review

Commands practiced:

```bash
ps aux
```

```bash
systemctl status nginx
```

```bash
journalctl -u nginx -n 20
```

### Observations

- `ps aux` displays running processes.
- `systemctl status` quickly shows service health.
- `journalctl` helps troubleshoot service-related issues.

---

# File Operations Review

Commands practiced:

```bash
echo "Revision Day" >> notes.txt
```

```bash
mkdir revision-test
```

```bash
cp notes.txt revision-test/
```

### Observations

- File operations are becoming more comfortable.
- Redirection operators are useful for quick file updates.
- Creating and copying files is now much faster than Day 01.

---

# Permissions & Ownership Review

Commands practiced:

```bash
chmod 755 script.sh
```

```bash
sudo chown tokyo devops-file.txt
```

```bash
ls -l
```

### Observations

- Permissions control access.
- Ownership controls responsibility.
- Always verify changes using `ls -l`.

---

# Top 5 Commands for Troubleshooting

These are the commands I would use first during an incident:

## 1. ps aux

```bash
ps aux
```

Used to inspect running processes.

---

## 2. systemctl status

```bash
systemctl status nginx
```

Used to verify service health.

---

## 3. journalctl

```bash
journalctl -u nginx -n 50
```

Used to investigate service logs.

---

## 4. df -h

```bash
df -h
```

Used to check disk usage.

---

## 5. ls -l

```bash
ls -l
```

Used to inspect permissions and ownership.

---

# User & Group Review

Commands practiced:

```bash
id tokyo
```

```bash
groups tokyo
```

```bash
ls -l
```

### Observations

- Users can belong to multiple groups.
- Group assignments control shared access.
- Ownership verification is important before troubleshooting permission issues.

---

# Mini Self-Check

## 1. Which 3 commands save you the most time right now, and why?

### ps aux
Quickly identifies running processes.

### systemctl status
Provides instant service health information.

### ls -l
Shows permissions and ownership in one command.

---

## 2. How do you check if a service is healthy?

Commands:

```bash
systemctl status nginx
```

```bash
journalctl -u nginx -n 50
```

```bash
ps aux | grep nginx
```

These commands confirm:
- Service status
- Recent logs
- Running processes

---

## 3. How do you safely change ownership and permissions?

Example:

```bash
sudo chown tokyo:developers app.log
```

```bash
chmod 640 app.log
```

Always verify using:

```bash
ls -l app.log
```

---

## 4. What will you focus on improving in the next 3 days?

- More Linux administration practice
- Faster troubleshooting workflows
- Better understanding of networking concepts
- Stronger command-line confidence
- Cloud and DevOps fundamentals

---

# Key Takeaways

- Linux fundamentals are becoming easier to understand.
- Repetition improves command recall.
- Troubleshooting starts with observation before action.
- Permissions and ownership are critical for security.
- Consistent practice is more important than learning too many new topics.

---

# Final Thoughts

Day 12 was a valuable reminder that learning is not only about moving forward but also about reinforcing what has already been learned.

Reviewing the first 11 days helped strengthen my understanding of Linux, permissions, ownership, services, troubleshooting, and cloud basics.

The foundation is becoming stronger with every day of practice.

Day 12 completed.

Still learning. Still improving. Still building.