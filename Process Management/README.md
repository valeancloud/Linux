# Adding and Removing Software

As I get more comfortable with Linux, one of the first real “power user” skills I focus on is managing software. Instead of downloading programs from random websites, Linux provides package managers that allow me to safely install, update, and remove software while automatically handling dependencies. In this chapter, I walk through how software management works and the commands I use to keep a system clean and up to date.

---

## Understanding Package Managers

Different Linux distributions use different package managers, but they all serve the same purpose: installing software, tracking dependencies, and keeping everything organized.

The two most common families are:

- **Debian-based systems (Ubuntu, Kali, etc.)** use `apt`
- **Red Hat-based systems (CentOS, Fedora, RHEL)** use `yum` or `dnf`

Since most beginner-friendly and security-focused distributions use `apt`, that’s what I focus on here.

---

## Updating the System

Before installing anything, I always update the package list so the system knows about the latest available software.

sudo apt update

This command does not install updates—it only refreshes the package list. To actually upgrade installed software:

sudo apt upgrade

Running these two commands regularly helps keep the system secure and stable.

---

## Installing Software

To install a package, the basic pattern is:

sudo apt install package-name

For example, to install `nmap`:

sudo apt install nmap

The package manager automatically handles dependencies, configuration, and setup.

If I’m unsure of the exact package name, I search for it:

apt search keyword

This is useful when I know what I want to do but don’t know the exact package name.

---

## Checking Installed Software

To check whether a package is installed:

apt list --installed | grep package-name

This gives a quick confirmation without digging through menus or directories.

To view detailed information about a package:

apt show package-name

This includes the version, description, dependencies, and where the package came from.

---

## Removing Software

When I no longer need a program, I remove it with:

sudo apt remove package-name

This removes the software but leaves configuration files behind. For a complete cleanup:

sudo apt purge package-name

To remove unused dependencies that are no longer required:

sudo apt autoremove

I run `autoremove` occasionally to keep the system tidy.

---

## Installing from .deb Files

Sometimes software isn’t available in the default repositories. If I download a `.deb` file, I install it using:

sudo dpkg -i file.deb

If dependency errors occur, I fix them with:

sudo apt --fix-broken install

This is common when installing third-party tools.

---

## Adding External Repositories

Some software requires adding an external repository. The general process looks like this:

sudo add-apt-repository ppa:repository/name  
sudo apt update

Once added, the software can be installed normally using `apt`.

I only add external repositories when necessary, since they introduce software outside the distribution’s main control.

---

## Practical Workflow

This is my typical workflow on a fresh system.

First, I update everything:

sudo apt update  
sudo apt upgrade

Then I install the tools I know I’ll need:

sudo apt install nmap curl git net-tools

To check if something is already installed:

apt list --installed | grep curl

If I decide I don’t need a tool anymore:

sudo apt remove nmap

And finally, I clean up leftover dependencies:

sudo apt autoremove

This keeps the system lean, organized, and predictable.

---

## Building Good Software Management Habits

A few habits make software management smoother and safer:

- Always run `apt update` before installing anything
- Use `apt search` when you’re unsure of a package name
- Use `apt show` to understand what a package does
- Remove software you don’t use
- Avoid random downloads and use the package manager whenever possible

Once these commands become second nature, managing software on Linux feels controlled, efficient, and professional.
