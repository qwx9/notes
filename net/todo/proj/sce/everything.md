## Everything needed for playable game

## Last bugs
- where do you get the default shadow color from again?
there was a pcx file or something
	. shadow files have 0x2323ff
- vspr:
	; vspr -r hatchery 00 01 02 03 03 02 01 00
	vspr: open: '/sys/games/lib/sce/hatchery.01.00.s.bit' does not exist
- muta air but still uses ground pathfinding
- blocking overwrites blocks of ground units (hatch)
- spawn point invalid, inside hatch
- air spawn should be fixed
- drawing bugs at scale??

## Units behavior
- air units in general
- spawning units
- behavior framework
- larvae
	. zerg spawn with 3 larvae THEN 4 workers (for blockmap)
	. drones spawn below
- [attack command](attack)
- gather command
- stance: attack on sight/hold/patrol

## Buildings
- build action
- build trees
- spawning buildings
	. animations
	. requirements: resources, tech
- rally points

## Map/paths
- [mineral patches](minerals)
- better maps, converting from real maps to our own format
- movement waypoints
- creep

## Input
- grab mouse and mouse scrolling, scrolling with keyboard, remove middle click
- keyboard shortcuts

## Graphics
- just use ARGB32 images everywhere?
- drw.c assumes that a shadow sprite exists?
- unit selection -> selection ellipses of varying size (on top of shadows?)
- cursors?
- hud improvements
	. multiple selection
	. display resources
	. reuse scbw graphics for this

## Sound
- sound effects
- music

## Protocol
- teams
- observers
- lobby or challenge and pass for joining
- fog of war, sight range: building sight range, set to 1 for now

## Game
- races
- win conditions
- supply
- ai
