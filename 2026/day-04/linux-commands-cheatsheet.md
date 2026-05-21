# Linux Practice – Day 04

Today’s goal was to practice Linux fundamentals using real commands related to processes, services, and logs.

This practice helped me understand how Linux systems are monitored and managed in real-world DevOps environments.

---

# Process Checks

## 1. Check Running Processes

```bash
ps aux
```

### Output

```bash
USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root         1  0.0  0.1 169404 11584 ?        Ss   10:00   0:02 /sbin/init
tushar    2543  1.2  2.3 245678 45876 ?        Sl   10:10   0:15 code
root      3321  0.0  0.2  12844  5640 ?        Ss   10:20   0:01 sshd
```

### Learning

This command displays:
- Running processes
- Process ID (PID)
- CPU usage
- Memory usage
- Running user

---

## 2. Real-Time Process Monitoring

```bash
top
```

### Learning

The `top` command shows:
- Real-time CPU usage
- Memory usage
- Running tasks
- System load

Useful for identifying high resource-consuming applications.

---

## 3. Find Docker Process

```bash
pgrep docker
```

### Output

```bash
1456
```

### Learning

`pgrep` helps quickly find process IDs using process names.

---

# Service Checks

## 4. Check Docker Service Status

```bash
systemctl status docker
```

### Output

```bash
● docker.service - Docker Application Container Engine
   Loaded: loaded (/lib/systemd/system/docker.service)
   Active: active (running)
```

### Learning

This command helps verify:
- Whether service is running
- Service status
- Main PID
- Logs and errors

---

## 5. List Running Services

```bash
systemctl list-units --type=service
```

### Output

```bash
docker.service        loaded active running Docker Application Container Engine
ssh.service           loaded active running OpenBSD Secure Shell server
cron.service          loaded active running Regular background program processing daemon
```

### Learning

This command displays all active Linux services managed by systemd.

---

# Log Checks

## 6. Check Docker Logs

```bash
journalctl -u docker
```

### Output

```bash
Jul 20 10:15:01 systemd[1]: Started Docker Application Container Engine.
Jul 20 10:16:10 dockerd[1456]: API listen on /run/docker.sock
```

### Learning

`journalctl` helps analyze:
- Service logs
- Errors
- Restart failures
- Service startup activity

---

## 7. View Last 50 Log Lines

```bash
tail -n 50 /var/log/syslog
```

### Output

```bash
Jul 20 10:22:01 CRON[2456]: pam_unix(cron:session): session opened
Jul 20 10:23:10 systemd[1]: Started Session 24 of user tushar.
```

### Learning

Useful for:
- Checking recent system events
- Monitoring Linux activity
- Debugging issues

---

# Mini Troubleshooting Practice

## Problem

Wanted to verify whether Docker service was running properly.

---

## Step 1 – Check Docker Service Status

```bash
systemctl status docker
```

Result:
- Docker service was active and running.

---

## Step 2 – Verify Docker Process

```bash
pgrep docker
```

Result:
- Docker daemon process was running successfully.

---

## Step 3 – Inspect Docker Logs

```bash
journalctl -u docker
```

Result:
- No major errors found.
- Docker service started successfully.

---

# Additional Commands Practiced

## Check Disk Usage

```bash
df -h
```

Displays disk usage information.

---

## Check Memory Usage

```bash
free -h
```

Displays RAM usage.

---

## Check Open Ports

```bash
ss -tulnp
```

Displays active ports and network connections.

---

# Learning Outcome

Today’s practice improved my understanding of:
- Linux process management
- systemd services
- Log inspection
- Basic troubleshooting workflow

I realized that Linux troubleshooting is mostly about:
- Observing processes
- Checking services
- Reading logs carefully

These fundamentals are extremely important for DevOps, Cloud, Docker, and Kubernetes environments.

Day 04 completed.