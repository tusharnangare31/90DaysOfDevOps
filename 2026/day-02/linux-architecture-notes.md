# Linux Architecture, Processes, and systemd

## Linux Kernel
- The Linux kernel is written mainly in the C programming language.
- It is considered the heart of the Linux operating system.
- The kernel directly interacts with hardware components.
- It manages:
  - CPU
  - Memory
  - Devices
  - File systems
  - Networking
  - Processes

---

# User Space
- User space is the area where user applications run.
- Applications cannot directly access hardware.
- They communicate with the kernel using system calls.

## Examples of User Space Applications
- Browser
- VS Code
- Terminal
- Docker
- Python programs

---

# systemd / init

- Everything in Linux works as a process.
- `systemd` is the first process started during system boot.
- It always starts with:

```bash
PID 1
```

- systemd is responsible for:
  - Starting services
  - Managing background processes
  - Monitoring services
  - Handling boot process
  - Restarting failed services

---

# Daemon Process

- Daemon means background process.
- These processes continuously run in the background without user interaction.

## Examples
- nginx
- sshd
- docker daemon

---

# Process Management

- Everything in Linux is treated as a process.
- Every process has:
  - PID (Process ID)
  - Parent Process
  - State
  - Memory allocation

## Process Creation
Processes are mainly created using:
- `fork()`
- `exec()`

### fork()
- Creates a copy of the current process.

### exec()
- Replaces the current process with a new program.

---

# Process States

| State | Meaning |
|---|---|
| Running | Process is actively using CPU |
| Sleeping | Waiting for resource or event |
| Stopped | Process execution paused |
| Zombie | Process completed but still exists temporarily |

---

# Why systemd Matters

- systemd is important because it is the first init process that starts the operating system.
- It manages all services and background processes in Linux.
- It can automatically restart failed services.
- It also helps in troubleshooting using logs and service status.

---

# Useful systemd Commands

## Check Service Status

```bash
systemctl status nginx
```

## Start Service

```bash
systemctl start nginx
```

## Stop Service

```bash
systemctl stop nginx
```

## Restart Service

```bash
systemctl restart nginx
```

---

# 5 Daily Linux Commands

| Command | Purpose |
|---|---|
| `ls` | List files and directories |
| `cd` | Change directory |
| `pwd` | Show current working directory |
| `cp` | Copy files |
| `cat` | Display file contents |

---

# Why Linux Matters in DevOps

Linux is the foundation of most cloud and production systems.

Understanding Linux helps DevOps engineers:
- Debug failed applications
- Manage services
- Monitor system resources
- Troubleshoot production issues
- Work confidently with servers and cloud infrastructure
This knowledge is essential for efficient incident response and system management.
