# sce

## implementation

- note, we can precompute vx and vy in facemobj if we only have octile
  movement


## acceleration, deceleration, turn speed
- topspeed: unit's top movement speed (no upgrades)
	note that some units have inconsistent movement
	and this value is sometimes an approximation.
	unit: pixels per frame, double
- acceleration
	=1: fixed or patterned velocity (hopping zergling)
	unit: N/256 pixels (64 -> 4 frames to reach 1px/frame)
- halting distance: distance from unit's destination to start decelerating
	unit: 1/256th of a pixel
- turning rate: how far a unit can rotate in one frame
	unit: N/256ths of a circle per frame (64 -> 4 frame to turn a full circle)


## transparency palettes

- thingies, shadows all use transparency
- we have to figure out how ofire.pcx et al are used
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
- so, use first column of ofire.gfx to set both color and alpha?
or same color and different alpha?
	* other columns look better for tscglow
	  to set both color and alpha, and write a pic with an alpha channel
- the only way to get accurate colors would be to use the palettes, same as scbw;
	  but that introduces a lot of complexity in the fs/drw code
	* you have to use grp's, since we only want color indices
	* you have to do mirror images at runtime
	* you have to do team colors at runtime (team palettes)
	* you have to keep both color indices and final colors, since
	  you don't keep a palette per pixel
	* so now you have complex drawing in several stages/layers
- so, we'll try to get a mostly-accurate alpha blending solution
	* instead of just doing first column color + transparency,
	  we'll take into account the underlying color being black
	  and do the reverse alpha blend
- scbw doesn't seem to use a linear curve for index->alpha value
	* we do logistic growth, but it kind of looks like there's
	  a third middle plateau (3 steps)

## conventions

- atk1: vs gnd, atk2: vs air, atk3: vs sea
- buildtime: wiki buildtime * 1000 / (Te9 / 8*3) * Te6 -> raw buildtime


## gfx extraction

- every set is 17 pics long (16 for right angles + 1 at 180 degrees, and
  the 15 others being mirrored versions of the right angled ones
- first and 17th pic are rightly not mirrored,
so we end up with 2 + 15*2 pics total

	awk 'BEGIN{for(i=0; i<=390; i+=17) printf "drone.grp.%05d.bit\n", i; }'

- move frames -> idle + 4 more (5 frames total)
	* offset for drone graphics: -42,-47


## terrains

- 32x32 tiles are composed of 8x8 subtiles, then grouped together in a cv5
file which sets attributes for sets of tiles


## regenerating graphics

The thingy/tscglow is necessary!
It's removed afterwards.

	# clean up /sys/games/lib/sce first
	mkdir /tmp/scbw2
	cd /tmp/scbw2
	cp /tmp/scbw/unit/terran/scv*.bit /tmp/scbw/unit/terran/control*bit /tmp/scbw/unit/zerg/drone.*bit /tmp/scbw/unit/zerg/hatchery.*bit /tmp/scbw/unit/zerg/zhashad.grp*bit /tmp/scbw/unit/terran/tccShad.grp*bit /tmp/scbw/unit/thingy/tscglow.grp*bit /tmp/scbw/tileset/badlands.^(wpe vr4 vx4) .
	@{cd /tmp/scbw/unit/thingy; grp -sx ../../tileset/badlands/ofire.bit tscglow.grp && cp tscglow.grp*bit /tmp/scbw2/}
	rm /sys/games/lib/sce/*
	$home/p/sce/utils/genspr
	cp -x $home/p/sce/sce/*.db /sys/games/lib/sce/
