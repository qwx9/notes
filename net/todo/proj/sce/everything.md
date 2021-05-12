# Everything needed for playable game

## Game

### Units behavior
- space overlapped air units like in scbw
- spawning units
- behavior framework
- larvae
	. zerg spawn with 3 larvae THEN 4 workers (for blockmap)
	. drones spawn below
- [attack command](attack)
- gather command
- stance: attack on sight/hold/patrol

### Buildings
- build action
- build trees
- spawning buildings
	. animations
	. requirements: resources, tech
- rally points

### Map
- [mineral patches](minerals)
- geysers
- elevation
- map features (isometric): bridges, etc
- better maps, converting from real maps to our own format
- creep

### Game
- races
- win conditions
- supply, resources
- ai
- fog of war
- sight range
- unit/building upgrades

### Movement
- it's slightly weird that units move
with their upper left corner towards the destination,
rather than the unit's center (esp. when scaling)
- fix halting distance using db halt values instead of shitty heuristic,
still fails sometimes

### Pathfinding
- movenear for air
- movement waypoints


## Engine

### Pathfinding
- [finding closest free spot to target object](pathfinding)
- [current pathfinding bugs](pathfinding)
- [non 8x8 grid](pathfinding): arbitrary unit sizes
- [non octile movement](pathfinding)
- multiagent pathfinding
- pathfinding fucks out in rare instances (while moving?)

### Protocol
- teams
- multiple concurrent players
- client<>server communication
- observers
- lobby or challenge and pass for joining
- fog of war, sight range: building sight range, set to 1 for now

### Refactoring/optimization
- p/sce bin
	* games/sce or sce/sce
	* sce/... tools
	* remove from /amd64 /386 /arm
	* add in profile
- [clean ups](proj/sce/cleanup)
- move to using sigrid's clock logic (justify);
reused in various projects: nanosec() etc in treason et al

### Files
- pack game data/sprites in a more intelligent way (zip? tapefs?)
	. [sprite loading and management](sprites)


## UI

### Input
- grab mouse and mouse scrolling, scrolling with keyboard, remove middle click
- keyboard shortcuts

### Graphics
- just use ARGB32 images everywhere?
- unit selection -> selection ellipses of varying size (on top of shadows?)
- cursors?
- hud improvements
	. multiple selection
	. display resources
	. reuse scbw graphics for this
- shadow color slightly wrong;
where do you get the default shadow color from again?
there was a pcx file or something
	. shadow files have 0x2323ff
- [more accurate ofire approximation](ofire)

### Sound
- sound effects
- music


## Tools
- vspr:
	; vspr -r hatchery 00 01 02 03 03 02 01 00
	vspr: open: '/sys/games/lib/sce/hatchery.01.00.s.bit' does not exist
