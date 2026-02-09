# Filesystem & Storage Device Management

As I work more deeply with Linux systems, I realize that understanding filesystems and storage devices is critical. Everything in Linux is treated as a file, including disks and partitions. In this section, I break down how storage devices work, how they’re mounted, how to inspect disk usage, and how to manage filesystems safely.

---

## Storage Devices in Linux

Linux represents storage devices as files in the /dev directory.

Common device names:
- /dev/sda → first disk
- /dev/sda1 → first partition on sda
- /dev/sdb → second disk
- /dev/nvme0n1 → NVMe drive
- /dev/nvme0n1p1 → partition on NVMe drive

Knowing these names helps me identify where data lives.

---

## Listing Storage Devices

To list all block devices:
lsblk

This shows:
- Disks
- Partitions
- Mount points
- Sizes

To see detailed disk info:
sudo fdisk -l

---

## Filesystems Explained

A filesystem defines how data is stored and retrieved.

Common Linux filesystems:
- ext4 → most common Linux filesystem
- xfs → high-performance, scalable
- vfat → FAT filesystems (USB drives)
- ntfs → Windows filesystems

Each partition must be formatted with a filesystem before use.

---

## Checking Filesystem Type

To see filesystem types:
lsblk -f

Or:
df -T

This helps me confirm how a device is formatted.

---

## Mounting Filesystems

Mounting makes a filesystem accessible.

Basic syntax:
mount device mount_point

Example:
sudo mount /dev/sdb1 /mnt/usb

Now the device is accessible through /mnt/usb.

---

## Unmounting Filesystems

To safely remove a device:
sudo umount /mnt/usb

Never remove a device before unmounting — it can cause data loss.

---

## Mount Points

A mount point is a directory where a filesystem is attached.

Common mount points:
- / → root filesystem
- /home → user data
- /mnt → temporary mounts
- /media → removable media

---

## Persistent Mounts with /etc/fstab

To mount devices automatically at boot, entries go in /etc/fstab.

Example entry:
UUID=abcd-1234 /data ext4 defaults 0 2

This ensures the filesystem mounts every time the system starts.

---

## Finding a Device UUID

UUIDs are safer than device names.

To find UUIDs:
blkid

Or:
lsblk -f

---

## Disk Usage and Free Space

To see disk usage by filesystem:
df -h

To see directory sizes:
du -sh directory/

To see detailed usage:
du -h --max-depth=1

---

## Checking and Repairing Filesystems

To check a filesystem:
sudo fsck /dev/sdb1

⚠ Filesystems should be unmounted before running fsck.

fsck helps detect and repair filesystem corruption.

---

## Formatting a Partition

To format a partition (destructive):
sudo mkfs.ext4 /dev/sdb1

This erases all existing data on the partition.

---

## Removable Media (USB Drives)

Insert a USB drive and check:
lsblk

Mount manually:
sudo mount /dev/sdb1 /mnt/usb

Unmount before removal:
sudo umount /mnt/usb

---

## Practical Storage Troubleshooting

Here’s how I typically diagnose storage issues:

If a disk is full:
df -h

If I need to find large directories:
du -h --max-depth=1 /

If a device won’t mount:
lsblk
blkid
dmesg | tail

If a filesystem is corrupted:
sudo fsck /dev/sdX

---

## Good Filesystem Management Habits

- Always unmount before removing devices
- Use UUIDs in /etc/fstab
- Monitor disk usage regularly
- Back up data before formatting
- Be cautious with root-level disk commands

---

## Key Takeaway

Understanding filesystems and storage devices gives me confidence and control over where data lives in Linux. Once I know how to inspect, mount, and manage disks safely, storage issues become predictable and manageable instead of stressful.
