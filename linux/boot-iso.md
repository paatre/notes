# How to install Linux from a bootable ISO image

## Introduction

This guide will show you how to install Linux from a bootable ISO image. The guide will go from downloading the ISO image to an USB drive and preparing it for installation to how to flash the image to the drive and boot from it.

The guide will use Ubuntu 26.04 as an example, but the steps should be similar for other Linux distributions.

## Downloading the ISO image

1. Go to the official Ubuntu website: https://ubuntu.com/download/desktop
1. Click on the "Download" button for the latest version and correct architecture (e.g., 64-bit) of Ubuntu.
1. Save the ISO file to your computer. The ISO is around 6-7 GB in size, so it may take some time to download depending on your internet speed.

## Preparing the USB drive

1. Insert a USB drive with at least 8 GB of storage into your computer. Note that all data on the USB drive will be erased during this process, so make sure to back up any important files.
1. Identify the device name of your USB drive. You can use the `lsblk` command in the terminal to list all block devices and find your USB drive (e.g., `/dev/sda`).
1. Unmount the USB drive if it is automatically mounted by the system. Without unmounting, the flashing process may fail due to the drive being used and locked by the kernel OR due to data data corruption because OS keeps writing to it when mounted.
1. Flash the ISO image to the USB drive. "Flashing" means writing the ISO image to the USB drive in a way that makes it bootable.
1. Optional: Rename the USB drive to something like "Ubuntu 26.04" for easier identification when booting.
1. After the process above is complete, safely eject the USB drive from your computer.

### CLI

There are couple of ways to do this on the command line. One of the most common tools for flashing ISO images to USB drives is `dd`.

```bash
sudo umount /dev/disk
sudo dd if=/path/to/ubuntu-26.04-desktop-amd64.iso of=/dev/disk bs=4M status=progress oflag=sync
```

But other tools like `cp` can also be used because it essentially does the same thing as `dd` when copying an ISO image to a block device. You just need to make sure to call `sync` after the copy to ensure that all data is written to the USB drive before it is ejected. If not, some data may still be in the buffer and not written to the drive, which can lead to a corrupted installation media.

```bash
sudo umount /dev/disk
cp /path/to/ubuntu-26.04-desktop-amd64.iso /dev/disk
sync
```

You can also use `pv` to monitor the progress of the copy operation:

```bash
sudo umount /dev/disk
pv /path/to/ubuntu-26.04-desktop-amd64.iso --output=/dev/disk --sync
```

You can also use `pv` to get a progress bar while copying:

```bash
sudo umount /dev/disk
pv /path/to/ubuntu-26.04-desktop-amd64.iso --output=/dev/disk --sync
```

### GUI

There are multiple ways to do this using a GUI. On Linux and Ubuntu, there are three popular tools: Ubuntu's Startup Disk Creator and GNOME's Disks.

Startup Disk Creator is a tool that comes pre-installed with Ubuntu. It provides a simple interface for creating a bootable USB drive from an ISO image. When you open the application the application will automatically detect the ISO image and the USB drive. You just need to make sure the correct selections are chosen, and then click "Make Startup Disk" to start the process.

With Disks, you need to select the USB drive to be flashed from the list of devices on the left. Then click on the "menu" button (three vertical dots) and select "Restore Disk Image". In the dialog that opens, select the ISO image you downloaded and click "Start Restoring". This will start the process of flashing the ISO image to the USB drive.

## Changing label of the USB drive

After flashing the ISO image to the USB drive, you can change the label of the drive for easier identification when booting.

You can do this with GNOME Disks by selecting the USB drive from the list of devices on the left, then clicking on the "menu" button (three vertical dots) and selecting "Edit Filesystem". In the dialog that opens, you can change the label of the drive to something like "Ubuntu 26.04" and click "OK" to save the changes.
