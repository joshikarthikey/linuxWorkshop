---
title: "Shell Scripting & Process Management"
author: "GDG Workshop - Aadesh"
theme:
  name: gruvbox-dark
  override:
    footer:
      style: template
      left:
        image: abc.png
      right: "{current_slide} / {total_slides}"
      height: 5
options:
  implicit_slide_ends: false
---

# Shell Scripting & Process Management 🚀

**Master the Terminal**

> "The shell is your steering wheel to the system."

---

**Today's Journey:**

1. 🔧 Shell Scripting Basics
2. 📝 Variables & User Input
3. 🔄 Control Flow (Loops & Conditions)
4. ⚙️ Process Management
5. 🎯 Real-World Practice

<!-- end_slide -->

# Part 1: Shell Scripting

## What is Shell Scripting?

**Definition:** A shell script is a text file containing a series of commands that the shell can execute.

**Why use scripts?**
* Automate repetitive tasks
* Ensure consistency
* Save time and reduce errors
* Chain multiple commands together

<!-- end_slide -->

## The Shebang Line

**Every script starts with a "shebang" (`#!`)**

```bash
#!/bin/bash
```

**What does it do?**
* Tells the system which interpreter to use
* Must be the very first line
* No spaces before `#!`

**Common Interpreters:**
* `#!/bin/bash` - Bash shell (most common)
* `#!/bin/sh` - POSIX shell (portable)
* `#!/usr/bin/python3` - Python scripts
* `#!/usr/bin/env bash` - Finds bash in PATH

<!-- end_slide -->

## Your First Script

**Let's create a simple "Hello World" script**

```bash +exec
#!/bin/bash

# This is a comment - lines starting with # are ignored
echo "Hello, Workshop Participants!"
echo "Welcome to Shell Scripting"

# Display current date and time
date

# Show who is running this script
echo "Script executed by: $(whoami)"
```

**Key Commands:**
* `echo` - Print text to screen
* `date` - Show current date/time
* `whoami` - Display current username

<!-- end_slide -->

## Making Scripts Executable

**Two ways to run scripts:**

**Method 1: Using bash command**
```bash
bash my_script.sh
```

**Method 2: Make it executable**
```bash
# Give execute permission
chmod +x my_script.sh

# Run it directly
./my_script.sh
```

**Why `./`?**
* Tells shell to look in current directory
* Security feature - prevents running malicious scripts from PATH

<!-- end_slide -->

# Part 2: Variables

## Working with Variables

**Variables store data for later use**

```bash +exec
#!/bin/bash

# Creating variables (NO SPACES around =)
NAME="Linux Workshop"
COUNT=42
DATE=$(date +%Y-%m-%d)

# Using variables (prefix with $)
echo "Event: $NAME"
echo "Participants: $COUNT"
echo "Date: $DATE"

# Arithmetic operations
(( COUNT = COUNT + 8 ))
echo "New count: $COUNT"
```

**Rules:**
* No spaces around `=` sign
* Access with `$` prefix
* Use `$(command)` for command substitution
* Use `$(( ))` for arithmetic

<!-- end_slide -->

## User Input

**Make scripts interactive with `read` command**

```bash
#!/bin/bash

# Ask for user's name
echo "What is your name?"
read USERNAME

# Ask for favorite language
echo "What's your favorite programming language?"
read LANGUAGE

# Display personalized message
echo ""
echo "Hello, $USERNAME!"
echo "Great choice! $LANGUAGE is awesome."
echo "Let's continue learning!"
```

**Note:** Run this in your terminal, not in the presentation.
Type: `nano input_demo.sh` → paste code → save → `chmod +x input_demo.sh` → `./input_demo.sh`

<!-- end_slide -->

## Command-Line Arguments

**Pass data when running script**

```bash +exec
#!/bin/bash

# Special variables for arguments
# $0 = script name
# $1 = first argument
# $2 = second argument
# $# = total number of arguments
# $@ = all arguments

echo "Script name: $0"
echo "Total arguments: $#"

# Simulate arguments for demo
ARG1="DevOps"
ARG2="2024"

echo ""
echo "First argument: $ARG1"
echo "Second argument: $ARG2"
```

