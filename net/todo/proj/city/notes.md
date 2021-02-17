# city(1) unstructured notes

=> initially, fuck it, random stats, we want the framework
	we won't put everything in either
we lack timing info (upgrades, builds) and higher level upgrade costs
	so set the same ones everywhere for now
mills get 1 field automatically
terraforming: wood 75, pond 75, field 20, plain 50
	field doesn't need to be adjacent, but it can be converted from other terrain
swamp, unbuildable terrain (also mountains etc)
also travel time to deliver needed materials
upkeep percentage: multiply upkeep
upkeep levels: add +1 to affected counters
we'll set upkeep for 20%
build cost can actually be upgrade level0
20% prod (lvl1, 40%: *= 2)
townhall:
	upkeep (20%) f1/f2,g1/f3,g2
		60%: 5f,3g
	upgrade1 l18, s18, g90
	upgrade2 l30,s24,t15,g160
	storage/"production" 20/50(?)/100(?)
w
	upkeep (20%): f1/f2,g2,t1/f3,g4,t2
	production cost/output w1/w2/w3
		don't need tools for lvl3?
l
	upkeep (20%) f 2 / f 3 t 1 g 3 / f 4 t 2 g 5
	prod: 1:1, 1:2, w1t1:l3
	upgrade1 cost l14,s5,t2,g70
mill
	upkeep f 1 / f 2 g 1 / f 3 g 2
	u1 l20,s10,t6,g60
	prod 2/3/t1:5
fish
	upkeep f 1 / f 2 g 1 / f 3 g 2
	prod 3 → need 1 tool also
	u1 l14,s12,t5,g50
farm
	upkeep f 1 l 1 / f 2 l 1 g 2 / f 3 l 1 g 3
	u1 l12,w36,sw8,g60
	prod 1w:2c,1w:3c,2w:4c
stone
	upkeep f 1/f2 g1/f3 g2
	u1 l10,s5,g75
	prod s1/s2/t1:s4
smelter
	upkeep f2/f3,g1/f4,g2
	u1 l18,s10,t6,g80
	prod w2:i1,w2:i2,w2,t1:i4
smith
	upkeep f2/f3,g1/f4,g2
	prod: ?
wepsmith s16,i14,t14,g26/f2/f3,t1,g3/f4,t2,g4
	prod: ?
market l15,s10,g30/g1/g2/g4
	u1 l25,s25,g80
	lvl1 can sell food,wood,stone, no lumber
		each worth 1g
	lvl2 can sell l and i; s and l worth 2
	lvl3 can sell t and w, worth 5 and 10
