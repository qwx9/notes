# use RGB24 for opaque images: tileset and fixed objects, and RGBA32 for the rest

- transform shadows into images with alpha (0x7f/0x80 or w/e)
	=> revise extraction scripts
	sceass generates shadows for scv/drone
	they don't have shadow grp's
	=> use alpha (pico), etc
	others: figure out a way (pico I guess)
- adapt drw.c, fs.c (dat.h?) etc to changes: remove drawshadow, etc.
