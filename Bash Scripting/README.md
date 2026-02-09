# Bash Scripting

Bash scripting allows you to automate tasks, chain commands together, and control system behavior using logic. Instead of typing commands manually, you write them once and let the system run them for you.

---

## What Is a Bash Script?

A Bash script is a plain text file containing Linux commands executed in sequence by the Bash shell.

Scripts are commonly used for:
- Automation
- System maintenance
- Recon and enumeration
- Repetitive administrative tasks

---

## Creating Your First Bash Script

Create a new file:
touch script.sh

Open it in an editor:
nano script.sh

Add the shebang at the top:
#!/bin/bash

This tells the system to run the script using Bash.

---

## Making a Script Executable

Give execution permission:
chmod +x script.sh

Run it:
./script.sh

Or:
bash script.sh

---

## Basic Script Structure

Example:
#!/bin/bash
echo "Hello, world"

Scripts execute top to bottom, line by line.

---

## Variables in Bash

Define a variable (no spaces):
NAME="Andrew"

Use it:
echo "Hello $NAME"

Variables are case-sensitive.

---

## User Input

Read input from the user:
read USERNAME
echo "You entered: $USERNAME"

Prompt with text:
read -p "Enter your name: " NAME

---

## Arguments Passed to Scripts

Scripts can accept arguments when run.

Example:
./script.sh arg1 arg2

Inside the script:
$1 → First argument  
$2 → Second argument  
$@ → All arguments  
$# → Number of arguments  

---

## Conditional Statements (if)

Basic if statement:
if [ $VALUE -eq 10 ]; then
  echo "Value is 10"
fi

Common comparisons:
- -eq → equal
- -ne → not equal
- -gt → greater than
- -lt → less than
- = → string comparison

---

## If-Else Logic

Example:
if [ $USER == "root" ]; then
  echo "Running as root"
else
  echo "Not running as root"
fi

---

## Loops

### For Loop

Example:
for i in 1 2 3; do
  echo $i
done

---

### While Loop

Example:
COUNT=1
while [ $COUNT -le 5 ]; do
  echo $COUNT
  COUNT=$((COUNT+1))
done

---

## Command Substitution

Store command output in a variable:
DATE=$(date)

Use it:
echo "Today is $DATE"

---

## Exit Status and Logic

Every command returns an exit code:
0 → Success  
Non-zero → Failure  

Check last command:
echo $?

Use in logic:
if [ $? -eq 0 ]; then
  echo "Command succeeded"
fi

---

## Functions

Functions group reusable logic.

Example:
myfunc() {
  echo "Function running"
}

Call it:
myfunc

---

## Redirecting Input and Output

Redirect output:
ls > files.txt

Append output:
ls >> files.txt

Redirect errors:
ls /fake 2> error.txt

---

## Practical Example Script

Example automation script:
#!/bin/bash
echo "Updating system"
sudo apt update && sudo apt upgrade -y
echo "Done"

---

## Common Bash Scripting Mistakes

- Forgetting the shebang
- Using spaces around =
- Not quoting variables
- Running scripts without execute permissions

---

## Good Bash Scripting Habits

- Comment your scripts
- Use clear variable names
- Test scripts before running as root
- Keep scripts simple and readable

---

## Key Takeaway

Bash scripting turns manual Linux commands into repeatable, automated workflows. Mastering it is essential for efficiency, system control, and security-focused tasks.