**Usage Example:**
```bash
./my_script.sh DevOps 2024
```

<!-- end_slide -->

# Part 3: Control Flow

## Conditional Statements (if-else)

**Make decisions in your scripts**

```bash +exec
#!/bin/bash

# Check if a file exists
FILE="test.txt"

# Create the file for demo
touch "$FILE"

if [ -f "$FILE" ]; then
    echo "✓ File '$FILE' exists!"
    echo "File size: $(wc -c < $FILE) bytes"
else
    echo "✗ File '$FILE' not found!"
fi

# Clean up
rm -f "$FILE"
```

**Common Test Operators:**
* `-f file` → file exists
* `-d dir` → directory exists
* `-z string` → string is empty
* `$A -eq $B` → numbers are equal
* `$A -lt $B` → A is less than B

<!-- end_slide -->

## More Conditional Examples

**Checking numbers and strings**

```bash +exec
#!/bin/bash

AGE=25

if [ $AGE -ge 18 ]; then
    echo "Age $AGE: Adult ✓"
else
    echo "Age $AGE: Minor"
fi

# String comparison
OS="Linux"

if [ "$OS" = "Linux" ]; then
    echo "Running on $OS - Perfect! 🐧"
elif [ "$OS" = "Windows" ]; then
    echo "Running on $OS"
else
    echo "Unknown OS: $OS"
fi
```

**Operators:**
* `-eq` → equal (numbers)
* `-ne` → not equal
* `-gt` → greater than
* `-ge` → greater or equal
* `=` → equal (strings)
* `!=` → not equal (strings)

<!-- end_slide -->

## For Loops

**Repeat actions multiple times**

```bash +exec
#!/bin/bash

echo "=== Example 1: Simple Range ==="
for i in {1..5}
do
    echo "  Iteration $i"
done

echo ""
echo "=== Example 2: File Operations ==="
# Create demo files
for num in {1..3}
do
    FILENAME="file_$num.txt"
    echo "Processing $FILENAME"
    echo "Content of file $num" > "$FILENAME"
done

# List them
echo ""
echo "Created files:"
ls file_*.txt

# Clean up
rm -f file_*.txt
```

<!-- end_slide -->

## While Loops

**Loop until condition is false**

```bash +exec
#!/bin/bash

echo "Countdown Timer:"

COUNT=5
while [ $COUNT -gt 0 ]
do
    echo "  $COUNT seconds remaining..."
    (( COUNT-- ))
    sleep 1
done

echo "  🎉 Time's up!"
```

**When to use while:**
* Unknown number of iterations
* Waiting for a condition
* Reading files line-by-line

<!-- end_slide -->

## Practical Example: Backup Script

**Let's create a real-world backup script**

```bash +exec
#!/bin/bash

# Backup script for workshop files

# Configuration
SOURCE_DIR="workshop_files"
BACKUP_DIR="backups"
TIMESTAMP=$(date +%Y%m%d_%H%M%S)
BACKUP_NAME="backup_$TIMESTAMP"

echo "=== Backup Script Started ==="
echo ""

# Create source directory with sample files
mkdir -p "$SOURCE_DIR"
for i in {1..3}; do
    echo "Important data $i" > "$SOURCE_DIR/file$i.txt"
done

# Create backup directory
mkdir -p "$BACKUP_DIR"

# Perform backup
echo "Backing up: $SOURCE_DIR"
echo "Destination: $BACKUP_DIR/$BACKUP_NAME"
cp -r "$SOURCE_DIR" "$BACKUP_DIR/$BACKUP_NAME"

# Verify backup
if [ -d "$BACKUP_DIR/$BACKUP_NAME" ]; then
    FILE_COUNT=$(ls "$BACKUP_DIR/$BACKUP_NAME" | wc -l)
    echo ""
    echo "✓ Backup completed successfully!"
    echo "  Files backed up: $FILE_COUNT"
else
    echo "✗ Backup failed!"
fi

# Cleanup
rm -rf "$SOURCE_DIR" "$BACKUP_DIR"
```

