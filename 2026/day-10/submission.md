# Day 10 – File Permissions & File Operations Challenge

## Introduction

Today’s focus was on one of the most important Linux concepts:

> File Permissions & File Operations

Every Linux system depends heavily on files.

Logs, scripts, configurations, applications, services — almost everything inside Linux is stored as files.

That means understanding:
- File creation
- File reading
- Permissions
- Ownership
- Execute access

is extremely important for DevOps engineers.

Today’s challenge helped me understand how Linux controls access to files and directories using permissions.

I also practiced:
- Creating files
- Reading files
- Modifying permissions
- Executing scripts
- Understanding permission errors

This was one of the most practical Linux exercises so far.

---

# Task 1 – Creating Files

The first task was creating different types of files.

---

# Creating Empty File

Command used:

```bash
touch devops.txt
```

This created an empty file named:

```text
devops.txt
```

The `touch` command is commonly used to:
- Create files
- Update timestamps

---

# Creating notes.txt with Content

Command used:

```bash
echo "Linux permissions are important" > notes.txt
```

Then added more content using:

```bash
echo "DevOps engineers use chmod daily" >> notes.txt
```

```bash
echo "Security starts with proper permissions" >> notes.txt
```

This helped me understand:
- `>` overwrites file content
- `>>` appends new content

---

# Creating Shell Script Using Vim

Command used:

```bash
vim script.sh
```

Inside the file:

```bash
echo "Hello DevOps"
```

This created a simple shell script.

I also practiced:
- Opening files using vim
- Editing content
- Saving files

---

# Verifying Files

Command used:

```bash
ls -l
```

This displayed:
- File names
- Permissions
- Ownership
- File size

---

## Files Created

![Files Created](./files-created.png)

---

# Task 2 – Reading Files

The next task focused on reading files using different Linux commands.

---

# Reading notes.txt

Command used:

```bash
cat notes.txt
```

The `cat` command displays complete file contents.

Useful for:
- Reading logs
- Viewing configurations
- Inspecting scripts

---

# Opening script.sh in Read-Only Mode

Command used:

```bash
vim -R script.sh
```

The `-R` option opens vim in read-only mode.

This prevents accidental modification of files.

---

# Displaying First 5 Lines

Command used:

```bash
head -n 5 /etc/passwd
```

This displayed the first 5 lines of the passwd file.

Useful when:
- Working with large files
- Reading logs
- Quickly inspecting configurations

---

# Displaying Last 5 Lines

Command used:

```bash
tail -n 5 /etc/passwd
```

This displayed the last 5 lines of the file.

The `tail` command is extremely useful for:
- Monitoring logs
- Viewing latest entries
- Debugging applications

---

## Reading Files

![Reading Files](./reading-files.png)

---

# Task 3 – Understanding Permissions

Next, I checked Linux file permissions.

Command used:

```bash
ls -l devops.txt notes.txt script.sh
```

Example output:

```text
-rw-rw-r-- notes.txt
```

---

# Understanding Permission Format

Linux permissions follow this format:

```text
rwxrwxrwx
```

Permission breakdown:
- First 3 → Owner
- Next 3 → Group
- Last 3 → Others

Permission meanings:
- `r` = read (4)
- `w` = write (2)
- `x` = execute (1)

---

# Common Permission Numbers

```text
755 = rwxr-xr-x
644 = rw-r--r--
640 = rw-r-----
```

This helped me understand how numeric permissions work internally.

---

## Permissions Before Changes

![Permissions Before](./permissions-before.png)

---

# Task 4 – Modifying Permissions

This was the most important part of today’s challenge.

---

# Making script.sh Executable

Command used:

```bash
chmod +x script.sh
```

Then executed the script:

```bash
./script.sh
```

The script ran successfully and displayed:

```text
Hello DevOps
```

This demonstrated how execute permissions control script execution.

---

# Making devops.txt Read-Only

Command used:

```bash
chmod -w devops.txt
```

This removed write permissions from the file.

Now the file became read-only.

---

# Setting notes.txt Permission to 640

Command used:

```bash
chmod 640 notes.txt
```

Meaning:
- Owner → read/write
- Group → read
- Others → no access

This helped me understand secure permission management.

---

# Creating project Directory with 755

Directory created:

```bash
mkdir project
```

Permission assigned:

```bash
chmod 755 project
```

Meaning:
- Owner → full access
- Group → read/execute
- Others → read/execute

This is one of the most common directory permission configurations in Linux.

---

# Verifying Permission Changes

Command used:

```bash
ls -l
```

This confirmed:
- Updated permissions
- Execute access
- Read-only restrictions

---

## Permissions After Changes

![Permissions After](./permissions-after.png)

---

# Task 5 – Testing Permissions

This task demonstrated what happens when permissions are incorrect.

---

# Trying to Write into Read-Only File

Command used:

```bash
echo "test" >> devops.txt
```

Result:

```text
Permission denied
```

This showed how Linux prevents unauthorized file modification.

---

# Removing Execute Permission

Command used:

```bash
chmod -x script.sh
```

Then tried running:

```bash
./script.sh
```

Result:

```text
Permission denied
```

This proved:
- Scripts require execute permission
- Linux blocks unauthorized execution

---

## Permission Errors

![Permission Errors](./permission-errors.png)

---

# Important Commands Practiced

## Create File

```bash
touch filename
```

---

## Write Content

```bash
echo "text" > file
```

---

## Append Content

```bash
echo "text" >> file
```

---

## Read File

```bash
cat filename
```

---

## First Lines

```bash
head -n 5 file
```

---

## Last Lines

```bash
tail -n 5 file
```

---

## Change Permissions

```bash
chmod permissions file
```

---

## Make File Executable

```bash
chmod +x script.sh
```

---

## Remove Write Permission

```bash
chmod -w file
```

---

## Check Permissions

```bash
ls -l
```

---

# Biggest Learning from Today

The biggest realization from today was:

> Linux permissions are one of the core foundations of system security.

Today helped me understand:
- How Linux controls file access
- Why execute permissions matter
- How scripts are protected
- Why wrong permissions break applications

This is extremely important in:
- DevOps
- Cloud servers
- Linux administration
- Production infrastructure

---

# Challenges Faced

## Permission Denied Errors

Initially, some operations failed because permissions were removed intentionally.

This helped me understand:
- Why Linux blocks unauthorized access
- How permission errors happen
- How chmod fixes access issues

---

# What I Learned

Today I learned:
- File creation
- File reading
- Vim basics
- Linux permission structure
- chmod usage
- Execute permissions
- Read-only restrictions
- Linux security fundamentals

---

# Final Thoughts

Today’s challenge felt very practical because file permissions are used everywhere in Linux systems.

I successfully practiced:
- Creating files
- Reading files
- Editing files
- Managing permissions
- Executing scripts
- Troubleshooting permission errors

And one thing became very clear:

> Good DevOps engineers must understand Linux permissions deeply because most infrastructure security depends on proper access control.

Day 10 completed.

Still learning. Still improving. Still building.