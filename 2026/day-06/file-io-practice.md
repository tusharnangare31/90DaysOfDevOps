# Day 06 – Linux Fundamentals: Read and Write Text Files

## Introduction

Today’s focus was on practicing one of the most basic but important Linux skills:

> Reading and writing text files from the terminal.

In Linux, almost everything is managed using text files:
- Logs
- Configuration files
- Scripts
- Environment variables
- Application settings

That is why understanding file input and output operations is extremely important for every DevOps engineer.

Today’s task was simple:
- Create a text file
- Write content into it
- Append new content
- Read the file using different commands

Even though the commands were basic, this practice helped me better understand how Linux handles file operations.

---

# Creating a File

The first step was creating a file named:

```bash
notes.txt
```

Command used:

```bash
touch notes.txt
```

### Observation

The `touch` command created an empty file successfully.

This command is commonly used in Linux to:
- Create empty files
- Update file timestamps

---

# Writing Text into File

I used output redirection (`>`) to write content into the file.

Command used:

```bash
echo "Learning Linux fundamentals" > notes.txt
```

### Observation

The text was successfully written into the file.

The `>` operator:
- Creates file if it does not exist
- Overwrites existing content

This is important because accidental overwriting can remove old data.

---

# Appending New Lines

Next, I added more content using append redirection (`>>`).

Commands used:

```bash
echo "Practicing file operations" >> notes.txt
```

```bash
echo "DevOps requires Linux skills" >> notes.txt
```

### Observation

The new lines were added without removing previous content.

The `>>` operator:
- Appends content
- Preserves existing data

Very useful for:
- Writing logs
- Updating files
- Appending script outputs

---

# Using tee Command

I also practiced the `tee` command.

Command used:

```bash
echo "Linux commands improve troubleshooting" | tee -a notes.txt
```

### Observation

This command:
- Displayed output on terminal
- Simultaneously appended output into file

The `tee` command is extremely useful during:
- Logging
- Scripting
- Output monitoring

---

# Reading Full File Content

To read the file completely, I used:

```bash
cat notes.txt
```

### Output

```bash
Learning Linux fundamentals
Practicing file operations
DevOps requires Linux skills
Linux commands improve troubleshooting
```

### Observation

The `cat` command displayed the complete file content.

This command is heavily used in Linux for:
- Reading configuration files
- Viewing logs
- Checking script content

---

# Reading First Few Lines

To read only the first lines of the file, I used:

```bash
head -n 2 notes.txt
```

### Output

```bash
Learning Linux fundamentals
Practicing file operations
```

### Observation

The `head` command is useful when:
- Checking beginning of logs
- Inspecting file headers
- Reading partial content quickly

---

# Reading Last Few Lines

To read the last lines, I used:

```bash
tail -n 2 notes.txt
```

### Output

```bash
DevOps requires Linux skills
Linux commands improve troubleshooting
```

### Observation

The `tail` command is commonly used in DevOps for:
- Monitoring logs
- Tracking application activity
- Debugging issues

Especially:

```bash
tail -f logfile
```

is extremely useful for real-time log monitoring.

---

# Checking Disk Space Dynamically

I also practiced command substitution.

Command used:

```bash
echo "disk space is $(df -h / | awk 'NR==2 {print $4}')"
```

### Output

```bash
disk space is 54G
```

### Observation

This command helped me understand:
- Command substitution using `$()`
- Combining Linux commands
- Parsing command output using `awk`

This is extremely useful in shell scripting and automation.

---

# Final Content of notes.txt

```text
Learning Linux fundamentals
Practicing file operations
DevOps requires Linux skills
Linux commands improve troubleshooting
```

---

# Important Learning from Today

Today’s practice looked simple, but it helped me understand something important:

> Linux automation starts with file handling.

Most DevOps work involves:
- Editing config files
- Reading logs
- Writing scripts
- Redirecting outputs
- Automating tasks

These small commands form the foundation for:
- Shell scripting
- CI/CD automation
- Monitoring systems
- Infrastructure management

---

# Commands Practiced Today

## Create File

```bash
touch notes.txt
```

---

## Write into File

```bash
echo "text" > notes.txt
```

---

## Append into File

```bash
echo "text" >> notes.txt
```

---

## Write and Display Together

```bash
echo "text" | tee -a notes.txt
```

---

## Read Full File

```bash
cat notes.txt
```

---

## Read First Lines

```bash
head -n 2 notes.txt
```

---

## Read Last Lines

```bash
tail -n 2 notes.txt
```

---

## Command Substitution

```bash
echo "disk space is $(df -h / | awk 'NR==2 {print $4}')"
```

---

# Final Thoughts

Today’s learning was simple but very practical.

I practiced:
- File creation
- Writing text
- Appending content
- Reading files
- Output redirection
- Command substitution

These are very basic Linux operations, but they are used daily in DevOps and system administration.

The more I practice these small Linux fundamentals, the stronger my command-line confidence becomes.

Day 06 completed.

Still learning. Still improving. Still building.