# Linux Commands Cheat Sheet

Linux commands are essential for system administration, troubleshooting, DevOps, and cloud environments. This cheat sheet covers commonly used Linux commands for filesystem management, process management, networking, monitoring, and permissions.

---

# Filesystem Commands

The Linux filesystem provides the structure for storing and organizing files and directories.

---

## pwd - Print Working Directory

The `pwd` command prints the full path of the current working directory.

```bash
pwd
```

This command helps identify your current location in the filesystem.

---

## ls - List Directory Contents

The `ls` command lists files and directories.

```bash
ls
```

### Common Options

- `-l` → Detailed list
- `-a` → Show hidden files
- `-h` → Human-readable file sizes

Example:

```bash
ls -lah
```

---

## cd - Change Directory

The `cd` command changes the current directory.

```bash
cd /path/to/directory
```

### Shortcuts

```bash
cd ~
```

Move to home directory.

```bash
cd ..
```

Move one level up.

```bash
cd -
```

Switch to previous directory.

---

## mkdir - Make Directories

Creates directories.

```bash
mkdir project
```

### Option

```bash
mkdir -p devops/project/app
```

Creates nested directories.

---

## rmdir - Remove Empty Directory

Removes empty directories.

```bash
rmdir test
```

---

## rm - Remove Files or Directories

Deletes files or directories.

```bash
rm file.txt
```

### Common Options

- `-r` → Recursive delete
- `-f` → Force delete
- `-i` → Interactive confirmation

Example:

```bash
rm -rf folder
```

---

## cp - Copy Files and Directories

Copies files or directories.

```bash
cp source.txt backup.txt
```

### Option

```bash
cp -r folder1 folder2
```

Recursively copies directories.

---

## mv - Move or Rename Files

Moves or renames files.

```bash
mv old.txt new.txt
```

---

## ln - Create Links

Creates hard or symbolic links.

```bash
ln source.txt hardlink
```

Symbolic link:

```bash
ln -s source.txt softlink
```

---

## touch - Create Empty File

Creates a new empty file.

```bash
touch app.log
```

---

## cat - Display File Content

Displays content of a file.

```bash
cat file.txt
```

---

## less - View Large Files

Views large files page by page.

```bash
less app.log
```

---

## more - View File Content

Displays file content page by page.

```bash
more file.txt
```

---

## head - View Beginning of File

Shows first lines of a file.

```bash
head file.txt
```

---

## tail - View End of File

Shows last lines of a file.

```bash
tail -f app.log
```

`-f` is used for real-time log monitoring.

---

## find - Search Files

Searches files and directories.

```bash
find /home -name "file.txt"
```

### Options

- `-type f` → Files only
- `-type d` → Directories only

---

## locate - Quickly Find Files

Quickly searches files using database indexing.

```bash
locate nginx.conf
```

---

# Filesystem Information Commands

---

## df - Disk Space Usage

Displays filesystem disk usage.

```bash
df -h
```

`-h` shows human-readable sizes.

---

## du - Directory Space Usage

Shows directory size.

```bash
du -sh folder/
```

---

## mount - Mount Filesystem

Mounts a storage device.

```bash
mount /dev/sda1 /mnt
```

Unmount filesystem:

```bash
umount /mnt
```

---

## fsck - Filesystem Check

Checks and repairs filesystem errors.

```bash
fsck /dev/sda1
```

---

## mkfs - Create Filesystem

Formats storage partitions.

```bash
mkfs.ext4 /dev/sda1
```

---

# Process Management Commands

Everything in Linux works as a process.

---

## ps - Show Running Processes

Displays running processes.

```bash
ps aux
```

---

## pidof - Get Process ID

Shows PID of a process.

```bash
pidof nginx
```

---

## pgrep - Search Process by Name

Finds process IDs by name.

```bash
pgrep chrome
```

---

## top - Real-Time Process Monitoring

Shows CPU and memory usage in real time.

```bash
top
```

---

## htop - Interactive Process Viewer

Advanced version of top.

```bash
htop
```

---

## kill - Kill Process

Terminates process using PID.

```bash
kill 1234
```

---

## killall - Kill Process by Name

Kills all matching processes.

```bash
killall firefox
```

---

## pkill - Kill Process Using Pattern

Kills processes matching a name.

```bash
pkill nginx
```

---

## jobs - Show Background Jobs

Displays shell background jobs.

```bash
jobs
```

---

## fg - Bring Job to Foreground

Brings background process to foreground.

```bash
fg %1
```

---

## bg - Run Job in Background

Moves process to background.

```bash
bg %1
```

---

## nice - Start Process with Priority

Runs process with lower priority.

```bash
nice -n 10 script.sh
```

