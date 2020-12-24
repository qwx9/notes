# bin(2): grouped memory allocation

## example idea for wrappers?

	typedef struct Buf Buf;
	struct Buf{
		void *p;
		int elsize;
		int nelem;
		Bin bin;
	};
	
	void *
	growbuf(Buf *buf)
	{
		int osize, size;
	
		assert(buf->p != nil);
		osize = buf->nelem++ * buf->elsize;
		if(osize == 0)
			return;
		size = osize + buf->elsize;
		if((buf->p = bingrow(buf->bin, buf->p, osize, size, 1)) == nil)
			sysfatal("bingrow: %r");
		return buf->p;
	}
	
	void
	initbuf(Buf *buf, int elsize)
	{
		if((buf->p = binalloc(buf->bin, elsize, 1)) == nil)
			sysfatal("binalloc: %r");
		buf->elsize = elsize;
	}


## but can't wrap it

- Bin is incomplete, we can't embed it in other structs
- can't (and shouldn't) use Bin's internals to access these
- this means that Bin and its pointer (and size) will have to be separate


## unsuitable for general growable arrays

- have to lug a Bin around everywhere now,
on top pointer to buffer and size
- if used with multiple types, have to cast ⇒ very bugprone
- this is served better by just realloc and a fixed extra buffer size


## better application? specialized use or single array

- static Bin's in separate files for specific handling
- with as little generality as possible
- no mixed types, no casting


## better application? one static bin to rule them all

- no growable arrays
- binalloc every element in place of malloc
- this acts as "buffered" allocs
- same as what we'd implement with realloc + fixed size increment


## bin(2)-inspired attempt at general growable arrays

- see hmst
- uses macros, danger zone
