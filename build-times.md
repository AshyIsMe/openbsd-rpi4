# Rough benchmarks of building openbsd base from source on a Raspberry Pi 4 B

The rpi is not in a case, has a heatsink attached and a small fan.


# Running off a SanDisk Ultra Fit 64GB usb stick

umass0 at uhub0 port 2 configuration 1 interface 0 "SanDisk Ultra Fit" rev 3.00/1.00 addr 5
sd1 at scsibus1 targ 1 lun 0: <SanDisk, Ultra Fit, 1.00> removable serial.078155838107bba6d9db

*Used `make -j 4 build` for all builds*

## Default (SLOW) mounting options
This was without softdep and with atime ON by default:

* Kernel:
  * Approx 12mins from memory (Should probably test again)
* Base:
  * `652m52.11s real  1980m49.01s user   283m24.25s system`


## With softdep and noatime on all fstab mounts

* Base:
  * `630m36.32s real  1976m35.38s user   296m55.71s system`

# Running off a memfs ramdisk

```
# 2GB in a count of 512byte sectors
# expr 1024 \* 1024 \* 1024 \* 2 / 512
# 4194304

umount /usr/src
umount /usr/obj

mount_mfs -s 4194304 swap /usr/src
mount_mfs -s 4194304 swap /usr/obj

git clone --depth 1 https://github.com/openbsd/src /usr/src


```

* Kernel:
  * `13m12.05s real    36m38.53s user    12m27.78s system`
* Base:
  * `625m19.66s real  1963m11.83s user   314m59.17s system`