<!-- end_slide -->

# Part 4: Process Management

## What is a Process?

**Process:** A running instance of a program

**Every process has:**
* **PID (Process ID):** Unique identifier
* **PPID (Parent Process ID):** Who started it
* **State:** Running, Sleeping, Stopped, Zombie
* **Resources:** CPU time, Memory usage

**View your shell's PID:**
```bash +exec
echo "My shell's PID: $$"
echo "My parent's PID: $PPID"
```

<!-- end_slide -->

## Viewing Processes

**Essential commands to monitor processes**

```bash +exec
#!/bin/bash

echo "=== Top 5 Processes by Memory ==="
ps aux --sort=-%mem | head -n 6

echo ""
echo "=== My Running Processes ==="
ps -u $(whoami) -o pid,cmd | head -n 5
```

**Key Commands:**
* `ps aux` → All processes (snapshot)
* `ps -ef` → Full-format listing
* `pgrep name` → Find PID by name
* `top` / `htop` → Real-time monitoring

<!-- end_slide -->

## Background vs Foreground

**Foreground:** Process occupies your terminal

**Background:** Process runs independently

```bash
# Run in foreground (blocks terminal)
sleep 10

# Run in background (returns control)
sleep 10 &

# List background jobs
jobs

# Bring to foreground
fg %1
```

**Live Demo Required:** Try this in your terminal!

<!-- end_slide -->

## Process Control Signals

**Keyboard Shortcuts:**

| Key | Signal | Action |
|-----|--------|--------|
| `Ctrl+C` | SIGINT | Interrupt (kill gracefully) |
| `Ctrl+Z` | SIGTSTP | Suspend (pause) |
| `Ctrl+D` | EOF | End of input |

**Commands:**
* `bg` → Resume suspended process in background
* `fg` → Bring background process to foreground
* `kill PID` → Terminate process
* `kill -9 PID` → Force kill (SIGKILL)

<!-- end_slide -->

## Hands-On: Process Control

**Let's practice process control**

**Step 1:** Open a terminal and run:
```bash
sleep 100
```

**Step 2:** While running, press `Ctrl+Z`
* Process is now suspended

**Step 3:** Type and run:
```bash
bg
```
* Process now runs in background

**Step 4:** Check it's running:
```bash
jobs
```

**Step 5:** Kill it:
```bash
kill %1
```

<!-- end_slide -->

## The kill Command

**Different ways to terminate processes**

```bash +exec
#!/bin/bash

# Start a background process
sleep 30 &
SLEEP_PID=$!

echo "Started sleep with PID: $SLEEP_PID"
echo ""

# Show it's running
echo "Process info:"
ps -p $SLEEP_PID -o pid,cmd,stat

# Kill it gracefully
echo ""
echo "Sending SIGTERM..."
kill $SLEEP_PID

# Wait a moment
sleep 1

# Verify it's gone
if ps -p $SLEEP_PID > /dev/null 2>&1; then
    echo "Process still running (shouldn't happen)"
else
    echo "✓ Process terminated successfully"
fi
```

<!-- end_slide -->

## Signal Types

**Common signals you should know:**

| Signal | Number | Description | Usage |
|--------|--------|-------------|-------|
| SIGTERM | 15 | Graceful termination | `kill PID` |
| SIGKILL | 9 | Force kill | `kill -9 PID` |
| SIGINT | 2 | Interrupt (Ctrl+C) | `kill -2 PID` |
| SIGHUP | 1 | Hang up | `kill -1 PID` |
| SIGSTOP | 19 | Stop/pause | `kill -19 PID` |
| SIGCONT | 18 | Continue | `kill -18 PID` |

**Best Practice:** Always try SIGTERM first, use SIGKILL only if necessary.

<!-- end_slide -->

## Process Priority (nice)

**Control CPU priority of processes**

```bash
# Start with low priority (nice value 10)
nice -n 10 ./my_heavy_script.sh &

# Change priority of running process
renice -n 5 -p PID
```

**Nice Values:**
* Range: -20 (highest priority) to 19 (lowest)
* Default: 0
* Only root can set negative values
* Higher nice value = "nicer" to other processes

