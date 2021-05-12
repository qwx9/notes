# WEU todo

- scripts to convert to and from spred images and palettes, replacing
  wpic; use special color for transparent pixels in sprites
	- using plumb for graphics:
		. for(i in /n/tapefs/wal*.wl6) wpic -w $i \
			| plumber -id image -t image
		. open a page instance FIRST
		. error prone
- script to unpack fonts, etc., to be ready for edition
- wlmp: wl3d lump file server, serving uncompressed, converted, stripped
  lumps, with write support
	* also serve synthetic lumps representing static data? (pals,
	  etc.) could just distribute these as files instead
- wmap: map editor
- script to convert to/from dos savegames (arduous, and requires either a
  map file or static adresses for arrays)
- scripts to replace imf.c and al.c
- documentation for the commands, examples
