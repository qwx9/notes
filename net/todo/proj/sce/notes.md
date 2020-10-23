# sce

## random todos

- use RGB24 for opaque images: tileset and fixed objects, and RGBA32 for the rest
	=> ok, done
	. transform shadows into images with alpha (0x7f/0x80 or w/e)
		=> revise extraction scripts
		sceass generates shadows for scv/drone
		they don't have shadow grp's
		=> use alpha (pico), etc
		others: figure out a way (pico I guess)
	. adapt drw.c, fs.c (dat.h?) etc to changes: remove drawshadow, etc.
- probably still have to re-adjust ofire alpha
	. scbw has an aura that "colors" the ground as well as the shadow etc,
	  ours is very very subtle
	. in fact it almost looks like it's a stair function,
	  not a simple logistic one: fires are high alpha, glow
	  is drastically lower, then cuts off
- further improvements to sprite loading
	. in the end, we can generate mirror sprites with simple transformations
	. and we could generate team colors just the same
	. this would halve the number of rotation sprites, and make the pics smaller,
	. all of this (might?) increase processing time
	. wad-file system: load sprites on demand
		fs serves each file, yielding a data buffer on read
		so, not much time to load, just takes time to cache shit
		when necessary
		this would distribute the load a bit?
- problem: isometric terrain
	. bridge -> some pixels are blocked, others not
	. so, how to indicate which are blocked and which not,
	  how to do collision, and what the fuck
	=> store isometric textures? as in diamond shaped pics?
	. might be nice to modify sprites and pathfinding to fit this...
		* problem: wall-offs, but perhaps we could just kludge that,
		  rather than everything else
	. we know things are slanted exactly 25°, and that if we use that,
	  we'd get a simple square grid with straight edges instead of this mess
	. unit sizes still won't be multiples of 8, but everything would benefit
		25° = 25 * (π / 180) rad
		projection = size * cos(25 * (π / 180))
			scv: 23x23 -> circa 20.8x20.8
		that's the length in pixels of one side of the diamond
		but the unit size is still basically 23x23
- cleanup
	. maybe simplify directions by having a table indexed by direction
		with x and y deltas
	. jumpdiag: we check with bload that we actually can move diagonally,
	  but we should also check, using the bytes read, that we can jump
	  straight from there at all, instead of the ofs1/ofs2 shit
		because it means that we can't jump diagonally in the first
		place
	. jumpdiag: left1/left2/Δx/Δy should be a table not a switch
	. rearrange dir defines so that we can just do 1<<θN, etc.
	. jumpeast seriously needs to be simplified somehow, it's
	  incomprehensible and there's tons of assumptions
- extraction scripts could probably use an overhaul, after we deal with sprite db
- genspr script
	. add extract script, which should do grp -s and sctile and genmap
	. add terrain sprites
	. add grp -s shit to automate all extraction (in tree)
- zerg spawn with 3 larvae THEN 4 workers (for blockmap)
	. zerg species
	. larva behavior
- drones should still spawn below hatchery? (wrt larvae)
- attack command
	. attack action and order
	. pathfinding to get in range
		* find nearest node in range and plot course
		* recalculate if target moves
	. attack frames and sprites
		* spray animation for drone attack
		* sparks animation for scv attack
		*> have to reference those
	. damage
	. death
	. death animation
	. attack-move action? attack area?
- building move -> building rally point
- waypoints
- implement unit behaviors in a lib.c file or something
	. either point specifically to a db name
	. or specify behaviors and attribute them in the db arbitrarily
- implement creep
- network protocol, client separation
- mineral patches, graphics and db
	. 4 spaces in every direction forbidding cc placement
	. tiles are 2x1 terrain tiles drawn on top of rest
	. immutable mobj's
	. minerals: min0?.grp, three types depending on abundance, with
	  several variants each
	. Geyser.grp (terrain pal?)
- player races (and race-unspecific map starts)
- gathering command and behavior
- note, we can precompute vx and vy in facemobj if we only have octile
  movement
- hud improvements
	. multiple selection
	. display resources
	. reuse scbw graphics for this
- spawning
	. animations
	. requirements: resources, tech
- building
	. animations
	. constraints per race
- build trees
- network protocol and clear client/server separation
	. split client sleep etc from server
	. that way we can move the camera around etc while paused or while
	  sleeping a tic delay
- fog of war, sight
	. building sight range, set to 1 for now
- music and sound effects
	. in their own proc?
		* schedule sound effects via channels
	. could just spawn audio/wavdec or audio/opusdec or even play(1)
