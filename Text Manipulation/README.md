# Text Manipulation

As I move deeper into Linux, one of the most important skills I focus on is learning how to work with text efficiently. Linux is built around text—configuration files, logs, scripts, system output—so being able to search, filter, and transform text directly from the terminal is a huge advantage. In this section, I walk through the tools I rely on to inspect and manipulate text quickly and cleanly.

## Viewing Text in Different Ways

Before manipulating anything, I need to be able to look at it. I start with the simplest tool:
cat file.txt

This prints the entire file to the screen. It’s great for small files, but for anything longer, I switch to:
less file.txt

Inside `less`, I can scroll with the arrow keys, jump pages with space or b, and quit with q. It’s one of the best ways to explore large files without overwhelming the terminal.

If I only need the beginning or end of a file, I use:
head file.txt
tail file.txt

And when I want to watch a file update in real time—like a log that’s actively changing—I use:
tail -f logfile.log

This is especially useful when I’m troubleshooting or monitoring activity.

## Searching Through Text

One of the most powerful tools in Linux is grep. I use it constantly to find patterns inside files:
grep "pattern" file.txt

If I want to search through an entire directory and its subdirectories:
grep -r "pattern" /path/to/search

This is perfect for digging through logs, configs, or codebases.

I can also combine grep with other commands using pipes. For example, if I want to list only the `.txt` files in a directory:
ls -l | grep ".txt"

Or if I want to find a specific process:
ps aux | grep ssh

Pipes let me take the output of one command and feed it directly into another, which becomes incredibly powerful as commands get more complex.

## Redirecting Input and Output

Redirection is one of the core ideas in Linux. Instead of printing output to the screen, I can send it into a file:
command > file.txt

This overwrites the file. If I want to append instead:
command >> file.txt

I use this when I’m building notes, logs, or quick data files.

To feed a file into a command:
command < input.txt

This is useful when a program expects input but I want to provide it automatically.

And of course, pipes let me chain commands together:
command1 | command2

This is the foundation of building more advanced command sequences.

## Creating Text with Echo

Sometimes I want to create a file with a single line of text:
echo "Starting notes" > notes.txt

Or add another line:
echo "More details..." >> notes.txt

This is a simple but effective way to build files without opening an editor.

## Combining Tools for Real Work

Here’s a small workflow that shows how these tools come together.

Let’s say I’m exploring logs and want to find all lines that mention “error,” then save them for review:
grep -r "error" /var/log > errors.txt

If I want to see the most recent errors as they happen:
grep "error" /var/log/syslog | tail -f

Or if I want to extract only the first 20 lines of a file and save them:
head -n 20 bigfile.txt > summary.txt

These small combinations make it easy to slice through large amounts of text without ever leaving the terminal.

## Building Good Habits

As I work with text more often, I’ve learned a few habits that make everything smoother:

- Use grep early and often to narrow down what you’re looking for.
- Use pipes to chain simple commands into powerful workflows.
- Use redirection to save output instead of copying and pasting.
- Use less to explore files without flooding the screen.
- Use tail -f when monitoring logs or active processes.

Mastering these basics makes the terminal feel less like a tool and more like an extension of how I think through problems. Text manipulation is one of the core skills that unlocks everything else in Linux, and getting comfortable with these commands pays off immediately in real-world work.
