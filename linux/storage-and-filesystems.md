# Storage and filesystems

## Drives and block devices

When a hard drive or SSD is plugged in, Linux gives the drive a name like:

1. `sdX`: historically meant "SCSI disk" but now is used for all kinds of disks, including SATA (which is a more modern and newer standard than SCSI) and USB drives. The `X` is a letter that starts with `a` for the first drive, `b` for the second, and so on.
2. `nvmeXnY`: for NVMe (Non-Volatile Memory Express) M.2 SSD, where `X` is the controller number starting from 0, and `Y` is the namespace number starting from 1.
3. `vdX` and `xvdX`: for virtual disks in virtualized environments (QEMU, KVM, VirtualBox, AWS, GCP), where `X` is a letter starting from `a`.
4. `hdX`: for older PATA/IDE drives, where `X` is a letter starting from `a`. This naming is mostly obsolete, modern Linux systems typically use `sdX` for all types of disks.
5. `mmcblkX`: for SD cards and eMMC (embedded MultiMediaCard) storage, where `X` is a number starting from 0. You can see this if you e.g. work with Raspberry Pi, which uses an SD card as its main storage.
6. `loopX`: for loop devices, which are virtual block devices that allow you to mount files as if they were disks. The `X` is a number starting from 0.

These drives are located under the `/dev` directory.

Use `lsblk` to list all block devices and their partitions, along with their sizes and mount points. `lsblk -f` also shows the filesystem type and UUID. You can use `df -h` to see the disk usage of mounted filesystems in a human-readable format. GNOME's Disk Usage Analyzer (Baobab) provides a graphical interface to visualize disk usage and find large files and directories.

Most desktop environments also have a disk management tool (like GNOME Disks) that allows you to view and manage your drives and partitions, including formatting, creating partitions, and checking the health of the drive.

## Partitions

A drive can be divided into multiple partitions. Partitions are like separate sections of the drive. Each partition can have its own filesystem and can be mounted independently. Partitions are named by appending a number to the drive name. For example, if you have a drive named `/dev/sda`, its partitions would be `/dev/sda1`, `/dev/sda2`, etc.

For example, modern Linux systems use UEFI (Unified Extensible Firmware Interface) to boot, and UEFI understands FAT32 filesystems. When you install Linux on a UEFI system, the installer creates a small FAT32 partition (usually around 100-500 MB) called the EFI System Partition (ESP). This partition is used to store the bootloader and other files needed for the system to boot. The ESP is typically mounted at `/boot/efi` in the Linux filesystem. The rest of the drive can be partitioned and formatted with other filesystems (like ext4, XFS, Btrfs, etc.) for the root filesystem (`/`).

## Mounting

To see and use the files on a partition, it needs to be mounted. Mounting is the process of making a filesystem accessible at a certain point in the directory tree. For example, if you have a partition `/dev/sda1` with an ext4 filesystem, you can mount it to `/mnt/data` using the command:

```bash
sudo mount /dev/sda1 /mnt/data
```

After mounting, you can access the files on that partition by navigating to `/mnt/data`. To unmount the partition when you're done, use:

```bash
sudo umount /mnt/data
```

You can also use the `mount` command without arguments to see all currently mounted filesystems and their mount points. The `df -h` command also shows the mounted filesystems along with their disk usage.

## Filesystems

A filesystem is a way of organizing and storing files on a partition. There are many different types of filesystems, each with its own features and use cases.

### ext4

The ext4 (fourth extended filesystem) is the most commonly used filesystem on Linux and considered to be very stable. ext4 was published on 2006. It is a _journaling_ filesystem, which means it keeps a log of changes that will be made to the filesystem. This helps prevent data corruption in case of a crash or power failure due to the fact that the filesystem can be quickly restored to a consistent state using a journal. ext4 supports large files (up to 16 TB) and large volumes (up to 1 EB). Used by default on distributions like Ubuntu and Debian, and many other Ubuntu-based and Debian-based distributions.

### Btrfs

Btrfs (B-tree filesystem) is a newer filesystem that offers advanced features like snapshots, subvolumes, and built-in RAID support. Btrfs was accepted to Linux kernel in 2009 and declared stable in 2013. Btrfs is designed to be a modern filesystem that can handle large storage devices and provide better performance and reliability. Btrfs is used by default on distributions like openSUSE and Fedora.

### XFS

