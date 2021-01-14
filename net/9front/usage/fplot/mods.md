# fplot(1) mods, pending patches i guess

## draw only some ticks

Modify fplot.c:^drawaxes\(.
For instance, for a plot '-1.1 1.1 -1.1 1.1',
to draw only three ticks at -1, 0 and 1 on both axes:

	//nx = ticks(xmin, xmax, &dx, &mx);
	nx = 3;		// nticks
	dx = 1;		// step
	mx = -1;	// start offset
	[...]
	//ny = ticks(ymin, ymax, &dy, &my);
	ny = 3;
	dy = 1;
	my = -1;
