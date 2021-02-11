# Status

## jd
- Take care: any time there are functions with the same arguments
but a different return type, ALL of them must be renamed to avoid conflicts!
- So safest route is to rename by hand conservatively,
only where there are conflicts
- At least the local history shit works sorf of
- Renamed all functions using the refactor→rename function,
but that fucks everything up: unassigned ambiguous functions will be mapped
pseudo-randomly, disaster
- Huge pain in the ass to fix compilation wrt variable names everywhere
- Some cases are handled better than cfr,
ie. code is unbroken aside from fucked references
- Each class variable has to be renamed,
the rest of the code refers to them with unique names
- Can use regexp to automate this somewhat,
but still need to fix many errors manually


## cfr, second attempt
- There's a lot of annoyances preventing an easy diff
- Began to diff this and the old cfr; went through all but k,t,u,v,w,x,y,z
- This one includes fixes from the other 3 decompilations
(marked in comments)
- Got that to compile
- Previous attempt actually involved debugging after run,
because of cfr bugs; need to diff both attempts...
- Merged stuff from the other decompilers in for whatever didn't decompile cleanly
- The regex was a very bad idea, must do find and replace individually instead
- Potentially destructive: used s/([a-z])\./DD\1./-like regexp everywhere
to rename classes;
this changes some variable names as well but can be fixed easily
- Actually that was a bad idea,
short-named class vars can be clobbered,
don't do it
- cfr decompilation doesn't improve with the various options,
so using the old one
- Close to being able to compile,
have to complement with other versions for some unresolved code


## cfr
- This is what the latest version is based on,
must've been easiest to repair
- The annotated source has no syntax errors and compiles fine,
but rendering doesn't work at all
- Requires 1.8 for decomp


## dj/djjad
- Seem identical even if djjad is a more recent build


## Notes
- Just renaming the class files in the file
doesn't have an impact on compilation (class names),
just prevents running
- The java SE version currently used is 1.7, not 1.6
(what? java ME is 1.3)
- Always open a cmd with rc,
otherwise it's just pain;
can start sam that way too;
9dporg.bat on desktop


## TODO
- Compare compilation with the various decompilers outputs
- Check if any of them output a version with working rendering
	* Get jd to compile, check if it works
- Once a javashit is made to work,
we should touch it as little as possible;
rather than rewriting in c,
just put a bunch of defines and shit to turn this into valid c with minimal code
(eg typedefs, #define class struct, etc.) + manifest values