<!-- end_slide -->

## Exit Status

**Every command returns an exit code**

```bash +exec
#!/bin/bash

# Successful command
ls /tmp > /dev/null 2>&1
echo "ls exit status: $?"

# Failed command
ls /nonexistent_directory > /dev/null 2>&1
echo "failed ls exit status: $?"

# Custom exit status
function my_function() {
    if [ "$1" = "success" ]; then
        return 0
    else
        return 1
    fi
}

my_function success
echo "Function with success: $?"

my_function fail
echo "Function with fail: $?"
```

**Convention:** 0 = success, non-zero = error

<!-- end_slide -->

## Job Control Demo Script

**Complete example showing all concepts**

```bash +exec
#!/bin/bash

echo "=== Job Control Demonstration ==="
echo ""

# Start multiple background jobs
echo "Starting 3 background jobs..."
sleep 5 &
JOB1=$!
sleep 10 &
JOB2=$!
sleep 15 &
JOB3=$!

echo "  Job 1 (PID $JOB1): sleep 5"
echo "  Job 2 (PID $JOB2): sleep 10"
echo "  Job 3 (PID $JOB3): sleep 15"
echo ""

# Show job status
echo "Current jobs:"
jobs
echo ""

# Wait for first job
echo "Waiting for Job 1 to complete..."
wait $JOB1
echo "  ✓ Job 1 finished!"
echo ""

# Kill remaining jobs
echo "Terminating remaining jobs..."
kill $JOB2 $JOB3
wait $JOB2 $JOB3 2>/dev/null

echo "  ✓ All jobs cleaned up!"
echo ""
echo "=== Demo Complete ==="
```

<!-- end_slide -->

# Part 5: Practical Examples

## System Information Script

**Create a system monitoring script**

```bash +exec
#!/bin/bash

echo "╔════════════════════════════════════════════╗"
echo "║      SYSTEM INFORMATION REPORT             ║"
echo "╚════════════════════════════════════════════╝"
echo ""

# System Info
echo "📋 System Information:"
echo "  Hostname: $(hostname)"
echo "  Kernel: $(uname -r)"
echo "  OS: $(cat /etc/os-release | grep PRETTY_NAME | cut -d'"' -f2)"
echo ""

# Current User
echo "👤 User Information:"
echo "  Username: $(whoami)"
echo "  User ID: $(id -u)"
echo "  Home: $HOME"
echo ""

# Time
echo "🕐 Time Information:"
echo "  Date: $(date '+%Y-%m-%d')"
echo "  Time: $(date '+%H:%M:%S')"
echo "  Uptime: $(uptime -p)"
echo ""

# Disk Usage
echo "💾 Disk Usage (/):"
df -h / | tail -n 1 | awk '{print "  Used: "$3" / "$2" ("$5")"}'
echo ""

# Process Count
PROC_COUNT=$(ps aux | wc -l)
echo "⚙️  Running Processes: $PROC_COUNT"
echo ""

echo "Report generated at: $(date)"
```

<!-- end_slide -->

## Log File Analyzer

**Parse and analyze log files**

```bash +exec
#!/bin/bash

LOG_FILE="system.log"

# Create sample log file
cat > "$LOG_FILE" << EOF
2024-01-29 10:00:00 INFO Server started
2024-01-29 10:05:23 WARNING High memory usage detected
2024-01-29 10:10:45 ERROR Connection timeout
2024-01-29 10:15:12 INFO User login: admin
2024-01-29 10:20:33 ERROR Database connection failed
2024-01-29 10:25:01 WARNING Disk space low
2024-01-29 10:30:00 INFO Backup completed
EOF

echo "=== Log File Analysis ==="
echo ""

# Count different log levels
ERROR_COUNT=$(grep -c "ERROR" "$LOG_FILE")
WARNING_COUNT=$(grep -c "WARNING" "$LOG_FILE")
INFO_COUNT=$(grep -c "INFO" "$LOG_FILE")

echo "Summary:"
echo "  Total lines: $(wc -l < $LOG_FILE)"
echo "  Errors: $ERROR_COUNT"
echo "  Warnings: $WARNING_COUNT"
echo "  Info: $INFO_COUNT"
echo ""

# Show all errors
echo "Error Messages:"
grep "ERROR" "$LOG_FILE" | while read line; do
    echo "  • $line"
done

# Clean up
rm -f "$LOG_FILE"
```