---

## renice - Change Process Priority

Changes running process priority.

```bash
renice -n 5 1234
```

---

## nohup - Run Process After Logout

Keeps process running after logout.

```bash
nohup python app.py &
```

---

## screen - Create Virtual Terminal

Creates detachable terminal sessions.

```bash
screen
```

---

## taskset - Set CPU Affinity

Assigns CPU cores to process.

```bash
taskset -c 0,1 app
```

---

## pstree - Show Process Tree

Displays hierarchical process tree.

```bash
pstree
```

---

## lsof - List Open Files

Shows open files and ports.

```bash
lsof -i :80
```

---

## systemctl - Manage Services

Manages Linux services.

```bash
systemctl status nginx
```

Other examples:

```bash
systemctl start nginx
```

```bash
systemctl restart nginx
```

---

# Networking Commands

Networking commands help troubleshoot server and connectivity issues.

---

## ping - Check Network Connectivity

Checks if host is reachable.

```bash
ping google.com
```

---

## ip addr - Show IP Address

Displays network interfaces and IP addresses.

```bash
ip addr
```

---

## ifconfig - Show Network Interfaces

Displays interface configuration.

```bash
ifconfig
```

---

## curl - Send HTTP Requests

Tests APIs and websites.

```bash
curl https://google.com
```

---

## wget - Download Files

Downloads files from internet.

```bash
wget file_url
```

---

## netstat - Show Network Connections

Displays open ports and connections.

```bash
netstat -tulnp
```

---

## ss - Socket Statistics

Modern alternative to netstat.

```bash
ss -tulnp
```

---

## dig - DNS Lookup Tool

Queries DNS records.

```bash
dig google.com
```

---

## nslookup - DNS Query

Checks DNS information.

```bash
nslookup google.com
```

---

## mtr - Network Diagnostic Tool

Combines ping and traceroute.

```bash
mtr google.com
```

---

## tcpdump - Capture Network Packets

Captures network traffic.

```bash
tcpdump -i eth0
```

---

## traceroute - Trace Packet Route

Shows network packet path.

```bash
traceroute google.com
```

---

# Log & Monitoring Commands

These commands help monitor Linux systems and troubleshoot issues.

---

## journalctl - View systemd Logs

Displays service logs.

```bash
journalctl -u nginx
```

---

## grep - Search Text

Searches patterns in files.

```bash
grep error app.log
```

---

## free - Show Memory Usage

Displays RAM usage.

```bash
free -h
```

---

## uptime - Show System Uptime

Displays running time and load average.

```bash
uptime
```

---

## vmstat - System Performance Statistics

Shows memory and CPU statistics.

```bash
vmstat
```

---

## iostat - CPU and Disk Statistics

Displays disk and CPU performance.

```bash
iostat
```

---

# Permissions Commands

Linux security depends heavily on permissions.

---

## chmod - Change Permissions

Changes file permissions.

```bash
chmod 755 script.sh
```

---

## chown - Change Ownership

Changes file owner.

```bash
chown user:user file.txt
```

---

## chgrp - Change Group Ownership

Changes group ownership.

```bash
chgrp devops file.txt
```

---

## sudo - Run Command as Root

Executes commands with admin privileges.

```bash
sudo apt update
```

---

## whoami - Show Current User

Displays current logged-in user.

```bash
whoami
```

---

## id - Show User and Group IDs

Displays user and group information.

```bash
id
```

---

# Archive & Compression Commands

---

## tar - Archive Files

Creates archive files.

```bash
tar -cvf backup.tar folder/
```

---

## zip - Compress Files

Compresses files.

```bash
zip backup.zip file.txt
```

---

## unzip - Extract Zip Files

Extracts zip archives.

```bash
unzip backup.zip
```

---

# Utility Commands

---

## history - Show Command History

Displays previously used commands.

```bash
history
```

---

## clear - Clear Terminal

Clears terminal screen.

```bash
clear
```

---

## uname - Show System Information

Displays Linux system details.

```bash
uname -a
```

---

## hostname - Show Hostname

Displays system hostname.

```bash
hostname
```

---

## which - Locate Command Path

Shows command binary path.

```bash
which docker
```

---

## man - Open Manual Pages

Displays command documentation.

```bash
man ls
```

---

## nano - Terminal Text Editor

Opens Nano editor.

```bash
nano file.txt
```

---

# Conclusion

Linux commands are one of the most important skills for DevOps and Cloud Engineers.

Strong command-line knowledge helps in:
- Troubleshooting servers
- Monitoring applications
- Debugging networking issues
- Managing Linux systems
- Handling production incidents efficiently

Understanding these commands improves operational confidence and real-world troubleshooting ability.