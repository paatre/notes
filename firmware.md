# Firmware

Ubuntu Wiki has its own page for explaining what a firmware is. Here's a citation from the page:

> Many devices have two essential software pieces that make them function in your operating system. The first is a working driver, which is the software that lets your system talk to the hardware. **The second is firmware, which is usually a small piece of code that is uploaded directly to the device for it to function correctly. You can think of the firmware as a way of programming the hardware inside the device.** In fact, in almost all cases firmware is treated like hardware in that it's a black box; there's no accompanying source code that is freely distributed with it.

## How to update firmware

In Ubuntu, there's a `fwupd` system daemon that enables firmware updates via [LVFS](https://fwupd.org/), Linux Vendor Firmware Service, a free portal for hardware vendors to upload firmware updates. The latest Ubuntu version, 23.10 (Mantic Minotaur), comes with a new `firmware-updater` software created with Flutter and is installed via Snap. The software is a new GUI for using `fwupd`. Firmware updates can also be installed via a command-line utility tool `fwupdmgr` which communicates with `fwupd` daemon.
