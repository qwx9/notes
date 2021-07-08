# mk sucks

## notes

	Amavect ⊢ I had recently written a Makefile for a game engine written in Go. There's some complication because it uses CGo.
	Amavect ⊢ I initially did a `CONF=os make` style command, but then I was able to simplify it into a `make os` command.
	Amavect ⊢ https://github.com/Windblade-GR01/Ikemen-GO/blob/master/Makefile
	Amavect ⊢ mk does very little to improve make
	Amavect ⊢ make does a really fine job, as long as it's not auto-generated.
	Amavect ⊢ But here is where mk and make fail. What people want from a build system is a magic, no-effort, make-it-work button.
	Amavect ⊢ But my project template attempts something that make nor mk really solve in a satisfying way: using the environment to drive the build targets.
	Amavect ⊢ env vars, and obtaining file lists (via `{})
	Amavect ⊢ Ideally, users should *NOT* care about what env vars to set in order to build the target. They should only care about which target to produce. Drawterm's `CONF=os make` is deficient in this way.
	Amavect ⊢ On top of this, a ton of deps need to be installed beforehand. Perhaps not an issue on 9front, yet. YET. Perhaps this issue is only solved on the package manager level.
	Amavect ⊢ In this way, a default target for builds that try to be cross-platform is a poor design choice.
	Amavect ⊢ Don't do `CONF=os make`, do `make file-for-os`
	Amavect ⊢ Another issue of environment is the `objtype=$thing make` pattern. It is entirely a workaround of mk's limitations.
	Amavect ⊢ Ideally, I would just specify `mk 6.targ 8.targ` and not have to care about the environment. Just the target.
	Amavect ⊢ etc.

- see Amavect's _template_ repo for ideas


## alternatives
- djb-style builds: build the toolchain to build the project
- nix package management
- see sigrid's pkg(1)
- fs/ns building programs on demand based on deps specified in the ns itself
