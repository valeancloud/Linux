# Managing User Environment Variables

Environment variables control how the Linux system and applications behave for a user. They define things like command paths, default editors, language settings, and program behavior. Understanding them is critical for customization, scripting, and troubleshooting.

---

## What Are Environment Variables?

Environment variables are key-value pairs stored in memory and inherited by child processes.

Example:
PATH=/usr/local/bin:/usr/bin:/bin

This tells the system where to look for executable programs when you type a command.

---

## Viewing Environment Variables

To see all environment variables for your current session:
env

or:
printenv

To view a specific variable:
echo $PATH
echo $HOME
echo $USER

---

## Common Environment Variables

Some important ones you’ll see often:

- HOME → Your home directory
- USER → Current logged-in user
- PATH → Directories searched for commands
- SHELL → Your default shell
- PWD → Current working directory
- LANG → Language and locale settings

---

## Temporary Environment Variables (Session-Only)

To set a variable for the current shell session:
export MYVAR="testvalue"

Verify it:
echo $MYVAR

This variable disappears when you close the terminal.

---

## Making Variables Permanent (User-Level)

To make environment variables persist across sessions, add them to shell configuration files.

### For Bash users (most common):

Edit:
~/.bashrc

Add:
export MYVAR="testvalue"

Apply changes without logging out:
source ~/.bashrc

---

## Login vs Non-Login Shell Files

Linux loads different files depending on how the shell starts:

- ~/.bash_profile → Login shells
- ~/.bashrc → Interactive shells
- ~/.profile → Used if bash_profile doesn’t exist

Best practice:
- Put environment variables in ~/.bashrc
- Or ~/.profile for broader compatibility

---

## System-Wide Environment Variables

For variables that apply to all users:

Edit:
 /etc/environment

Example:
MYVAR="globalvalue"

Changes require logout/login to take effect.

⚠ Use caution—this affects every user.

---

## Modifying the PATH Variable

To add a directory to PATH safely:
export PATH=$PATH:/opt/mytools/bin

To make it permanent:
Add the same line to ~/.bashrc

Verify:
echo $PATH

---

## Removing Environment Variables

To remove a variable from the current session:
unset MYVAR

To remove permanently:
- Delete it from ~/.bashrc, ~/.profile, or /etc/environment

---

## Environment Variables in Scripts

Variables can be used inside scripts:
echo "Home directory is $HOME"

Exported variables are inherited by child processes:
export API_KEY="abc123"

---

## Practical Workflow

Typical setup steps:

1. Check existing variables:
   env
2. Add custom paths or settings:
   export PATH=$PATH:/custom/bin
3. Persist changes in ~/.bashrc
4. Reload shell:
   source ~/.bashrc

---

## Good Environment Variable Habits

- Keep PATH clean and minimal
- Avoid hardcoding secrets in plain text
- Use user-level configs before system-wide ones
- Comment your changes for clarity

---

## Key Takeaway

Environment variables let you customize and control how Linux behaves for you and your programs. Once mastered, they become a powerful tool for automation, scripting, and system efficiency.
