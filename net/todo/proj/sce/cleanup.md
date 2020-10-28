# clean ups

- maybe simplify directions by having a table indexed by direction
	with x and y deltas
- jumpdiag: we check with bload that we actually can move diagonally,
but we should also check, using the bytes read, that we can jump
straight from there at all, instead of the ofs1/ofs2 shit
	because it means that we can't jump diagonally in the first
	place
- jumpdiag: left1/left2/Δx/Δy should be a table not a switch
- rearrange dir defines so that we can just do 1<<θN, etc.
- jumpeast seriously needs to be simplified somehow, it's
incomprehensible and there's tons of assumptions