- better maps, converting from real maps to our own format
- networking
	. recvbuf and sendbuf for accumulating messages to flush to all clients
 	. server: spawn server + local client (by default)
 	. client: connect to server
 	. both initialize a simulation, but only the server modifies state
 - command line: choose whether or not to spawn a server and/or a client
   (default: both)
 - always spawn a server, always initialize loopback channel to it; client
   should do the same amount of work but the server always has the last word
   	. don't systematically announce a port
 - client: join observers on connect
 - program a lobby for both client and server
 	. server: kick/ban ip's, rate limit, set teams and rules
 	. master client -> server priviledges
 	. master client == local client
 	. if spawning a server only, local client just implements the lobby
 	  and a console interface, same as lobby, for controlling the server
 	. client: choose slot
- server (sim.c) keeps track of all commited paths and uses map.c pathfinding to
  recompute them when necessary; client only issues commands to change state or set
  movement, etc. (if allowed)
	. in other words, there is no need for a client blockmap, the client doesn't
	  do pathfinding
	. selection and issuing orders is done client-side
	. server should not send information on non-visible objects to the client to
	  avoid cheating, etc., but this is besides the point right now
	. so, maybe client SHOULD maintain a blockmap based on known information? and
	  it should do pathfinding to validate a command before issuing it?
	. in other words, sim.c is server only; client and server have disparate
	  views on the simulation
		* however, the server should be able to inform the client when an
		  object becomes visible or disappears -> how?
		* it has to maintain some structure to check for visibility etc to
		  inform the client of any visible changes
		* client-side equivalent of sim.c which receives messages from server
		  and updates known state
	. the client/server separation can be done later
- safer programming paradigm; in fine, we're routing network requests
	. shit should be more consistent: fbw is scaled, while fbh isn't
	. coordinate conversions via functions attached to types or something

pathfinding problems
--------------------
- click immediately right of unit
	. the problem is that the unit doesn't extend to the edge of the node
	. so, we can't select via rightmost node pixels since we don't draw there
	. but, we can click there and select a nonsensical goal
	. if it is within the unit, since we remove the unit for pathfinding,
	  the node is accessible
	. so the unit boundary checks are fucked?
	. our selection algorithm is correct
	. actually, the same happens in scbw: since our selection algo is
	  correct, we select an outer pixel that is unoccupied but still
	  marked blocked, and we try to move towards it; but, the object
	  blocking it becomes the new goal, and if the click isn't too far
	  apart of the unit, the unit doesn't budge
	. the problem is that units are not 8x8 blocks
		* drone against hatchery can be on top of the hatch
		  (on the right) and be flush against it
		* units still move if selecting outside of drawn pixels
		* movement can be very small since the resolution is 1x1
- even though we said that we don't redo pathfinding if target is an object
  which is still blocked, we redo it anyway
	. problem is once again the unit sizes vs. pathfinding grid
	. target can be blocked without us clicking on a unit, in which
	  case we decide to try to pathfind once more to check if it's
	  still blocked, which makes sense
- units (and even buildings) are not multiples of 8x8 blocks
	. scv, drone -> 23x23
	. (max height of ground unit: 32)
	. we therefore cannot reduce the pathfinding grid, we have to
	  use pixels everywhere
	. so, objects have a block size and an effective size
		- identical except for buildings, which are aligned to
		  32 pixels (block size)
		- so, when placing spawning, we use block size (unit and
		  building alike): scv's are aligned past the cc block
		  size but are spaced with their effective block size
	. we'll have to implement a closed set on top of the open one,
	  we can't afford to have such a huge array of pointers, most of
	  them will be empty too
		- jump nodes generation lends itself well to this, and
		  our sliding window hack can use any unit size, though
		  this negates a lot of the benefits of the block-based
		  symmetry breaking
		- however, we mustn't keep allocating shit, use a freelist
		  or something
		- avl trees for storing and looking up nodes seems like
		  a good idea
	. but, scbw pathfinding is still done with 8x8 blocks, so wtf
	. we can't just increase the nodemap (and bmap), it's already gigantic
	. can we generate bmap for a certain pixel size on the fly?
	. can we get multiples of 8 or w/e by projecting the units on an
	  isometric map?
		25°
	. there's a 2:1 ratio between graphics and pathfinding (?) and
	  12 directions instead of 8 because of it? (???)
- scbw is built on a square grid engine but with isometric graphics,
  instead of being a fully isometric engine and do pathing on an isometric grid;
  instead, we do pathing on a square grid and intersect isometric graphics
  can we do something about this? object projections on an isometric grid?
  adapting unit movement stats by projecting them?
