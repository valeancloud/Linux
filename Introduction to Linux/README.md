# Getting Started with the Basics

When I sit down at a Linux system, I don’t start by clicking around—I start in the terminal. The terminal is where you get direct, precise control over the system. If you’re following along, the goal here is simple: get comfortable moving around, looking at things, and running basic commands without guessing. Once these basics feel natural, everything else in Linux builds on top of them.

## The Terminal and the Shell

The terminal is the window; the shell is the program that actually interprets what you type. On most systems, that shell is `bash` or something similar. When you type a command and press Enter, the shell finds the program, runs it, and shows you the result. If a command seems to “do nothing,” it usually means it succeeded quietly.

A few good habits right away:
- Type carefully and read the output.
- If something errors, don’t panic—read the message, it usually tells you what went wrong.
- Use the terminal as your primary interface, not a last resort.

## Knowing Where You Are

Linux organizes everything in a tree starting at `/` (the root). To do anything meaningful, you need to know where you are in that tree.

To see your current location:
pwd

This prints the full path of your working directory. I run this often when I’m moving around a lot, just to stay oriented.

To see what’s in the current directory:
ls

When I want more detail—like permissions, owners, and sizes:
ls -l

To include hidden files (those starting with a dot):
ls -a

And to combine both:
ls -la

These simple variations give you a quick snapshot of your environment.

## Moving Around the Filesystem

Navigation is one of the first skills to become muscle memory. Here are the core movements I use constantly:

Change to a specific directory:
cd /path/to/location

Go to your home directory:
cd ~

Move up one level:
cd ..

Jump back to the previous directory:
cd -

I use `cd ..` all the time when backing out of nested paths, and `cd -` is great when I’m bouncing between two locations.

## Creating and Organizing Directories

Once I know where I am, I start shaping the structure I need.

Create a single directory:
mkdir projects

Create nested directories in one command:
mkdir -p notes/linux/basics

The `-p` flag is useful when the parent directories don’t exist yet—it builds the whole path for you.

## Working with Files

To create an empty file or update its timestamp:
touch notes.txt

This is a quick way to create placeholder files you’ll fill in later.

Copy a file:
cp source.txt backup.txt

Copy a directory and its contents:
cp -r src/ src-backup/

Move or rename a file (same command does both):
mv oldname.txt newname.txt
mv notes.txt notes/notes.txt

If the destination is a directory, `mv` moves the file there. If it’s a new name, it renames the file.

Remove a file:
rm file.txt

Remove an empty directory:
rmdir emptydir

Remove a directory and everything inside it:
rm -r old-project/

I treat `rm -r` with respect—once it’s gone, it’s gone.

## Looking Inside Files

To quickly print the contents of a file:
cat file.txt

For longer files, I prefer a pager so I can scroll:
less file.txt

Inside `less`, I use:
- space to go forward
- b to go back
- q to quit

To see just the beginning of a file:
head file.txt

To see just the end:
tail file.txt

If I’m watching a log or something that updates over time:
tail -f logfile.log

That `-f` flag is great for monitoring activity in real time.

## Using Echo and Redirection

Sometimes I want to create a file with a single line of text:
echo "Linux basics" > notes.txt

The `>` operator overwrites the file. If I want to append instead of overwrite:
echo "More details..." >> notes.txt

This is a simple but powerful pattern: use `echo` to generate text, and redirection to send it into a file.

## Getting Help When You’re Stuck

Linux has built-in documentation for most commands. When I’m not sure how something works, I start here:
man ls

This opens the manual page for `ls`. I scroll with the arrow keys or space, and press `q` to quit.

Many commands also support:
command --help

For example:
ls --help

This prints a quick usage summary and options. Between `man` and `--help`, you can usually figure out what a command can do without leaving the terminal.

If I don’t remember the exact name of a command but know what it relates to, I can use:
apropos keyword

For example:
apropos directory

This searches the manual page descriptions for anything related to “directory.”

## Command History and Efficiency

The shell keeps a history of what you’ve run. To see it:
history

Instead of retyping commands, I use the up and down arrow keys to scroll through previous commands. This is one of the fastest ways to work—run something once, then tweak and rerun it from history.

Tab completion is another habit I rely on constantly. Start typing a command or filename and press Tab:
- If there’s a unique match, it completes it.
- If there are multiple matches, pressing Tab twice shows the options.

This reduces typos and speeds up navigation significantly.

## Cleaning Up the View

When the terminal gets cluttered, I like to clear it:
clear

This doesn’t reset anything; it just gives you a clean view so you can focus on what you’re doing next.

## Putting It All Together

Here’s a simple flow that ties these basics together in a way I actually use them.

First, I create a workspace:
mkdir projects
cd projects
mkdir linux-basics
cd linux-basics

Then I create a notes file and start capturing information:
echo "Getting started with the basics" > notes.txt
echo "Commands: pwd, ls, cd, mkdir, rm, cat, less, head, tail, echo, man, history" >> notes.txt

I can verify what’s in my directory:
ls -l

And check the contents of my notes:
cat notes.txt

If I want to watch a log file while I’m experimenting:
tail -f /var/log/syslog   # or another log file available on your system

At this stage, the goal isn’t to memorize everything at once. It’s to get comfortable typing these commands, seeing how the system responds, and building a mental model of how Linux behaves. Once these basics feel natural, more advanced topics—like permissions, networking, and scripting—fit into place much more easily.