<!-- end_slide -->

## Process Monitor Script

**Monitor specific processes**

```bash +exec
#!/bin/bash

# Function to monitor process
monitor_process() {
    PROCESS_NAME=$1
    
    echo "Monitoring: $PROCESS_NAME"
    echo "================================"
    
    # Check if process exists
    if pgrep -x "$PROCESS_NAME" > /dev/null; then
        echo "✓ Process is running"
        
        # Get process details
        PID=$(pgrep -x "$PROCESS_NAME" | head -n 1)
        echo "  PID: $PID"
        
        # Memory usage
        MEM=$(ps -p $PID -o %mem= | xargs)
        echo "  Memory: ${MEM}%"
        
        # CPU usage
        CPU=$(ps -p $PID -o %cpu= | xargs)
        echo "  CPU: ${CPU}%"
    else
        echo "✗ Process not found"
    fi
    echo ""
}

# Demo with bash (always running)
monitor_process "bash"

# Demo with non-existent process
monitor_process "nonexistent_app"
```

<!-- end_slide -->

## Automated Cleanup Script

**Clean temporary files automatically**

```bash +exec
#!/bin/bash

CLEANUP_DIR="temp_files"
DAYS_OLD=7

echo "=== Automated Cleanup Script ==="
echo ""

# Create demo directory with old files
mkdir -p "$CLEANUP_DIR"
echo "Creating demo files..."
for i in {1..5}; do
    FILE="$CLEANUP_DIR/temp_file_$i.txt"
    echo "Temporary data $i" > "$FILE"
    # Make some files appear old
    if [ $i -le 3 ]; then
        touch -d "10 days ago" "$FILE"
    fi
done

echo "Files before cleanup:"
ls -lh "$CLEANUP_DIR"
echo ""

# Find and delete old files
echo "Searching for files older than $DAYS_OLD days..."
OLD_FILES=$(find "$CLEANUP_DIR" -type f -mtime +$DAYS_OLD)

if [ -n "$OLD_FILES" ]; then
    echo "Found old files:"
    echo "$OLD_FILES" | while read file; do
        echo "  • $file"
    done
    
    echo ""
    echo "Deleting old files..."
    find "$CLEANUP_DIR" -type f -mtime +$DAYS_OLD -delete
    
    DELETED_COUNT=$(echo "$OLD_FILES" | wc -l)
    echo "✓ Deleted $DELETED_COUNT file(s)"
else
    echo "No old files found"
fi

echo ""
echo "Files after cleanup:"
ls -lh "$CLEANUP_DIR"

# Clean up demo directory
rm -rf "$CLEANUP_DIR"
```

<!-- end_slide -->

# Workshop Challenge 🏆

## Challenge: System Monitor Dashboard

**Create a script named `monitor.sh` that:**

1. Displays system hostname and current date/time
2. Shows current user and their home directory
3. Lists the top 3 processes by CPU usage
4. Shows disk usage of the root filesystem
5. Counts total running processes
6. Saves all output to `system_report_$(date +%Y%m%d).txt`

**Bonus Points:**
* Add color output using ANSI codes
* Accept command-line argument for output filename
* Create a loop that runs every 10 seconds

**Time: 15 minutes**

<!-- end_slide -->

## Challenge Solution Template