- idea wrt waypoints
	. preprocess map to identify jump points (basically everything
		  around corners and where direction can change)
	. build connected graphs based on this (if areas are disjoint)
	. to update, move/remove/join jump points around unit and
		  adjacent blocks
	. we'd then just do simple a* through the graph, adding a target
		  node, regardless of whether shit is inaccessible or not
	. we then have no need to identify jump points online
	. to get anyangle working, we need to make sure there are no
		  obstacles between two nodes in the graph
	. what are connected are jump points with no obstacles between
		  them
			-> so this is the problem, determining visible points,
			but given the information that they are necessarily
			next to obstacles
	. problem: unit sizes: maybe store max size of unit on each node
		  and offset for other sizes
		  (it's actually pivot points, not jump points, since we don't
		  only look at forced neighbors)
	. should check a simple graph pruning strategy
			+ choke/way points or w/e
			or coupling with jps preprocessing
	. look into coarse grids
- look into anya, any-angle path finding: is the complexity higher and
  the performance lower than jps + path smoothing? obviously the paths
  would be suboptimal with just path smoothing, but who cares
	. another interesting direction: SRC-df (also from harabor)
	. look into alternatives to his own algorithms too (competition
	  results)
	. if we stick with jps, implement intermediate node pruning, and
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
	. there's other competitive algorithms (SUB-TL, etc)
		look at pathfinding competitions, particularly for multi-
		agent planning
	. (scbw isn't anya)
- implement group pathfinding and movement (multi-agent pathfinding)
	. this exists for both jps/a∗ and anya (see recent arxiv papers)
- pathfinding shunts for any gathering units
- multiple selection and commands
	. group pathfinding
- a* algorithm
	. we must not expose the implementation to the rest of the code, meaning
	  Path contents must not have any internal data
	. the code should be clear as to what it is doing, the algorithm must be
	  easily understandable from it
	. there are a number of mathematical constraints that we should verify
	  to know validate we are in fact calculating the optimal path most
	  efficiently
	. finally, there are speed-optimizations one can do
		* another idea: partition map into empty rectangles
			- within them, the shortest possible distance is a
			  straight line between two outer edges
				. so even if we need to test multiple edges, we
				  don't need to look at anything in between
			- we can still do further optimizations there
				. i.e. maximal number of nodes visited, suboptimal
				  paths, perhaps only if the map exceeds a
				  partition threshold
			- critically, these graphs are per-player
				. so, client-side? the server would need to keep
				  track of positions, but not pathfinding
				. seems like a good idea in that a server
				  shouldn't do pathing for 8 clients anyway, the
				  clients should issue move requests, etc.
			- so, we need to disassociate Map and Path, and need a
			  different structure to handle Mobj stacking and
			  selection
			- the block graph should be mutable, since objects can
			  move
		* allowing suboptimal paths (see wikipedia)
		* maximal limit on the heuristic 
			- perhaps based on initial distance, only tolerate a
			  certain amount of increase
		* limiting the total number of nodes one can visit while searching
			-> this should be done anyway since we don't want paths
			   to take huge amounts of time
		* preprocessing to add information to the algorithm
			-> a* is built for calculating a path once on a static
			   graph between two points
		* grouping nodes to make higher order graphs
			-> check if there even is a path before doing pathfinding
		* any preprocessing that reduces complexity
		* no pathfinding in unexplored areas, just straight lines
- pathfinding improvements
	. vectoring: remove? we always target an adjacent tile now,
	  so only 8 possible directions?
	. no, there's 32 rotations, so current implementation is
	  wrong
	. we already have code in place to deal with moving in a
	  straight line
	. instead of storing each tile, store only inflection points?
	. our distance heuristic is wrong, diagonal movement should
	  be less expensive than or as expensive as grid movement
	. it is as expensive!
	. brood war movement works with straight lines (vectors),
	  but is weird as hell
		. more approximate, for efficiency?
		. seems like we can get vectors more affine than 45°
		. there's some special behavior on end of movement
			* keep moving until momentum fully lost?
			* no, deceleration occurs prior to objective
			* so, we do have to keep track of distance to
			  objective
	. the problem might be that there are 32 directions, not 8
		. inflection point == angle is different
		. our heuristic must give an appropriate distance for
		  all 32 directions
		. there's 32 angles, 8 directions
		. we want to reduce path to a set of direct,
		  straight paths
		. maybe start from the end and group shit where angle
		  is the same
		. there are 32 angles -> 360/32 == 11.25
		  so, whenever the angle between the first point and
		  the current point is >= 11.25, we have changed
		  direction, and we can save the previous point as
		  the end of the previous segment, and the current
		  point as the new start
- pathfinding optimizations
	. http://www.redblobgames.com/pathfinding/grids/algorithms.html
	. consider building a waypoint map from the blockmap: this
	  would require an algorithm to rebuild or modify a map of
	  critical/pivot/choke points that are the shortest path
	  around any obstacle; the destination node is weighted
	  against pivot nodes, and if there are no obstacles present,
	  the destination is less costly than any pivot
		=> graph representation or something?
- sce.db conventions
	. atk1: vs gnd, atk2: vs air, atk3: vs sea
	. buildtime: wiki buildtime * 1000 / (Te9 / 8*3) * Te6 -> raw buildtime
- graphics frame extraction
	. every set is 17 pics long (16 for right angles + 1 at 180 degrees, and
	  the 15 others being mirrored versions of the right angled ones
	  => first and 17th pic are rightly not mirrored, so we end up with 2 + 15*2
	  pics total
	  awk 'BEGIN{for(i=0; i<=390; i+=17) printf "drone.grp.%05d.bit\n", i; }'
	  => move frames -> idle + 4 more (5 frames total)
	  offset for drone graphics: -42,-47
- terrains
	. 32x32 tiles are composed of 8x8 subtiles, then grouped together in a cv5
	  file which sets attributes for sets of tiles
- acceleration, deceleration, turn speed
	. topspeed: unit's top movement speed (no upgrades)
		note that some units have inconsistent movement
		and this value is sometimes an approximation.
		unit: pixels per frame, double
	. acceleration
		=1: fixed or patterned velocity (hopping zergling)
		unit: N/256 pixels (64 -> 4 frames to reach 1px/frame)
	. halting distance: distance from unit's destination to start decelerating
		unit: 1/256th of a pixel
	. turning rate: how far a unit can rotate in one frame
		unit: N/256ths of a circle per frame (64 -> 4 frame to turn a full circle)
- transparency palettes
	. thingies, shadows all use transparency
	. we have to figure out how ofire.pcx et al are used
		* one ofire.pcx per tileset, 256x63 palette
		* actual compositing or just pixel overwrite with a palette color?
		* shadow is just 0.5 alpha (?)
		* in what circumstances would a different brightness level be needed?
		* for(i in *.grp) echo -n $i ' ' && grp -s /tmp/scbw/SC_Unit_Palette.pal $i |[2] awk '{if($1 > m) m = $1}END{print m}'
		* for(i in *.grp) echo -n $i ' ' && grp -s /tmp/scbw/SC_Unit_Palette.pal $i |[2] awk 'BEGIN{m=255}{if($1 < m) m = $1}END{print m}'
		=> normal draw: indices 0-255
		=> composite: 0-31 dark.pcx, 0-63 ofire.pcx (44)
		=> index of palette, not palette value, then select color from this palette's color
		=> transform index to do blending
		* shadow: right now we just set previous pixel value to half (* 127/255)
		* so, with dark.pcx, index 1, maybe we just basically do that
		* ofire.pcx: the higher the index in the grp is, the lesser the transparency
			minimal index -> don't change color or slight change
		* so, we could assume that the highest index is fully opaque
		* so how does that work? possibly just replace previous index?
		* minimal index is 1, so indices go from 1 to 63
	. so, use first column of ofire.gfx to set both color and alpha? or same color and different alpha?
		* other columns look better for tscglow
	  to set both color and alpha, and write a pic with an alpha channel
	. the only way to get accurate colors would be to use the palettes, same as scbw;
	  but that introduces a lot of complexity in the fs/drw code
		* you have to use grp's, since we only want color indices
		* you have to do mirror images at runtime
		* you have to do team colors at runtime (team palettes)
		* you have to keep both color indices and final colors, since
		  you don't keep a palette per pixel
		* so now you have complex drawing in several stages/layers
	. so, we'll try to get a mostly-accurate alpha blending solution
		* instead of just doing first column color + transparency,
		  we'll take into account the underlying color being black
		  and do the reverse alpha blend
	. scbw doesn't seem to use a linear curve for index->alpha value
		* we do logistic growth, but it kind of looks like there's
		  a third middle plateau (3 steps)
- regenerating graphics
	# clean up /sys/games/lib/sce first
	mkdir /tmp/scbw2
	cd /tmp/scbw2
	cp /tmp/scbw/unit/terran/scv*.bit /tmp/scbw/unit/terran/control*bit /tmp/scbw/unit/zerg/drone.*bit /tmp/scbw/unit/zerg/hatchery.*bit /tmp/scbw/unit/zerg/zhashad.grp*bit /tmp/scbw/unit/terran/tccShad.grp*bit /tmp/scbw/unit/thingy/tscglow.grp*bit /tmp/scbw/tileset/badlands.^(wpe vr4 vx4) .
	@{cd /tmp/scbw/unit/thingy; grp -sx ../../tileset/badlands/ofire.bit tscglow.grp && cp tscglow.grp*bit /tmp/scbw2/}
	rm /sys/games/lib/sce/*
	$home/p/sce/utils/genspr
	cp -x $home/p/sce/sce/*.db /sys/games/lib/sce/
