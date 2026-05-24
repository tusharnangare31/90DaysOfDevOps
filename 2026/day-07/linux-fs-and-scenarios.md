# Day 07 – Linux File System Hierarchy & Scenario-Based Practice

## Introduction

Today’s focus was on understanding the Linux file system hierarchy and practicing real-world troubleshooting scenarios.

One thing I’ve realized during this DevOps journey is:

> Linux is not only about commands.  
> It is also about understanding where things live inside the system.

If someone understands:
- Where logs are stored
- Where configuration files exist
- Where binaries are located
- Where services store data

then troubleshooting becomes much easier.

That is why today’s learning was very important.

The second part of today’s task was scenario-based troubleshooting practice.

This helped me think more like a DevOps engineer instead of just memorizing commands.

---

# Linux File System Hierarchy

## /

The root (`/`) directory is the starting point of the entire Linux file system.

Everything in Linux starts from `/`.

Example:

```bash
ls -l /
```

Directories found:
- home
- etc
- var

I would use this when:
- Navigating Linux filesystem
- Understanding Linux structure
- Troubleshooting paths

---

# /bin

Contains essential Linux command binaries.

Examples:
- ls
- cp
- mv
- cat

Example:

```bash
ls -l /bin
```

I would use this when:
- Running Linux commands
- Troubleshooting missing binaries

---

# /boot

Contains bootloader files and Linux kernel files.

Examples:
- grub
- vmlinuz

Example:

```bash
ls -l /boot
```

I would use this when:
- Troubleshooting boot issues
- Checking kernel files

---

# /dev

Contains device files representing hardware devices.

Examples:
- sda
- tty
- null

Example:

```bash
ls -l /dev
```

I would use this when:
- Managing disks
- Troubleshooting devices

---

# /etc

Contains Linux configuration files.

Examples:
- hostname
- passwd
- ssh

Example:

```bash
ls -l /etc
```

I would use this when:
- Editing configs
- Managing services
- Troubleshooting settings

---

# /home

Contains home directories of normal users.

Examples:
- Documents
- Downloads

Example:

```bash
ls -l /home
```

I would use this when:
- Managing user files
- Accessing user environments

---

# /lib

Contains essential shared libraries required by system binaries.

Example:

```bash
ls -l /lib
```

I would use this when:
- Troubleshooting dependencies
- Understanding libraries

---

# /lib64

Contains 64-bit shared libraries.

Example:

```bash
ls -l /lib64
```

I would use this when:
- Working with 64-bit applications

---

# /lost+found

Used by Linux filesystem for recovering lost or corrupted files.

Example:

```bash
ls -l /lost+found
```

I would use this when:
- Investigating filesystem corruption

---

# /media

Contains mounted removable devices.

Examples:
- USB drives
- External storage

Example:

```bash
ls -l /media
```

I would use this when:
- Accessing removable media

---

# /mnt

Temporary mount point for manually mounted filesystems.

Example:

```bash
ls -l /mnt
```

I would use this when:
- Mounting disks manually

---

# /opt

Contains optional third-party applications.

Examples:
- Chrome
- External software

Example:

```bash
ls -l /opt
```

I would use this when:
- Installing external applications

---

# /proc

Virtual filesystem containing process and kernel information.

Examples:
- cpuinfo
- meminfo

Example:

```bash
ls -l /proc
```

I would use this when:
- Monitoring processes
- Inspecting system information

---

# /root

Home directory of the root user.

Example:

```bash
ls -l /root
```

I would use this when:
- Performing administrative tasks

---

# /run

Contains runtime information for running processes.

Example:

```bash
ls -l /run
```

I would use this when:
- Troubleshooting running services

---

# /sbin

Contains system administration binaries.

Examples:
- reboot
- shutdown
- fsck

Example:

```bash
ls -l /sbin
```

I would use this when:
- Managing Linux systems

---

# /snap

Contains snap package files and applications.

Example:

```bash
ls -l /snap
```

I would use this when:
- Managing snap packages

---

# /srv

Contains service-related data.

Example:

```bash
ls -l /srv
```

I would use this when:
- Managing server application data

---

# /sys

Virtual filesystem containing kernel and hardware information.

Example:

```bash
ls -l /sys
```

