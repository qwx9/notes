# void on beaglebone black

## setup

- download available rootfs
- partition 2gb sd card:
	* mmcblk0p1: 64mb FAT32, bootable
	* mmcblk0p1: rest is a linux partition
- mkfs
- mkfs.ext4: disable journaling: -O ^has_journal,
the kernel seems to assume this and it won't boot otherwise
- mount both
- unpack rootfs into ext4
- must copy in a certain order into the FAT32
to put shit in specific blocks:
first MLO then u-boot.img
- touch blank uEnv.txt


## deviations

- this is where shit hits the fan,
had to do this to get anything
- copy boneblack dtd and zImage to FAT32 root
- then add to uEnv.txt to set the right partition, root= and bootpart
- disks for u-boot: 0 is sd, 1 is emmc, we want to boot from 0:1,
the ext4 partition


## result

- nope!
- the rootfs' available have 3.8 kernel
which doesn't work for some reason
- 3.8 is known to cause problems
because of a significant change wrt device trees
- we've had issues with it with gentoo
- boot hangs at runlevel 1->2
