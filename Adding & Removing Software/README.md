# Adding and Removing Software

As I get more comfortable with Linux, one of the first real “power user” skills I focus on is managing software. Every distribution has its own package manager, but the core idea is the same: instead of downloading programs from random websites, Linux gives you a built‑in system to install, update, and remove software safely. In this section, I walk through how I handle software management and the commands I rely on to keep a system clean and up to date.

## Understanding Package Managers

Different Linux distributions use different package managers, but they all serve the same purpose: they install software, track dependencies, and keep everything organized. The two most common families are:

- **Debian‑based systems (Ubuntu, Kali, etc.)** use `apt`
- **Red Hat‑based systems (CentOS, Fedora, RHEL)** use `yum` or `dnf`

Since most security‑focused and beginner‑friendly distros use `apt`, that’s what I focus on here.

## Updating the System

Before installing anything, I always update the package list. This ensures the system knows about the latest versions available:
sudo apt update

This doesn’t install updates—it just refreshes the list of available packages. To actually upgrade the installed software:
sudo apt upgrade

I run these two commands regularly to keep the system current and secure.

## Installing Software

To install a package, the pattern is simple:
sudo apt install package-name

For example, if I want to install `nmap`:
sudo apt install nmap

The package manager handles everything—dependencies, configuration, and setup.

If I’m not sure whether a package exists or what it’s called, I search for it:
apt search keyword

This is helpful when I know what I want to do but don’t know the exact package name.

## Checking Installed Software

To see if a package is installed:
apt list --installed | grep package-name

This gives me a quick confirmation without digging through menus or directories.

If I want to see details about a package:
apt show package-name

This includes the version, description, dependencies, and where it came from.

## Removing Software

When I no longer need a program, I remove it cleanly:
sudo apt remove package-name

This removes the software but leaves behind configuration files. If I want a full cleanup:
sudo apt purge package-name

And to remove unused dependencies:
sudo apt autoremove

I run `autoremove` occasionally to keep the system tidy.

## Installing from .deb Files

Sometimes software isn’t available in the default repositories. If I download a `.deb` file, I install it with:
sudo dpkg -i file.deb

If there are missing dependencies, I fix them with:
sudo apt --fix-broken install

This is common when installing third‑party tools.

## Adding External Repositories

Some software requires adding a new repository. The general pattern looks like this:
sudo add-apt-repository ppa:repository/name
sudo apt update

After that, I can install the package normally.

I only add external repositories when necessary, since they introduce software outside the main distribution’s control.

## Practical Workflow

Here’s how I typically handle software on a fresh system.

First, I update everything:
sudo apt update
sudo apt upgrade

Then I install the tools I know I’ll need:
sudo apt install nmap curl git net-tools

If I want to check whether something is already installed:
apt list --installed | grep curl

If I decide I don’t need a tool anymore:
sudo apt remove nmap

And to clean up leftover dependencies:
sudo apt autoremove

This keeps the system lean, organized, and predictable.

## Building Good Software Management Habits

A few habits make software management smooth and safe:

- Always run `apt update` before installing anything.
- Use `apt search` when you’re unsure of a package name.
- Use `apt show` to understand what a package does.
- Remove software you don’t use to keep the system clean.
- Avoid random downloads—use the package manager whenever possible.

Once you get comfortable with these commands, installing and removing software becomes second nature, and managing a Linux system feels much more controlled and efficient.
