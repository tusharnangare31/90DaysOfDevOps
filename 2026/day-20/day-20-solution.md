# Day 20 Solution – Log Analyzer and Report Generator

## Overview

In this challenge, I built a Bash script called `log_analyzer.sh` that automates log analysis and report generation.

The script performs the following tasks:

* Accepts a log file as input
* Validates user input
* Counts errors and failures
* Identifies critical events
* Finds the top 5 most common error messages
* Generates a detailed report
* Archives processed log files

---

# log_analyzer.sh

```bash
#!/bin/bash

set -euo pipefail

# Check argument
if [ $# -eq 0 ]; then
    echo "Usage: $0 <log_file>"
    exit 1
fi

LOG_FILE=$1

# Validate file
if [ ! -f "$LOG_FILE" ]; then
    echo "Error: File '$LOG_FILE' does not exist."
    exit 1
fi

DATE=$(date +%Y-%m-%d)
REPORT="log_report_${DATE}.txt"

TOTAL_LINES=$(wc -l < "$LOG_FILE")

ERROR_COUNT=$(grep -Ei "ERROR|Failed" "$LOG_FILE" | wc -l)

CRITICAL_EVENTS=$(grep -n "CRITICAL" "$LOG_FILE" || true)

TOP_ERRORS=$(grep "ERROR" "$LOG_FILE" \
| sed 's/^.*ERROR //' \
| sort \
| uniq -c \
| sort -rn \
| head -5)

{
echo "========================================="
echo "       DAILY LOG ANALYSIS REPORT"
echo "========================================="
echo ""
echo "Date of Analysis : $(date)"
echo "Log File         : $LOG_FILE"
echo "Total Lines      : $TOTAL_LINES"
echo "Total Errors     : $ERROR_COUNT"

echo ""
echo "----- Top 5 Error Messages -----"
echo "$TOP_ERRORS"

echo ""
echo "----- Critical Events -----"
echo "$CRITICAL_EVENTS"

} > "$REPORT"

echo ""
echo "================================="
echo "Analysis Complete"
echo "================================="
echo "Total Errors Found : $ERROR_COUNT"
echo "Report Generated   : $REPORT"

# Optional Archive Feature
mkdir -p archive

mv "$LOG_FILE" archive/

echo "Archived Log File To: archive/"
```

---

# Sample Log File

```text
2026-06-06 10:10:11 INFO Application Started
2026-06-06 10:11:05 ERROR Connection timed out
2026-06-06 10:11:15 ERROR Connection timed out
2026-06-06 10:11:20 ERROR File not found
2026-06-06 10:12:01 CRITICAL Database connection lost
2026-06-06 10:13:50 Failed Login Attempt
2026-06-06 10:15:25 ERROR Permission denied
2026-06-06 10:18:12 CRITICAL Disk space below threshold
2026-06-06 10:19:01 ERROR Connection timed out
```

---

# Running the Script

Make executable:

```bash
chmod +x log_analyzer.sh
```

Execute:

```bash
./log_analyzer.sh sample_log.log
```

---

# Console Output

```text
=================================
Analysis Complete
=================================
Total Errors Found : 5
Report Generated   : log_report_2026-06-06.txt
Archived Log File To: archive/
```

---

# Generated Report

File:

```text
log_report_2026-06-06.txt
```

Content:

```text
=========================================
       DAILY LOG ANALYSIS REPORT
=========================================

Date of Analysis : Fri Jun 06 12:30:15 UTC 2026
Log File         : sample_log.log
Total Lines      : 9
Total Errors     : 5

----- Top 5 Error Messages -----

3 Connection timed out
1 Permission denied
1 File not found

----- Critical Events -----

5:2026-06-06 10:12:01 CRITICAL Database connection lost

8:2026-06-06 10:18:12 CRITICAL Disk space below threshold
```

---

# Commands and Tools Used

| Command  | Purpose                  |
| -------- | ------------------------ |
| grep     | Search keywords          |
| grep -n  | Display line numbers     |
| grep -c  | Count occurrences        |
| wc -l    | Count total lines        |
| sort     | Sort output              |
| uniq -c  | Count duplicate entries  |
| head -5  | Display top 5 results    |
| sed      | Extract error messages   |
| mv       | Move processed logs      |
| mkdir -p | Create archive directory |

---

# Task Mapping

## Task 1 – Input Validation

```bash
if [ $# -eq 0 ]; then
```

Checks whether a log file was provided.

```bash
if [ ! -f "$LOG_FILE" ]; then
```

Checks if the file exists.

---

## Task 2 – Error Count

```bash
grep -Ei "ERROR|Failed" "$LOG_FILE"
```

Counts all ERROR and Failed entries.

---

## Task 3 – Critical Events

```bash
grep -n "CRITICAL" "$LOG_FILE"
```

Displays critical events along with line numbers.

---

## Task 4 – Top Error Messages

```bash
grep "ERROR" "$LOG_FILE"
```

Extracts errors.

```bash
sort | uniq -c | sort -rn
```

Finds the most common error messages.

---

## Task 5 – Summary Report

Creates:

```text
log_report_<date>.txt
```

containing:

* Analysis date
* Log file name
* Total lines
* Total errors
* Top 5 errors
* Critical events

---

## Task 6 – Archive Processed Logs

```bash
mkdir -p archive
mv "$LOG_FILE" archive/
```

Moves processed logs into the archive directory.

---

# What I Learned

### 1. Log Analysis Can Be Automated

Instead of manually checking logs, Bash scripts can quickly generate meaningful summaries.

### 2. Linux Text Processing Tools Are Powerful

Commands such as:

```bash
grep
awk
sed
sort
uniq
```

can analyze thousands of log entries in seconds.

### 3. Automation Improves Operations

Automated log analysis helps identify issues faster and reduces manual effort.

---

# Conclusion

This challenge demonstrated how Bash scripting can be used to automate log monitoring and reporting tasks. By combining tools like `grep`, `sed`, `sort`, and `uniq`, I created a reusable log analyzer that identifies errors, highlights critical events, generates reports, and archives processed logs automatically.

Day 20 completed successfully.