```bash
#!/bin/bash

# System Monitor Dashboard
# Your name here

OUTPUT_FILE="system_report_$(date +%Y%m%d).txt"

# Header
echo "==================================" | tee "$OUTPUT_FILE"
echo "  SYSTEM MONITOR DASHBOARD" | tee -a "$OUTPUT_FILE"
echo "==================================" | tee -a "$OUTPUT_FILE"
echo "" | tee -a "$OUTPUT_FILE"

# 1. System Info
echo "🖥️  Hostname: $(hostname)" | tee -a "$OUTPUT_FILE"
# TODO: Add more info

# 2. User Info
# TODO: Implement

# 3. Top Processes
echo "" | tee -a "$OUTPUT_FILE"
echo "Top 3 Processes by CPU:" | tee -a "$OUTPUT_FILE"
# TODO: Use ps command

# 4. Disk Usage
# TODO: Use df command

# 5. Process Count
# TODO: Count processes

echo "" | tee -a "$OUTPUT_FILE"
echo "Report saved to: $OUTPUT_FILE"
```

<!-- end_slide -->

## Best Practices Summary

**Writing Better Shell Scripts:**

1. **Always use shebang** (`#!/bin/bash`)
2. **Add comments** to explain complex logic
3. **Use meaningful variable names**
   * Good: `BACKUP_DIR`, `USER_NAME`
   * Bad: `x`, `tmp`, `data`
4. **Quote variables** (`"$VAR"` not `$VAR`)
5. **Check for errors**
   * Use `set -e` to exit on errors
   * Check exit status: `if [ $? -ne 0 ]; then`
6. **Make scripts modular** with functions
7. **Add usage instructions** (`--help` option)

<!-- end_slide -->

## Common Pitfalls to Avoid

**Mistakes beginners make:**

❌ **Spaces around `=` in assignments**
```bash
NAME = "John"  # WRONG
NAME="John"    # CORRECT
```

❌ **Forgetting quotes**
```bash
if [ $VAR = "test" ]    # Breaks if VAR is empty
if [ "$VAR" = "test" ]  # CORRECT
```

❌ **Using `==` in `[ ]`**
```bash
if [ $A == $B ]   # WRONG (Bash-specific)
if [ "$A" = "$B" ]  # CORRECT (POSIX-compliant)
```

❌ **Not checking if file exists**
```bash
cat file.txt  # Fails if file doesn't exist
if [ -f file.txt ]; then cat file.txt; fi  # CORRECT
```

<!-- end_slide -->

## Debugging Scripts

**Tools and techniques:**

**1. Enable debug mode**
```bash
#!/bin/bash -x
# OR
set -x  # Turn on
set +x  # Turn off
```

**2. Check syntax without running**
```bash
bash -n script.sh
```

**3. Use echo statements**
```bash
echo "DEBUG: Variable VALUE = $VALUE"
```

**4. Trap errors**
```bash
trap 'echo "Error at line $LINENO"' ERR
```

**5. Use ShellCheck** (online tool)
* Visit shellcheck.net
* Paste your script
* Get suggestions

<!-- end_slide -->

## Advanced Topics to Explore

**Next steps in your shell scripting journey:**

1. **Functions and Modularity**
   * Create reusable code blocks
   * Pass parameters to functions

2. **Regular Expressions (regex)**
   * Pattern matching with `grep`, `sed`, `awk`

3. **File Descriptors and Redirection**
   * `>`, `>>`, `2>`, `&>`, `|`, `tee`

4. **Command Substitution**
   * `$()` vs backticks

5. **Array Manipulation**
   * Indexed and associative arrays

6. **Signal Handling with trap**

7. **Parallel Processing**
   * `xargs -P`, GNU parallel

<!-- end_slide -->

## Bonus: Quick Reference Card

**Essential Commands:**

| Category | Commands |
|----------|----------|
| **Variables** | `VAR="value"`, `$VAR`, `${VAR}` |
| **Conditionals** | `if [ ]; then; fi`, `[[ ]]`, `-f`, `-d` |
| **Loops** | `for i in; do; done`, `while [ ]; do` |
| **Process** | `ps`, `top`, `kill`, `jobs`, `bg`, `fg` |
| **I/O** | `read`, `echo`, `printf`, `>`, `>>` |
| **Exit** | `exit 0`, `$?` |

**Keyboard Shortcuts:**
* `Ctrl+C` - Interrupt
* `Ctrl+Z` - Suspend
* `Ctrl+D` - EOF
* `Ctrl+L` - Clear screen