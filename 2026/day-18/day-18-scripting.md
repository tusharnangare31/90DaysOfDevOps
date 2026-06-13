# Day 18 of #90DaysOfDevOps – Shell Functions, Strict Mode & Building Reusable Scripts

## Introduction

As shell scripts become larger and more complex, writing everything in a single block quickly becomes difficult to manage.

Today's focus was on learning how to write cleaner and more maintainable Bash scripts using functions, local variables, and strict mode.

These concepts are widely used in production-grade scripts because they improve readability, reliability, and reusability.

The topics covered today were:

* Functions
* Function arguments
* Return values
* Local variables
* Strict mode (`set -euo pipefail`)
* Building a reusable system information script

This was an important step from writing simple scripts to creating structured automation.

---

# Why Functions Matter

Functions allow us to break large scripts into smaller reusable sections.

Instead of repeating the same code multiple times, we can define it once and call it whenever needed.

Benefits of functions:

* Cleaner code
* Easier maintenance
* Better readability
* Reusability
* Reduced duplication

Functions are one of the most important building blocks in shell scripting.

---

# Task 1 – Basic Functions

The first task was to create simple functions that accept arguments.

## functions.sh

```bash
#!/bin/bash

greet() {
    echo "Hello, $1!"
}

add() {
    echo "Sum: $(($1 + $2))"
}

greet "Sahil"
add 10 20
```

### Output

```text
Hello, Sahil!
Sum: 30
```

---

## What I Learned

Functions can accept arguments in the same way scripts accept command-line arguments.

Inside a function:

```bash
$1
$2
```

represent the first and second arguments passed to that function.

This makes functions flexible and reusable.

---

# Task 2 – System Resource Checks Using Functions

Instead of running commands manually every time, functions can organize system checks into reusable blocks.

## disk_check.sh

```bash
#!/bin/bash

check_disk() {
    echo "Disk Usage:"
    df -h /
}

check_memory() {
    echo "Memory Usage:"
    free -h
}

check_disk
echo
check_memory
```

### Example Output

```text
Disk Usage:
Filesystem      Size  Used Avail Use% Mounted on
/dev/root        20G  7.0G   13G  35% /

Memory Usage:
              total        used        free
Mem:          3.8Gi       1.2Gi       2.0Gi
```

---

## Why This is Useful

In DevOps, monitoring scripts often check:

* Disk space
* Memory usage
* CPU utilization
* Service health

Functions help keep these checks organized.

---

# Task 3 – Understanding Strict Mode

One of the most important Bash concepts I learned today was:

```bash
set -euo pipefail
```

Many production shell scripts use this setting because it helps prevent hidden failures.

---

## strict_demo.sh

```bash
#!/bin/bash

set -euo pipefail

echo "Strict mode enabled"
```

---

## Understanding Each Flag

### set -e

Exit immediately when a command fails.

Example:

```bash
false
```

Without `set -e`, the script continues.

With `set -e`, the script stops immediately.

---

### set -u

Treat undefined variables as errors.

Example:

```bash
echo "$UNDEFINED_VAR"
```

Without `set -u`, Bash prints nothing.

With `set -u`, the script exits with an error.

---

### set -o pipefail

Detect failures inside pipelines.

Example:

```bash
grep "hello" file.txt | cat
```

Without `pipefail`, Bash only checks the last command.

With `pipefail`, the entire pipeline fails if any command fails.

---

## Why Strict Mode Matters

Strict mode makes scripts:

* Safer
* More predictable
* Easier to debug

Most professional Bash scripts enable strict mode at the beginning.

---

# Task 4 – Local Variables

Functions can create variables that exist only inside the function.

This is done using the `local` keyword.

## local_demo.sh

```bash
#!/bin/bash

test_local() {
    local NAME="Sahil"
    echo "Inside function: $NAME"
}

test_global() {
    ROLE="DevOps Engineer"
}

test_local

echo "Outside function: ${NAME:-Not Available}"

test_global

echo "Global variable: $ROLE"
```

### Output

```text
Inside function: Sahil
Outside function: Not Available
Global variable: DevOps Engineer
```

---

## What I Learned

Local variables:

* Exist only inside functions
* Prevent accidental modification
* Improve script reliability

This concept is very similar to variable scope in programming languages.

---

# Task 5 – Building a System Information Reporter

The final challenge combined everything learned so far.

The goal was to create a reusable script using multiple functions.

---

## system_info.sh

```bash
#!/bin/bash

set -euo pipefail

hostname_info() {
    echo "===== HOSTNAME & OS ====="
    hostname
    uname -a
}

uptime_info() {
    echo
    echo "===== UPTIME ====="
    uptime
}

disk_info() {
    echo
    echo "===== DISK USAGE ====="
    df -h | head -5
}

memory_info() {
    echo
    echo "===== MEMORY USAGE ====="
    free -h
}

cpu_info() {
    echo
    echo "===== TOP CPU PROCESSES ====="
    ps aux --sort=-%cpu | head -6
}

main() {
    hostname_info
    uptime_info
    disk_info
    memory_info
    cpu_info
}

main
```

---

### Example Output

```text
===== HOSTNAME & OS =====
ip-172-31-35-193

===== UPTIME =====
up 2 days, 4 hours

===== DISK USAGE =====
Filesystem      Size  Used Avail Use%

===== MEMORY USAGE =====
Memory statistics...

===== TOP CPU PROCESSES =====
Top processes...
```

---

# Real-World DevOps Applications

The concepts learned today are heavily used in:

### Monitoring Scripts

Checking:

* CPU usage
* Memory usage
* Disk space
* Service status

---

### Deployment Scripts

Functions help separate deployment steps into reusable modules.

---

### Automation Workflows

Functions make automation scripts:

* Easier to maintain
* Easier to debug
* Easier to scale

---

# Key Learnings

## 1. Functions Make Scripts Reusable

Functions allow code to be written once and reused multiple times.

---

## 2. Strict Mode Makes Scripts Safer

Using:

```bash
set -euo pipefail
```

helps detect failures early and prevents unexpected behavior.

---

## 3. Local Variables Improve Reliability

Using:

```bash
local VARIABLE=value
```

prevents variables from leaking outside functions.

---

# Key Takeaways

* Functions improve readability and maintainability.
* Arguments can be passed directly to functions.
* Strict mode helps catch errors early.
* Local variables prevent unintended side effects.
* Structured scripts are easier to manage.
* Functions are a fundamental building block for DevOps automation.

---

# Final Thoughts

Today's challenge felt like a shift from basic shell scripting to writing more structured and professional scripts.

Instead of creating small standalone scripts, I learned how to organize logic into reusable functions, handle failures safely, and build scripts that are easier to maintain.

The System Information Reporter was a great exercise because it combined multiple concepts into a single practical tool.

As scripts become larger, these techniques become essential for keeping automation reliable and manageable.

Day 18 completed.

Still learning. Still improving. Still building.
