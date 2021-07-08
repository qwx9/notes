# sam/samterm tweaks

## usage
- see tips and tricks from umbraticus, sigrid, kvik, sam-d

## tweaks
- self-evident current selected window, always have a file window selected (last by default) unless command window is explicitely selected
- look command → overwrite search pattern
- [rsam and plumbing](9front/sam/rsam.plumb)
- just ham mods
	* default focus on start up should be command window
- different snarf buffer behavior
	* shared buffer like ham, but unhacked implementation?
	* snarffs?
	* multiple snarf buffers?
	* see discussion
- scroll past screen
- large file handling
- sam: 1,.d → /dev/snarf doesn't get the contents, normal?
- unnecessary/stupid jumps
	* u, /, ?: selection on screen → shouldn't jump, otherwise fucking center the line
	* pipes: ,|cmd: shouldn't really shift to the top
- annoying jumps for u command, searches, where selection should be centered, not off-screen,
and shouldn't move the display if it's visible, it's super confusing
- column wrapper script usable from sam, like gq in vim
- windowing
	* default command window and new window placement
		. sam.save, but not just on error?
		. see umbraticus' stuff
	* simple tiling implementation
		. not guided by command window placement
		. split window, stack windows
			* could even have file lists per stack, not one huge one
			* point or set to where a new window should stack
			* pane/window commands
			* save/restore positions like with sam.save
			* check out wmii
		. don't fuck shit up when resizing
- too many files open → menu is super awkward
	* multiple menus?
	* folding? depending on context? subdir? alpha order?
- rlook
- scrolling command window
- fork naming:
	* såm/såmtörm/samuil
	* suckterm/sadterm/shiterm/snarkterm/sodterm, sicterm
	* sidsucker/sidterm, siouxterm, scottie, satan/saterm
	* sauerterm/scabterm/scaterm
	* schlepterm/schmuckterm
	* sisyph*, tantal*, solipsism
	* bref
- keyboard shortcut for / or ? command or look?
- X for opened files only
- alt menu with opened files only (not all B'd)
- file selection idea: hold rmb and start typing pattern to filter menu by
	* command equivalent?
- highlight all
- toggle -i or -a