I would use this when:
- Inspecting hardware information

---

# /tmp

Contains temporary files.

Example:

```bash
ls -l /tmp
```

I would use this when:
- Creating temporary test files
- Running scripts

---

# /usr

Contains user applications, binaries, and libraries.

Examples:
- /usr/bin
- /usr/lib

Example:

```bash
ls -l /usr
```

I would use this when:
- Accessing installed software

---

# /var

Contains variable data that changes frequently.

Examples:
- logs
- cache
- mail

Example:

```bash
ls -l /var
```

I would use this when:
- Monitoring applications
- Troubleshooting services

---

# /var/log

Contains Linux and application logs.

Examples:
- syslog
- auth.log
- nginx logs

Example:

```bash
ls -l /var/log
```

I would use this when:
- Debugging applications
- Investigating failures
- Monitoring services

---

# Hands-On Commands Practiced

## Find Largest Log Files

```bash
du -sh /var/log/* 2>/dev/null | sort -h | tail -5
```

### Observation

This helped identify the largest log files inside `/var/log`.

Useful during:
- Disk space troubleshooting
- Log cleanup

---

## Check Hostname Configuration

```bash
cat /etc/hostname
```

### Observation

Displayed system hostname configuration.

Useful for:
- Server identification
- Network troubleshooting

---

## Check Home Directory

```bash
ls -la ~
```

### Observation

Displayed:
- Hidden files
- User directories
- Shell configuration files

Useful for:
- User environment management

---

# Scenario-Based Practice

## Scenario 1 – Service Not Starting

### Problem

A web application service called `myapp` failed after reboot.

---

### Step 1

Command:

```bash
systemctl status myapp
```

Why:
- Checks whether service is active, failed, or stopped

---

### Step 2

Command:

```bash
journalctl -u myapp -n 50
```

Why:
- Displays recent logs related to service

---

### Step 3

Command:

```bash
systemctl is-enabled myapp
```

Why:
- Verifies whether service starts automatically after reboot

---

### Step 4

Command:

```bash
systemctl restart myapp
```

Why:
- Attempts to restart service after troubleshooting

---

# Scenario 2 – High CPU Usage

### Problem

Server becomes slow due to high CPU usage.

---

### Step 1

Command:

```bash
top
```

Why:
- Displays live CPU usage

---

### Step 2

Command:

```bash
htop
```

Why:
- Provides interactive process monitoring

---

### Step 3

Command:

```bash
ps aux --sort=-%cpu | head -10
```

Why:
- Displays top CPU-consuming processes

---

# Scenario 3 – Finding Service Logs

### Problem

Developer wants Docker service logs.

---

### Step 1

Command:

```bash
systemctl status docker
```

Why:
- Verifies service status

---

### Step 2

Command:

```bash
journalctl -u docker -n 50
```

Why:
- Displays latest Docker logs

---

### Step 3

Command:

```bash
journalctl -u docker -f
```

Why:
- Streams live Docker logs

---

# Scenario 4 – Permission Denied

### Problem

`backup.sh` script shows permission denied error.

---

### Step 1

Command:

```bash
ls -l backup.sh
```

Why:
- Checks file permissions

---

### Step 2

Command:

```bash
chmod +x backup.sh
```

Why:
- Adds execute permission

---

### Step 3

Command:

```bash
ls -l backup.sh
```

Why:
- Verifies updated permissions

---

### Step 4

Command:

```bash
./backup.sh
```

Why:
- Executes the script

---

# Important Learning from Today

Today helped me understand:
- Linux filesystem structure
- Where logs and configs are stored
- How services behave
- How troubleshooting flows work

One major realization:

> Good troubleshooting depends heavily on understanding where things exist inside Linux.

If engineers know:
- Where logs live
- Where configs exist
- Where binaries are stored

they can solve problems much faster.

---

# Final Thoughts

Today’s learning felt very practical because it combined:
- Linux filesystem fundamentals
- Real-world troubleshooting scenarios

Instead of only memorizing commands, I started thinking more like an engineer investigating production issues.

The Linux filesystem is starting to feel much more understandable now.

And slowly, all these Linux fundamentals are connecting together into real DevOps workflows.

Day 07 completed.

Still learning. Still improving. Still building.