# OpenBSD on RaspberryPi 4 B

## Update EEPROM via Raspberry Pi OS

1. Boot Raspberry Pi OS on an sdcard
2. Run updates:

```
apt update && apt upgrade -y

rpi-eeprom-update
```


## Load the latest UEFI onto an sdcard (or USB drive)

Follow these steps: https://github.com/pftf/RPi4#installation

## Set UEFI settings for OpenBSD compatability

* Boot with hdmi monitor and keyboard attached
* Press ESC to enter UEFI settings
* Device Manager -> Raspberry Pi Configuration -> Advanced Configuration ->
  * Limit RAM to 3 GB: *Disabled*
  * System Table Selection: *Devicetree*
* Device Manager -> Raspberry Pi Configuration -> SD/MMC Configuration ->
  * uSD/eMMC Routing: *eMMC2 SDHCI*

Save the above with F10 and Reset from the main UEFI menu.

You can also try to set the boot order to have your USB drive first but I found that this setting would never actually save and take effect upon a reboot.


## Install OpenBSD

* Read the docs: https://www.openbsd.org/arm64.html
* And more docs: https://ftp.openbsd.org/pub/OpenBSD/6.8/arm64/INSTALL.arm64
(In particular the section: 'Install on Raspberry Pi 4:')

### Load miniroot onto a usb drive

```
# from an OpenBSD machine
dd if=miniroot68.img of=/dev/rsdXc bs=1m status=progress

# Or from a linux machine
dd if=miniroot68.img of=/dev/sdX bs=1m status=progress
```

### Boot the OpenBSD installer (serial console NOT required)

* Boot with hdmi monitor and keyboard attached
* Press ESC to enter UEFI settings
* Boot Manager -> Select your USB drive and press ENTER
* OpenBSD bootloader should start: 'OpenBSD/arm64 BOOTAA64 1.3'
  * QUICKLY Interrupt it with a letter keypress 'asdf etc'
  * Backspace whatever you typed
* Set the tty to use the framebuffer instead of the serial port:
  * set tty fb0
  * Thanks Christopher Solskogen: https://marc.info/?l=openbsd-arm&m=160683302013161&w=2
  * (I have not yet been successful in getting /etc/boot.conf to work as per his note and have been doing the above manually on every boot)
* Now do a normal OpenBSD install (over the top of the same USB drive if desired).
  * Wifi firmware is missing from the installer so this is easiest with ethernet plugged in for the install.  Wifi firmware is automatically installed on first boot if ethernet is available.
