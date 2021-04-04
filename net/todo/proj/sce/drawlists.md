# draw order, draw lists

- see rev db005316eeb552c68b6c54c7392bc119a69b12e0:

	biggest change: move obj lists to map grid rather than node grid:
	the idea was to attempt to find a better compromise between looking
	up units for drawing and selection, etc.  map objects can be bigger
	than both map and node grids, but we want minimum redundancy, so we
	only store object lists in one place.  we don't want to redraw the
	same object multiple times when scanning the drawing window, but we
	also want to be able to easily look up map objects at a given
	coordinate (client select or server map manipulation).  we could
	segment the map as with k-d trees or r-trees variants, but this
	blows up complexity for nothing.  quadtrees would still work with
	buckets of units and don't solve overlaps.
	previously, we just enlarged the search window to make sure we don't
	miss units that are farther back (x or y axis) than the given node,
	and it doesn't seem we can do better.
	besides renaming and cleaning up everything for clarity, moving
	object lists to the map grid reduces the number of nodes to scan by
	a factor of 4.  we enlarge draw window 256px top and left (largest
	unit assumed between 128 and 256px at most), and take scale into
	account.
	the problem really isn't completely solved either way: air/ground
	units can overlap (esp. flocks of air units), and there has to be
	some kind of heuristic to pick one out of all intersects.  ground
	units intersect building pixels as well.

- so, we couldn't easily do better than we already are, besides refactoring
- HOWEVER: ordering isn't top→bottom,left→right
	* Z-shape? top→bottom,right→left?
	* only units, not buildings? it's weird, have to investigate
