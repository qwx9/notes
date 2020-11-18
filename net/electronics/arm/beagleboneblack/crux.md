# installing crux on the beaglebone black

## old attempt notes

Downloaded 2.8 hf generic release from Crux-arm's homepage.

	$ wget http://crux-arm.nu/releases/2.8/crux-arm-rootfs-2.8-hardfp-efikamx.tar.xz
	$ wget http://crux-arm.nu/releases/2.8/crux-arm-rootfs-2.8-hardfp-efikamx.tar.xz.md5
	# prolly a better way (use tmp files?)
	$ md5sum crux-arm-rootfs-2.8-hardfp-efikamx.tar.xz && cat crux-arm-rootfs-2.8-hardfp-efikamx.tar.xz.md5

Installed needed tools on gentoo box. (http://dev.gentoo.org/~armin76/arm/beagleboneblack/install.xml)

	$ sudo emerge dev-vcs/git sys-devel/crossdev dev-embedded/u-boot-tools sys-fs/dosfstools app-arch/lzop

Built armv7a-hardfloat-linux-gnueabi toolchain on gentoo box.

	$ sudo crossdev -S armv7a-hardfloat-linux-gnueabi

Used mkcard.sh to partition a 32GB microSDHC card && mount

	$ wget http://dev.gentoo.org/~armin76/arm/beaglebone/mkcard.sh
	$ bash mkcard.sh /dev/sdh
	$ sudo mkdir -p /mnt/{p1,p2}
	$ sudo mount /dev/sdh1 /mnt/p1
	$ sudo mount /dev/sdh2 /mnt/p2

Extracted downloaded release on partition 2 of microSD card.

	$ sudo tar xpf crux-arm-rootfs-2.8-hardfp.tar -C /mnt/p2/

Configured/compiled u-boot.

	$ wget ftp://ftp.denx.de/pub/u-boot/u-boot-2013.04.tar.bz2
	$ tar xjpf u-boot-2013.04.tar.bz2 && cd u-boot-2013.04
	# We need to apply some patches to U-Boot that enhances
	# support for the BeagleBone Black
	$ git clone https://github.com/beagleboard/meta-beagleboard.git
	# Apply the patches
	$ for i in meta-beagleboard/common-bsp/recipes-bsp/u-boot/u-boot-denx/00*; do patch -p1 < $i; done
	$ make ARCH=arm CROSS_COMPILE=armv7a-hardfloat-linux-gnueabi- am335x_evm_config
	$ make ARCH=arm CROSS_COMPILE=armv7a-hardfloat-linux-gnueabi-
	$ sudo cp MLO u-boot.img /mnt/p1

Clone, cfg, build kernel (differs from guide)

	$ git clone https://github.com/beagleboard/kernel.git
	$ cd kernel
	$ git checkout origin/3.8 -b 3.8
	$ ./patch.sh
	$ cd kernel
	# get needed firmware
	$ wget "http://arago-project.org/git/projects/?p=am33x-cm3.git;a=blob_plain;f=bin/am335x-pm-firmware.bin;hb=HEAD" -O firmware/am335x-pm-firmware.bin
	$ cp ../configs/beaglebone .config
	# http://lists.denx.de/pipermail/u-boot/2011-December/114138.html
	$ LOADADDR=0x80008000 make -j4 ARCH=arm CROSS_COMPILE=armv7a-hardfloat-linux-gnueabi- uImage dtbs
	# this assumes that /mnt/p2 is mounted (don't know if it's strictly
	# necessary, but I would guess that yes)
	$ make -j4 ARCH=arm CROSS_COMPILE=armv7a-hardfloat-linux-gnueabi- modules
	# make -j4 ARCH=arm CROSS_COMPILE=armv7a-hardfloat-linux-gnueabi- INSTALL_MOD_PATH=/mnt/p2 modules_install
	# cp arch/arm/boot/{uImage,/dts/am335x-boneblack.dtb} /mnt/p2/boot/

Edited fstab.

	$ sudo vim /mnt/p2/etc/fstab
	Commented out everything in /mnt/p2/etc/fstab
	Added line:
		/dev/mmcblk0p2		/		ext4		noatime		0 1

Edited inittab.

	$ sudo vim /mnt/p2/etc/inittab
	Added lines:
		# serial console
		s0:12345:respawn:/sbin/agetty 115200 ttyO0 vt100

(Checked that root passwd was empty in /mnt/p2/etc/shadow)

Unmount microSD's partitions

	$ sudo umount /mnt/{p1,p2}

Insert microSD card into bblack, connect serial debug cable.
Boot from microSD.

	Press and hold USR1 button next to microSD slot, wait 5 seconds,
	then plug in miniUSB power while still holding.
	You can release the button once all 4 activity LEDs have lit up.
	If they do not light up, or if they stay lit and nothing happens,
	bad u-boot or kernel config.
	Should be visible on serial output.

Result:

- Could not login as root
- there's no delay after giving login/passwd to local
- maybe problem is different way of installing OS on an arm device?
- maybe openssl,passwd,local problem
- maybe kernel is too new /userspace?
- maybe device tree not handled well yet in 2.8?
- reported dtbo files missing by capemgr
- could try copying over files from arch-arm,
but kernel was compiled fine, problem should be in userspace
