# mineral patches

## TODO
- genspr: shadows are off, same formula doesn't hold for some reason
- vspr and sce fs.c assume building has team colors
	* vspr -r mineral0 00 01 02
	* vspr divides height by eight and shows only a strip of the sprite
	* fs.c exits with error, height not multiple of 8
; grp -s /sys/games/lib/scbw/SC_Unit_Palette.pal min01.grp
64 96
0,17	64,71
0,20	64,71
0,29	64,71
5,36	64,68
; grp -s /sys/games/lib/scbw/SC_Unit_Palette.pal min01Sha.grp
128 96
22,15	77,68	55,53		-10,-17	+0,17		-10,0		(-32,-15)
23,20	83,68	60,48		-9,-12	+0,20		-9,8		(-32,-12)
26,31	66,68	40,37		-6,-1	+0,29		-6,28		(-32,-3)
30,36	78,66	48,30		-2,4	+5,36		3,40		(-27,4)
- sprite entries, stages based on resource level
- random sprite at spawn
- link to resources in db
- arbitrary non-unit spawn
- selection/no move: server: move prohibited
	* immutable obj
- add geyser and corresponding shit
- ingame error messages? scrolling messages, slide up, w/ timeout
- 4 spaces in every direction forbidding cc placement