XFS (Extended Filesystem) is a high-performance, journaling file-system that is designed for large files and high-speed data transfer. XFS was accepted to Linux kernel in 2001. XFS is a journaling filesystem that is optimized for parallel I/O operations, making it suitable for high-performance applications and large storage systems. XFS is used by default on distributions like Red Hat Enterprise Linux (RHEL) and CentOS.

### FAT32

FAT32 (File Allocation Table 32) is an older filesystem that is widely supported across different operating systems, including Windows and macOS on top of Linux. FAT32 was accepted to Linux kernel in 1996. It is a simple filesystem that is easy to format and use, but it has limitations such as a maximum file size of 4 GB and a maximum partition size of 8 TB. FAT32 is commonly used for USB flash drives, SD cards and UEFI System Partitions (ESP).

### exFAT

exFAT (Extended File Allocation Table) is a newer filesystem developed by Microsoft to replace FAT32. exFAT was accepted to Linux kernel in 2019. It supports larger file sizes (up to 16 EB) and larger partitions (up to 128 PB). exFAT is widely supported across different operating systems, including Windows and macOS on top of Linux. exFAT is commonly used for external hard drives and USB flash drives that need to be compatible with both Windows and Linux.

### NTFS

NTFS (New Technology File System) is a proprietary, journaling filesystem developed by Microsoft for Windows. NTFS has been supported in the Linux kernel since 2001 and most notably a nfts3 driver which is a native implementation of NTFS in Linux was merged in 2021. NTFS is commonly used for Linux systems that dual-boot with Windows.

### tmpfs

tmpfs is a temporary filesystem that is stored in RAM. It is used for storing temporary files and data that do not need to be persisted across reboots. tmpfs is often used for directories like `/tmp` and `/run`, which are used for temporary files and runtime data. Since tmpfs is stored in RAM, it is very fast, but it also means that the data stored in tmpfs will be lost when the system is rebooted.

## Auto-mounting

There are two main ways to automatically mount a filesystem: using `fstab` or `udisks`. Both handle auto-mounting, but they serve different purposes and are used in different contexts.

### fstab

The `/etc/fstab` file is a configuration file that contains information about filesystems and their mount points. It is used by the system to automatically mount filesystems at boot time. Systemd reads the `fstab` file during the boot process and mounts the filesystems listed in it.

A Linux system assumes that if a filesystem is added to `fstab`, it's expected to be permanent and critical for the system to boot. If a drive listed in `fstab` is missing at boot time, the system will drop to a recovery shell and ask for the root password to allow you to fix the issue. This is a safety mechanism to prevent the system from booting with missing critical filesystems, which could lead to data loss or an unbootable system. If you have a non-critical drive that may not always be present, you can add the `nofail` option in `fstab` for that entry, which allows the system to boot even if that drive is missing.

Each line in the `fstab` file represents a filesystem and contains the following fields:

1. The device or partition to be mounted (e.g., `/dev/sda1`). This is usually specified by the device name or by the device UUID.
2. The mount point where the filesystem will be mounted (e.g., `/mnt/data`).
3. The filesystem type (e.g., `ext4`, `vfat`).
4. Mount options (e.g., `errors=remount-ro` which remounts the filesystem as read-only if there are errors, and `umask` which sets the permissions for files and directories on the filesystem).
5. Dump setting: an old backup utility that is rarely used today. A value of `0` means the filesystem will not be dumped, while `1` means it will be dumped. Usually set to `0` for most filesystems.
6. Pass setting: used by the `fsck` utility to determine the order in which filesystems should be checked at boot time. The root filesystem should have a value of `1`, while other filesystems should have a value of `2` or higher. A value of `0` means the filesystem will not be checked.

You can use `sudo mount -a` to mount all filesystems listed in `fstab` that are not currently mounted. This is useful for testing changes to the `fstab` file without rebooting the system.

To check the UUID of a partition, you can use the `blkid` command. Remember to use use `UUID` and not `PARTUUID` in the `fstab` file, as `PARTUUID`. Using UUIDs in `fstab` is generally recommended over using device names (like `/dev/sda1`) because device names can change if you add or remove drives, while UUIDs are unique identifiers that remain consistent regardless of the device name.

### udisks

After a Linux system has booted, Systemd starts an `udisks` service that automatically mounts any new drives that are plugged in. This is typically used for removable media like USB flash drives and external hard drives. When you plug in a new drive, `udisks` detects it and mounts it to a default location, usually under `/media/username/` on Ubuntu and Debian or `/run/media/username/` on Fedora, Arch and openSUSE, making it accessible to the user without needing to manually mount it.
