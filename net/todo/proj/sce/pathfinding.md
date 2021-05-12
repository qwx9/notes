# pathfinding

## see also
- lib/extra/unix/pathfinding: warthog etc

## sce bugs
- click immediately right of unit
	* the problem is that the unit doesn't extend to the edge of the node
	* so, we can't select via rightmost node pixels since we don't draw there
	* but, we can click there and select a nonsensical goal
	* if it is within the unit, since we remove the unit for pathfinding,
	  the node is accessible
	* so the unit boundary checks are fucked?
	* our selection algorithm is correct
	* actually, the same happens in scbw: since our selection algo is
	  correct, we select an outer pixel that is unoccupied but still
	  marked blocked, and we try to move towards it; but, the object
	  blocking it becomes the new goal, and if the click isn't too far
	  apart of the unit, the unit doesn't budge
	* the problem is that units are not 8x8 blocks
		- drone against hatchery can be on top of the hatch
		  (on the right) and be flush against it
		- units still move if selecting outside of drawn pixels
		- movement can be very small since the resolution is 1x1
- even though we said that we don't redo pathfinding if target is an object
  which is still blocked, we redo it anyway
	* problem is once again the unit sizes vs. pathfinding grid
	* target can be blocked without us clicking on a unit, in which
	  case we decide to try to pathfind once more to check if it's
	  still blocked, which makes sense


## pathfinding to an object = pathfinding to nearest point
- jps doesn't do that
- if there's nothing around the target,
eg. no blocks immediately adjacent to it,
and mover isn't next to it:
	* if there is no block, it's simple
	* 1) extend target rectangle (r.min and r.max)
	by mover's width and height
	* 2) minimal distance to rectangle:
		Δx = max(r.min.x - p.x, 0, p.x - r.max.x)
		Δy = max(r.min.y - p.y, 0, p.y - r.max.y)
		Δ = sqrt(Δx² + Δy²)
	* 3) modify the above to calculate instead
	an offset point (Δx,Δy) which gives us the target coords
- if there is blockage, this turns into another pathfinding problem,
find nearest free spot around target
	* we could do bfs, but that won't give us path length,
	since we may have to move around shit to reach the spot,
	and another farther spot might actually be cheaper
	wrt movement cost
	* so, among all possible paths to the nodes encircling the
	target, which is closest in terms of path length?
	* jfs doesn't deal with this at all,
	it optimizes a single path

## pathfinding not on a 8x8 grid?

- units (and even buildings) are not multiples of 8x8 blocks
- scv, drone -> 23x23
- (max height of ground unit: 32)
- we therefore cannot reduce the pathfinding grid, we have to
	  use pixels everywhere
- so, objects have a block size and an effective size
	* identical except for buildings, which are aligned to
	  32 pixels (block size)
	* so, when placing spawning, we use block size (unit and
	  building alike): scv's are aligned past the cc block
	  size but are spaced with their effective block size
- we'll have to implement a closed set on top of the open one,
we can't afford to have such a huge array of pointers, most of
them will be empty too
	* jump nodes generation lends itself well to this, and
	  our sliding window hack can use any unit size, though
	  this negates a lot of the benefits of the block-based
	  symmetry breaking
	* however, we mustn't keep allocating shit, use a freelist
	  or something
	* avl trees for storing and looking up nodes seems like
	  a good idea
- but, scbw pathfinding is still done with 8x8 blocks, so wtf
- we can't just increase the nodemap (and bmap), it's already gigantic
- can we generate bmap for a certain pixel size on the fly?
- can we get multiples of 8 or w/e by projecting the units on an
isometric map? 25°
- there's a 2:1 ratio between graphics and pathfinding (?) and
12 directions instead of 8 because of it? (???)


## transform grid to let go of isometricity?

- problem: isometric terrain
- bridge -> some pixels are blocked, others not
- so, how to indicate which are blocked and which not,
how to do collision, and what the fuck
	=> store isometric textures? as in diamond shaped pics?
- might be nice to modify sprites and pathfinding to fit this...
	* problem: wall-offs, but perhaps we could just kludge that,
	  rather than everything else
- we know things are slanted exactly 25°, and that if we use that,
	  we'd get a simple square grid with straight edges instead of this mess
- unit sizes still won't be multiples of 8, but everything would benefit
	25° = 25 * (π / 180) rad
	projection = size * cos(25 * (π / 180))
		scv: 23x23 -> circa 20.8x20.8
	that's the length in pixels of one side of the diamond
	but the unit size is still basically 23x23


## anya: any-angle pathfinding

- look into anya, any-angle path finding: is the complexity higher and
  the performance lower than jps + path smoothing? obviously the paths
  would be suboptimal with just path smoothing, but who cares
