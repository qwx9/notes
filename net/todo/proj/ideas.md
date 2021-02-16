# project ideas/wants

- a tool that can fit a curve of any shape to an equation?

	* draw a curve -> validate -> fit as best as possible
	* maybe choose family first
- ftart: multiplayer french tarot + ai
- semprini: check out hummod.org
- icrop: interactive image cropping via window size
	* there's one on shithub now
- more advanced tool for interactive image editing/analysis
	* paint already does pixel editing and has undo/redo,
	but overloading it with more shit might be teh suck
		. can add commands like crop canvas to window
		. could add < > ^ for external commands, start
		an rc and barf the canvas to stdout
		. but, undo/redo changes?
		. maybe paint should just... paint
	* instead, could integrate paint: call it on the canvas,
	read result in, etc.
	* could integrate pico as well, both with issuing expressions as usual,
	and using predefined expressions/templates for common shit
		. another occasion to fix its fucking syntax
	* multi-image editing like in sam
	* way to generate gifs (or unix scripts like kvik's x to ffmpeg)
	* way to edit gifs, adding text etc
		. even get the common meme fonts working
	* so, interactiveness useful for interactive and visual tweaking
	like cropping, etc.
		. hell could output command for snarfing in a script
	* way to also edit/load palettes and paletted images, without
	weird program-specific formats
	* check what sigrid/hundredrabbits/gridders have done wrt image editing
	* come up with a clever name
	* like page(1): call external programs for certain operations,
	let user pipe to programs on his own,
	let user define functions doing this
		⇒ pico, resize, etc
- some program to write a plan9 image as a c byte or u32int array

- 9ds: port 9front to the 3ds (haha)
- a2, a2gs: apple 2 emulator(s) (haha)
- audiosb16: add support for sbpro (u2,u17) based on openbsd sb* shit
	. expose opl registers somehow ('#A?/opl'?)
- bruce: ted5 port
	. nuke it...?
- c3d: catacombs 3d reimplementation
- cat-v: #cat-v simulation
- cathosp: the cat-v hospital manager
- catyps: catocalypse now! monopoly clone with cats (ask khm/sl for writing
  advice)
- crawl: 3d first person dungeon crawler akin to eye of the beholder or legacy
- dave2: dangerous dave in haunted mansion reimplementation
	. note: bug: waiting at pause for too long fucks timing up completely
- ddes: dark designs 1 and 2 reimplementation
- dicom: dicom image decoder
- di2: diablo 2 reimplementation (haha)
- doom deathmatch/coop bot
- doom wads
	. uefi.wad
		* players: tuttle glenda
	. catclock.wad
		* cacodemons → catclocks throwing pjw faces
- dosemu (games/pc? games/386?); dos/386 emulator (haha)
- ds9f: nds kernel // drawterm
- dsq3: nds quake3 port
- dstar: demonstar reimplementation
	. problem: ms wavesynth shit needs emulating (how? source? soundfont?)
	. seems no one gives a fuck about ms wavesynth, even when it actually
	  sounds great for this one game :(
	. easy solution: support for mp3 playback a la dmus
- dswolf: nds wolf3d port
- edwin: a program that berates your failures
- ether589/etherelnk3: which should u17 be using again?
- etheriwl: debug 4965 (u19,u20,u25): must have worked at some point
- etherwpi or new driver: add 2915abg support
	. better: check compat with newer intel card
- fight: ruhlistic 1-on-1 fight simulation (no, tekken-like) (haha)
- foe: fallout1-2 engine reimplementation (no, really)
- fudoom: nds doom port
- gbawolf: gba wolf3d port
- gm4s: tetris gm clone
- gmst: graphical mst: program to generate standard notation documents?
  images? from mst files, either interactive (selection, loops, modifications,
  etc.), or not
- hov3d: hovertank 3d reimplementation
- keen: commander keen 1-6 engine reimplementation
- kdreams: keen dreams reimplementation
- kiddie: uplink clone
- lib/avgn: complete collection of memorable insults from the first couple
  dozen episodes
- nds: nds emulator (haha)
- nuke: unrealistic explosion simulator
- p3n: insane gta-like openworld game project (HAHAHA)
- pakfs: quake pak file server, perhaps replacing code in qk1 itself
- pcm audio utils
	. a way to modify tempo OR pitch
	. a way to guess notes for note deaf idiots like me
- pcx: finish it
- pdf/ps: working string searching somehow
- pedal: software guitar effects processor
- pico: keep extending it
	. look at pico.ps examples for shit to do
- pkmnrl: pokemon rpg with a dose of realism (haha) (to be renamed?)
	. be able to harm *people* (extortion, etc.)
	. team rocket are really 9fans
- ppc: write kernel and drivers for u15 (haha)
- puke: puke images/ps/... from ppt* files
- q1ut: quake 1 editing utils (mostly qcc and map editor)
- qk3: quake 3 port, using own implementation of a software renderer (ouch)
- qn: crpg engine akin to infinity or fallout engine
- quake 2 mod ports
- radeon: add support for rv530 (u23)
- radeon: debug for u22
- radeon: try out new driver for u23 (in /tmp/s/)
- rally: simple 3d game based on v-rally 3 on gba
- rts: real-time strategy akin to starcraft
- sam: column wrapper script usable from sam, working like gq in vim
- shwf: shadowforge reimplementation
- scum: scumm reimplementation (haha)
- sf2: midi player with sf2 support
- spd: reimplement
- svs: svs image decoder, or a way to use 20gb tiff files
- syndopl: syndicate midi to imf decoder (or just the instruments)
	. does syndicate even store music as midi
- tbs: turn based strategy engine with multiplayer support and implementations
  of ancient empires 2, advance wars dual strike and catyps
	. tbs3: homm3? wesnoth?
- tscb: speech to text implementation (haha)
- u0-5: ultima 0-5 reimplementation
	. u1: probably want reimplementation of the dos/ega version, and c64
	  fonts are better (the msods ui looks like shit in general) (check
	  out gog)
- uuw: ultima underworld reimplementation
- wad(6): description of wad files
- war: lotr inspired high fantasy total war-like rts with cat-v theme
	. cats; ranks with increasing number of cats on head, marge simpson
	  hair with cat hats for wizards
	. mechanics similar to m2tw
- wlfs: wl3d data file server and converter
- wlmap: wl3d map editor, using wlfs
	. possibly call this bruce
- wrth: wraith reimplementation
