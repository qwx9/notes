# dporg stack

## textures, sprites
- extract wtexels.ibn, stexels.bin, bitshapes.bin
- decode pics, write them out

## things
- entities.db, entities.str extraction

## maps
- map bsp + str on demand loading
	. not everything at startup,
	unless we figure a way to store state separately
	. shim load gauge
- 32/256(*8)/2048(*8) grids
- map display: implement the automap!

## rendering
- render walls
- render things

## scripts
- basic scripting
- printf

## game
- playable game
- savegames

## data
- extraction script or notes
- possibly write pic decoder, but it wouldn't be super useful, so...
