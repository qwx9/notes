# mineral patches

## TODO
- genspr: shadows are off, same formula doesn't hold for some reason
- vspr and sce fs.c assume building has team colors
	* vspr -r mineral0 00 01 02
	* vspr divides height by eight and shows only a strip of the sprite
	* fs.c exits with error, height not multiple of 8
	(also other vspr error, see [other todo](everything)
----
unit grp dx/dy		64x96
shad grp dx/dy		128x96
	formula is:
		dx - ((grpdx / 2 - width) >= 32 ? 32 : 0)
			22 - ((128 / 2 - 32) >= 32 ? 32 : 0)	≡ 22 - 32
		dy - ((grpdy / 2 - height) >= 32 ? 32 : 0)
			15 - ((96 / 2 - 16) >= 32 ? 32 : 0)	≡ 15 - 32
	unit:
		dx - (grpdx / 2 - width)
			22 - (128 / 2 - 32)	≡ 22 - 32
		dy - (grpdy / 2 - height)
			15 - (96 / 2 - 16)	≡ 15 - 32
unit grp rect	shad rect	dx,dy	shofs	correc	Δshofs	final shofs
---- min01Sha.grp
0,17:64,71	22,15:77,68	55,53	-10,-17	+0,17	-10,0	-32,-15
0,20:64,71	23,20:83,68	60,48	-9,-12	+0,20	-9,8	-32,-12
0,29:64,71	26,31:66,68	40,37	-6,-1	+0,29	-6,28	-32,-3
5,36:64,68	30,36:78,66	48,30	-2,4	+5,36	3,40	-27,4
---- min02sha.grp
0,18:64,72	24,16:79,70	55,54	-8,-16	+0,18	-8,2	-32,-14
0,25:64,72	26,21:78,70	52,49	-6,-11	+0,25	-6,14	-32,-7
0,32:63,72	26,26:84,66	58,40	-6,-6	+0,32	-6,26	-32,0
1,41:63,72	26,38:80,65	54,27	-6,6	+1,41	-5,47	-31,9
---- min03Sha.grp
0,17:63,71	27,13:82,67	55,54	-5,-19	+0,17	-5,-2	-32,-15
1,27:63,71	27,23:88,65	61,42	-5,-9	+1,27	-4,18	-31,-5
2,30:63,71	27,28:87,69	60,41	-5,-4	+2,30	-3,26	-30,-2
3,37:61,70	31,35:82,63	51,28	-1,3	+3,37	2,40	-29,5
----
	=> dx has to do with unit minx, -(32-minx)
	=> dy also seems to be -(32-miny)
; awk 'BEGIN{OFS="\t"} NR==1 || $2 ~ /.*Resource_Mineral.*/{print $2, $29, $30, $31, $32, $33, $34, $35, $36}' lib/proj/scbw/'StarCraft UnitType Data - Sheet1.tsv'
Name	Width	Height	DimensionDown	DimensionLeft	DimensionRight	DimensionUp	TileWidth	TileHeight
Resource_Mineral_Field		64	32	15	32	31	16	2	1
Resource_Mineral_Field_Type_2	64	32	15	32	31	16	2	1
Resource_Mineral_Field_Type_3	64	32	15	32	31	16	2	1
- sprite entries, stages based on resource level
- random sprite at spawn
- link to resources in db
- arbitrary non-unit spawn
- selection/no move: server: move prohibited
	* immutable obj
- add geyser and corresponding shit
- ingame error messages? scrolling messages, slide up, w/ timeout
- 4 spaces in every direction forbidding cc placement
