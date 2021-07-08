# dmap

- nodebuilder, based on idbsp
- ui
	* move screen with mb1
		- maybe have a move mode besides vect/line/sect/obj
	* mode selection via mb3
	* transformation menu via mb2: snarf, paste, zerox, etc.
	* it might be useful to look at rio and how it uses threads and
	  processes; see pike's rio slides
- structs used should ease our use rather than cater to the disk
  representation; how the files are to be laid out only matters when
  reading/writing to file
	* although, care should be taken to properly limit variables
	→ checklimits() or celthing...() or bitmask or something
		- how about a fucking cast? everything is s16int
	* editing: load into linked lists rather than arrays?
	* shorts are used throughout, we need a get16s() or something
		- some values cannot be negative, but are stored in
		  shorts in the wads
- read/write of maps: multiple files are needed: how to load those most
  effectively?
	* specify them in order, probably:
		- dmap [thing] [ldef] [sdef] [vert] [sec]
- colors: automap colors?
	* doom uses thicker lines, is this good here?
	* doom automap distinguishes impassable walls and other shit, we
	  probably should too (how?)
	* dampen the colors some
- needed features(?)
	* undo/redo: how? like sam? underlying textual representation?
	* copy/paste between instances of dmap
	* using the plumber + plumbrules to display pics with page(1)
	* error checking (possibly external)
- problems
	* (?) view correction for zoom is not perfect, still scrolling
	  sideways on extreme factors
	* redraw is too costly, unnecessarily so. we're just doing what
	  deu was doing at this point, which isn't necessary; for
	  zooming and scrolling, we can move drawing to an offscreen
	  image, which is painted on the screen when necessary,
	  transforming it if needed
		- this might complicate the code too much, test
	* why is our shit flipped? does the map format not assume (0;0)
	  is the top left corner?
	* allow zooming in to more than 1:1
- todo
	* display
		- status line(s)? (coordinates of selection, etc.)
			. use an additional window for that?
			. or just a subcanvas like paint(1) and
			  everything else
		- non impassable walls (brown)
		- linedefs with non zero flags (yellow?)
		- things of different types
	* are there any differences between map formats for different
	  versions of vanilla doom? (there shouldn't be, aside from the
	  lump names)
	* editing
	* writing to files


## see also
- see also: unofficial doom specs, textures.txt, metrics.txt, level_des*, doom.txt
- see also: idbsp (get it)
- see also: p/doomut (dmutils.zip)
- more on doomed:
	* tom hall's the doom bible has an overview of scripts and stuff used
	* http://doomwiki.org/wiki/DoomEd
	* http://planetromero.com/2006/12/apple-next-merger-birthday
	* http://planetromero.com/2010/01/gametales-cray-ymp
	* http://planetromero.com/2009/01/doom-history-1994
	* http://www.gamasutra.com/php-bin/news_index.php?story=21405
		-> game developer magazine 1994
