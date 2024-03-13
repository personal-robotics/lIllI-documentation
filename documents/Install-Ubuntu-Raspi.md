# Ubuntu Installation on Raspberry Pi

## Recommended Hardware

Micro-SD cards for Raspberry Pi 3, 4, Zero (2023/03):
https://www.tomshardware.com/best-picks/raspberry-pi-microsd-cards

## Recommended Distribution

The recommended distribution is Ubuntu Desktop 22.04.3 LTS (64 bit).

NOTE: 32-bit version might have slightly lower memory consumption that 64-bit versions, however ROS2 provides binary packages only for 64-bit Ubuntu. ROS2 can still be compiled from source on Ubuntu 22.04 32-bit and Debian 11 (from: https://docs.ros.org/en/humble/How-To-Guides/Installing-on-Raspberry-Pi.html).

NOTE: Ubuntu Desktop 22.04.3 LTS officially requires at least 2 GB of RAM and is therefore only suited for Raspberry Pi 4. However, it can still be installed and used as a headless server on a Raspberry Pi 3 with 1 GB RAM.

NOTE: Ubuntu Server 22.04.2 LTS appears to have an issue where the wifi interface is not available on a Raspberry Pi 3. Ubuntu Server also features a different network configuration framework and package selection, which makes it less suitable as an OS for ROS 2 development.

NOTE: Raspberry Pi OS is based on Debian, for which there are no binary packages provided of ROS 2 Humble Hawksbill
see: https://www.raspberrypi.com/software/operating-systems/#raspberry-pi-os-64-bit
and: https://www.ros.org/reps/rep-2000.html#humble-hawksbill-may-2022-may-2027

## Installation

Use raspi-imager (https://www.raspberrypi.com/software/) to download and write the OS to an SD card or USB drive (for USB booting see hints below). On Ubuntu, install and run the software with `sudo apt install rpi-imager && rpi-imager`

Select the hardware, distribution and installation media, then start the installation. The software will automatically download the appropriate image.

NOTE: raspi-imager only lists distributions officially supported for the selected hardware. You can still install other images (e.g. Ubuntu Desktop 22.04 LTS on Raspi 3) by selecting a custom image. In this case, download the image first from the official website, then select it in raspi-imager.

HINT: set user name, WiFi password, and other options in raspi-imager before writing the OS. For this, open the advanced configuration menu with Ctrl+Shift+X. This way you do not have to enter them at first boot. 
This does not work when selecting a custom image (e.g. Ubuntu Desktop on Raspi 3), though.

## First Steps

Install SD card or USB drive in Raspberry-pi, connect monitor, keyboard, mouse, power supply.

Follow on-screen instructions.

NOTE: Upon first boot, the system might restart several times for initial configuration (e.g. resizing the file system) before the desktop appears.

## Tips & Tricks

### Disable Booting to Desktop

For headless servers, it is recommended to disable starting the desktop environment at boot, with:
```
sudo systemctl set-default multi-user
```

If you ever want to enable booting into the desktop, use:
```
sudo systemctl set-default graphical
```

### Disable Tracker 

From https://askubuntu.com/questions/1187191/tracker-process-taking-lot-of-cpu

`sudo apt purge tracker tracker-extract tracker-miner-fs`

This will remove packages:
gnome-shell-extension-desktop-icons-ng* nautilus* nautilus-share* tracker* tracker-extract* tracker-miner-fs* ubuntu-desktop* ubuntu-desktop-minimal* ubuntu-desktop-raspi*

### Booting from USB Drive

from: https://www.instructables.com/Booting-Raspberry-Pi-3-B-With-a-USB-Drive/

Raspberry Pi 3B+ supports booting from USB Drive out-of-the-box.

For the 3B model, enable USB boot as described above.
WARNING: this step cannot be undone. However, booting from SD card will still work if USB boot is enabled.

As an alternative, install both a bootable SD card and a bootable USB drive, and change the `root` parameter in the file `/boot/cmdline.txt` to point to the bootable USB drive partition. Use the command `blkid` to find the partUUID of that partition.

NOTE: boot partition /dev/mmcblk0p1 must be mounted as /boot/firmware to receive kernel updates. Remove the label and set new UUID for USB stick boot partition, so the SD card partition is mounted as /boot/firmware:

```
dosfslabel    -r /dev/sda1
dosfslabel -i -r /dev/sda1
# relabel SD card root partition 
tune2fs -L oldroot /dev/mmcblk0p2
# freshly checked fs required for random UUID
e2fsck -f /dev/mmcblk0p2
tune2fs -U random /dev/mmcblk0p2
```


When both SD card and USB stick are installed and both root partitions have the same label/UUID, the system boots the kernel from the SD card but uses the USB partition as the root file system?!

TODO 

sudo apt install linux-modules-extra-raspi

### Access to I2C Device

To fix access error `Permission denied: '/dev/i2c-1'`

check permissions of device file: `ls -l /dev/i2c-1` and add account `user` to dialout group: `sudo usermod -a -G dialout user` 

If user is currently logged in, log out and log in again for changes to take effect.

If using the VS Code Remote Host Extension, it might also be necessary to restart it (TODO add how to).