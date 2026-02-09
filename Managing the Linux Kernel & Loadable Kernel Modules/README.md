# Managing the Linux Kernel & Loadable Kernel Modules

As I move deeper into Linux, I realize the kernel is the heart of the entire operating system. It manages hardware, memory, processes, networking, and system calls. Understanding how to inspect, tune, and extend the kernel gives me low-level visibility and control — which is essential for security work, troubleshooting, and system customization.

---

## What the Kernel Is

The kernel is a single binary that loads at boot and runs in privileged mode.

To check the running kernel version:
uname -r

To see full kernel details:
uname -a

Kernel files typically live in:
- /boot/vmlinuz-<version>
- /lib/modules/<version>

Each kernel version has its own module directory.

---

## Kernel Parameters

Kernel parameters control how the system behaves at boot.

To see the parameters currently in use:
cat /proc/cmdline

To change parameters temporarily:
- Edit them in the GRUB boot menu

To change them permanently:
Edit:
/etc/default/grub

Then rebuild:
sudo update-grub

Common parameters:
- quiet → fewer boot messages
- nosplash → disables splash screen
- init=/bin/bash → boot directly into a shell
- single → single-user mode

---

## The /proc Virtual Filesystem

The /proc directory exposes kernel internals as virtual files.

Useful entries:
- /proc/cpuinfo → CPU details
- /proc/meminfo → memory usage
- /proc/modules → loaded kernel modules
- /proc/sys/ → tunable kernel parameters

To view loaded modules:
cat /proc/modules

To inspect kernel tunables:
ls /proc/sys

To read a specific tunable:
cat /proc/sys/net/ipv4/ip_forward

To modify a tunable temporarily:
echo 1 | sudo tee /proc/sys/net/ipv4/ip_forward

For persistent changes:
Edit /etc/sysctl.conf
Then apply:
sudo sysctl -p

---

## Loadable Kernel Modules (LKMs)

Loadable Kernel Modules extend kernel functionality at runtime. They allow the kernel to add features without rebooting.

To list loaded modules:
lsmod

To get module information:
modinfo <module>

To load a module:
sudo modprobe <module>

To unload a module:
sudo modprobe -r <module>

To load a module manually:
sudo insmod <module>.ko

To remove a manually inserted module:
sudo rmmod <module>

modprobe is preferred because it handles dependencies automatically.

---

## Module Dependencies

Modules often rely on other modules.

To see dependencies:
modinfo <module> | grep depends

To check if a module is in use:
lsmod | grep <module>

Dependencies matter when unloading modules — some cannot be removed if another module relies on them.

---

## Kernel Logs

Kernel messages are essential for debugging module issues.

To view kernel logs:
dmesg

To filter for module-related messages:
dmesg | grep -i module

To watch logs in real time:
dmesg --follow

---

## Building a Simple Kernel Module

A minimal module has two functions:
- One runs when the module loads
- One runs when it unloads

Example:
#include <linux/init.h>
#include <linux/module.h>

static int __init init_mod(void) {
    printk(KERN_INFO "Module loaded\n");
    return 0;
}

static void __exit exit_mod(void) {
    printk(KERN_INFO "Module unloaded\n");
}

module_init(init_mod);
module_exit(exit_mod);

MODULE_LICENSE("GPL");

To compile, use a Makefile:
obj-m += mymodule.o

all:
    make -C /lib/modules/$(shell uname -r)/build M=$(PWD) modules

clean:
    make -C /lib/modules/$(shell uname -r)/build M=$(PWD) clean

Build:
make

Load:
sudo insmod mymodule.ko

Check logs:
dmesg | tail

Unload:
sudo rmmod mymodule

---

## Securing Kernel Modules

Kernel modules can be abused by attackers to hide processes, escalate privileges, or install rootkits.

Hardening steps:

Disable module loading at runtime:
echo 1 | sudo tee /proc/sys/kernel/modules_disabled

(Requires reboot to undo.)

Require signed modules:
sudo mokutil --enable-validation

Monitor module activity:
journalctl -k

Regularly inspect:
- /proc/modules
- dmesg output

---

## Kernel Updates

To check for kernel updates:
apt list --upgradable | grep linux

To install updates:
sudo apt update
sudo apt upgrade

Reboot into the new kernel:
sudo reboot

Verify:
uname -r

---

## Practical Kernel Awareness

Here’s how I approach kernel-level troubleshooting:

If hardware isn’t detected:
dmesg | grep -i error

If a module won’t load:
modinfo <module>
dmesg | tail

If networking features aren’t working:
Check /proc/sys/net settings

If performance is off:
Inspect CPU, memory, and scheduler behavior via /proc

---

## Kernel Security Mindset

Key questions I ask:
- What modules are loaded?
- Are any modules unnecessary or suspicious?
- Are kernel parameters secure?
- Is module loading restricted?
- Are logs showing warnings or failures?

Kernel-level visibility is essential for system integrity.

---

## Common Mistakes

- Using insmod instead of modprobe
- Forgetting to rebuild GRUB after parameter changes
- Ignoring kernel logs
- Leaving module loading unrestricted
- Running outdated kernels with known vulnerabilities

Kernel misconfigurations can silently weaken system security.

---

## Good Kernel Practices

- Keep kernels updated
- Use modprobe for module management
- Monitor dmesg regularly
- Restrict module loading when possible
- Use sysctl for persistent tuning
- Review /proc for unexpected changes

---

## Key Takeaway

The kernel is the foundation of Linux. Once I understand how to inspect, tune, and extend it — and how modules interact with it — I gain deep control over the system. This knowledge is essential for security, troubleshooting, and advanced Linux work.
