# replicating setups

## files

- [~/.profile](/openbsd/basic/profile)


## networking

- [wireless](openbsd/networking/wireless)


## services

- laptops: enable apmd

	rcctl enable apmd
	rcctl set apmd flags -A
	rcctl start apmd
