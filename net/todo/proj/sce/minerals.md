# mineral patches

## TODO
- gather entry in db linking minerals to resource
	. *default* amount of 1500, geysers 5000 (but can actually be higher or lower in map)
		* the three fields are identical except for sprites
		* amount thresholds: ≥ 750 ≥ 500 ≥ 250
			/2 /3 /6
	. so define thresholds and corresponding state frames
- spawn: cannot have invalid amount <= 0
- random sprite at spawn(?) -> check
- add geyser and corresponding shit
	. size is 16,8
- 4 * 4x4 spaces in every direction forbidding cc placement
- gather unit command/capability
- unspawn when amount reaches 0 (minerals only)

https://makingcomputerdothings.com/brood-war-api-the-comprehensive-guide-unit-movement-and-worker-behavios/
https://makingcomputerdothings.com/?s=pathfinding
https://makingcomputerdothings.com/tag/bwapi-the-comprehensive-guide/
lib/extra/unix/bwapi/	← lgpl/gpl3, so cannot read code