- (scbw isn't anya)


## group pathfinding

- implement group pathfinding and movement (multi-agent pathfinding)
	* this exists for both jps/a∗ and anya (see recent arxiv papers)
- required for multiple selections


## scbw workarounds

- pathfinding shunts for any gathering units


## alternatives to jps

- another interesting direction: SRC-df (also from harabor)
- look into alternatives to his own algorithms too (competition
	  results)
- if we stick with jps, implement intermediate node pruning, and
	  perhaps even offline processing
	* we have to manage a variable size array of node pointers
		. we'll just keep a list around and grow it by
		  16 nodes everytime it's too small, fuck it
	* we have to bound diagonal jumps to keep them from going
	  too far (same bound as for a* search, or just a static
	  one, like 8 map tiles or w/e)
		. if we can keep going, just generate a jump
		  point where we can resume diagonal tracing
		. in which case, we might as well just restrict
		  ourselves to 64 points or less
	* we have to add code to traceback to add intermediate
	  nodes (using Δx, Δy)
	* distance limits: don't limit distance unless jump has
	  found nodes so far
- there's other competitive algorithms (SUB-TL, etc)
	look at pathfinding competitions, particularly for multi-
	agent planning


## waypoints?

- preprocess map to identify jump points (basically everything
around corners and where direction can change)
- build connected graphs based on this (if areas are disjoint)
- to update, move/remove/join jump points around unit and
adjacent blocks
- we'd then just do simple a* through the graph, adding a target
node, regardless of whether shit is inaccessible or not
- we then have no need to identify jump points online
- to get anyangle working, we need to make sure there are no
obstacles between two nodes in the graph
- what are connected are jump points with no obstacles between
them
	-> so this is the problem, determining visible points,
	but given the information that they are necessarily
	next to obstacles
- problem: unit sizes: maybe store max size of unit on each node
and offset for other sizes
	(it's actually pivot points, not jump points, since we don't
	only look at forced neighbors)
- should check a simple graph pruning strategy
	+ choke/way points or w/e
	or coupling with jps preprocessing
- look into coarse grids


## random stupid ideas

- another idea: partition map into empty rectangles
	* within them, the shortest possible distance is a
	  straight line between two outer edges
		. so even if we need to test multiple edges, we
		  don't need to look at anything in between
	* we can still do further optimizations there
		. i.e. maximal number of nodes visited, suboptimal
		  paths, perhaps only if the map exceeds a
		  partition threshold
	* critically, these graphs are per-player
		. so, client-side? the server would need to keep
		  track of positions, but not pathfinding
		. seems like a good idea in that a server
		  shouldn't do pathing for 8 clients anyway, the
		  clients should issue move requests, etc.
	* so, we need to disassociate Map and Path, and need a
	  different structure to handle Mobj stacking and
	  selection
	* the block graph should be mutable, since objects can
	  move
- allowing suboptimal paths (see wikipedia)
- maximal limit on the heuristic 
	* perhaps based on initial distance, only tolerate a
	  certain amount of increase
- limiting the total number of nodes one can visit while searching
	* this should be done anyway since we don't want paths
	   to take huge amounts of time
- preprocessing to add information to the algorithm
	* a* is built for calculating a path once on a static
	   graph between two points
- grouping nodes to make higher order graphs
	* check if there even is a path before doing pathfinding
- any preprocessing that reduces complexity
- no pathfinding in unexplored areas, just straight lines
- vectoring: remove? we always target an adjacent tile now,
so only 8 possible directions?
- no, there's 32 rotations, so current implementation is
wrong
- we already have code in place to deal with moving in a
straight line
- instead of storing each tile, store only inflection points?
- our distance heuristic is wrong, diagonal movement should
be less expensive than or as expensive as grid movement
- it is as expensive!
- brood war movement works with straight lines (vectors),
but is weird as hell
	* more approximate, for efficiency?
	* seems like we can get vectors more affine than 45°
	* there's some special behavior on end of movement
		. keep moving until momentum fully lost?
		. no, deceleration occurs prior to objective
		. so, we do have to keep track of distance to
		objective
- the problem might be that there are 32 directions, not 8
	* inflection point == angle is different
	* our heuristic must give an appropriate distance for
	all 32 directions
	* there's 32 angles, 8 directions
	* we want to reduce path to a set of direct,
	straight paths
	* maybe start from the end and group shit where angle
	is the same
	* there are 32 angles -> 360/32 == 11.25
	so, whenever the angle between the first point and
	the current point is >= 11.25, we have changed
	direction, and we can save the previous point as
	the end of the previous segment, and the current
	point as the new start
- http://www.redblobgames.com/pathfinding/grids/algorithms.html
- consider building a waypoint map from the blockmap: this
  would require an algorithm to rebuild or modify a map of
  critical/pivot/choke points that are the shortest path
  around any obstacle; the destination node is weighted
  against pivot nodes, and if there are no obstacles present,
  the destination is less costly than any pivot
	=> graph representation or something?
