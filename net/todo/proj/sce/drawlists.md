# draw order, draw lists

- would remove the need to treat shadows differently
- use avltree or something: number is layer
	* would have to be able to repair it
		- when something moves
		- when screen moves
	* ordering: either just flat list, or a tree,
	  or multiple trees (by layer)
	* check for visible shit: just check if graphic would
	  intersect with viewport, no arbitrary limits
	* when panning, check neighbours until we reach non-visible?
	  (to not scan entire map again)
	  => same problem as before, arbitrary limits
	  rather, we need to have a global tree or list of trees,
	  and select within it
	* what do we need to know? just lower left pixel?
	  after all, if that's within the view port we have to draw it,
	  but if not, there's no reason
		- actually, lower right boundaries: we need the upper
		  right there
		- so maybe data structure that stores left or right
		  boundaries, or both? partition space or something?
- drw: visible units: use size of largest object
- things are drawn right to left, not left to right (only units, not
	  buildings?), it's weird
- optimize redraw() to reduce overdraw (if necessary)
	* possibly build a queue of shit to draw to avoid parsing map
	* or parse once, putting shit into different "priority" queues,
	  which are then concatenated
	  	* creep set
	  	* corpse set
	  	* ground shadows set
	  	* ground set
	  	* air shadows set
	  	* sprite set
	  	* air set
	* then just draw shit in order, this way we parse Map only once
	* perhaps we could just have one Mobjl per object set anyway
		. creep is per map tile, but is just a flag
		. this way, there's a clear separation between selectable
		  and non selectable sets too
		. right now we don't even distinguish between sprites and
		  air units
	* implement selection using these "visibility lists" too
