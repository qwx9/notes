# sprite loading and management

- in the end, we can generate mirror sprites with simple transformations
- and we could generate team colors just the same
- this would halve the number of rotation sprites, and make the pics smaller,
- all of this (might?) increase processing time
- wad-file system: load sprites on demand
	fs serves each file, yielding a data buffer on read
	so, not much time to load, just takes time to cache shit
	when necessary
	this would distribute the load a bit?
