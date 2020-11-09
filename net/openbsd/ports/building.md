# building ports

## snapshot

	$ cd /usr
	$ tar xzf /tmp/ports.tar.gz
	$ cd ports
	$ doas make fix-permissions

- see: bsd.port.mk(5)

## mk.conf

	$ cat /etc/mk.conf
	PORTS_PRIVSEP=Yes
	CLEANDEPENDS=Yes
	FETCH_PACKAGES=-a
	PIPE="-pipe"
	ALWAYS_CLEAN=1


## dpb

	$ cd /usr/ports
	$ doas infrastructure/bin/dpb -I pkgpath..

- see: pkgpath(7), dpb(1)
- seems cool but hasn't done anything useful last time??
- build print/texlive/base → no print/texlive/base built


## disk space

- i don't see any value in keeping any distfiles around for any reason,
for my particular use case
(occasional build of a single port)
- but there's nothing in place in any of the infrastructure
to allow automatic cleaning of everything once builds finish
- to avoid having huge /usr partitions just for ports,
we can use mfs instead: mount_mfs(8)
- needs to have a swap partition, see manpage,
although not sure if we can just use whatever with no risk

	# mount -t mfs -o rw,nodev,nosuid,-s=16G /dev/sd0b /usr/ports/pobj
	# mount -t mfs -o rw,nodev,nosuid,-s=16G /dev/sd0b /usr/ports/distfiles

- possibly even:

	# mount -t mfs -o rw,nodev,nosuid,-s=16G /dev/sd0b /usr/ports/packages

- then re-run:

	# make fix-permissions
