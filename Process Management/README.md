# Process Management

As I work more in Linux, I quickly realize that everything running on the system is a process. Managing processes lets me see what’s running, how much resources are being used, and how to stop or control programs when something goes wrong. In this section, I break down how processes work and how to manage them using common Linux tools.

---

## What Is a Process?

A process is an instance of a running program.  
Every command I run creates a process, and Linux assigns it a unique identifier called a PID (Process ID).

Processes can be:
- Foreground (attached to the terminal)
- Background (running behind the scenes)

---

## Viewing Running Processes

To see current processes:
ps

To see all processes for all users:
ps aux

Common columns:
- USER → who owns the process
- PID → process ID
- %CPU → CPU usage
- %MEM → memory usage
- COMMAND → command being run

---

## Real-Time Process Monitoring with top

To see live system activity:
top

This shows:
- CPU and memory usage
- Running processes
- System load

Useful keys inside top:
- q → quit
- k → kill a process
- r → change priority

---

## Using htop (If Installed)

htop is a more user-friendly version of top.

Run:
htop

Benefits:
- Color-coded output
- Mouse support
- Easier process management

---

## Foreground and Background Processes

Run a command in the background:
command &

Example:
sleep 60 &

View background jobs:
jobs

Bring a job to the foreground:
fg %1

Send a running foreground process to background:
Ctrl + Z
bg

---

## Process States

Common process states:
- R → running
- S → sleeping
- Z → zombie (terminated but not cleaned up)
- T → stopped

Zombie processes usually indicate a parent process issue.

---

## Killing Processes

To stop a process by PID:
kill PID

Example:
kill 1234

To force kill:
kill -9 PID

⚠ kill -9 should be a last resort.

---

## Killing Processes by Name

To kill by process name:
pkill processname

Or:
killall processname

These are useful when I don’t know the PID.

---

## Understanding Signals

Signals tell processes how to behave.

Common signals:
- SIGTERM (15) → polite stop
- SIGKILL (9) → force stop
- SIGSTOP → pause
- SIGCONT → resume

Example:
kill -15 PID
kill -9 PID

---

## Process Priorities (nice)

Processes have priorities called niceness values.

Range:
- -20 → highest priority
- 19 → lowest priority

Start a process with priority:
nice -n 10 command

Change priority of a running process:
renice 5 PID

---

## Checking Resource Usage

To see memory usage:
free -h

To see disk usage:
df -h

To see per-process resource use:
top
htop

---

## Finding a Process

Search for a process:
ps aux | grep processname

This is one of the fastest ways to locate stuck or misbehaving processes.

---

## Practical Process Troubleshooting

Here’s how I usually diagnose issues:

If the system feels slow:
top

If a program freezes:
ps aux | grep program
kill PID

If a process won’t stop:
kill -9 PID

If I need it running in background:
command &

---

## Running Long Tasks Safely

To keep processes running after logout:
nohup command &

This is useful for long scans or scripts.

---

## Good Process Management Habits

- Monitor system resources regularly
- Kill processes gracefully when possible
- Avoid overusing kill -9
- Run heavy tasks with lower priority
- Learn top and ps well — they solve most issues

---

## Key Takeaway

Process management gives me control over what my Linux system is doing at all times. Once I understand how to view, prioritize, and stop processes, troubleshooting becomes faster and the system stays stable and responsive.
