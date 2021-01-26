# Status

## Results
- The annotated source has no syntax errors and compiles fine,
but rendering doesn't work at all
- No idea which decompiler we used last time


## cfr
- Potentially destructive: used s/([a-z])\./DD\1./-like regexp everywhere
to rename classes;
this changes some variable names as well but can be fixed easily
- Actually that was a bad idea,
short-named class vars can be clobbered,
don't do it
- cfr decompilation doesn't improve with the various options,
so using the old one
- Requires 1.8 for decomp


## Notes
- Just renaming the class files in the file
doesn't have an impact on compilation (class names),
just prevents running
- The java SE version currently used is 1.7, not 1.6
- Always open a cmd with rc,
otherwise it's just pain;
can start sam that way too;
9dporg.bat on desktop


## TODO
- Compare compilation with the various decompilers outputs:
	* 
- Check if any of them output a version with working rendering

[...]
