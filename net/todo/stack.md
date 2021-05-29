# overall todo

## practice of programming

### languages
- learn ocaml, try out on data manipulation problems à la dplyr?
- check out julia, myrddin again, zyg


## projects

### notes
- [plumb rules](proj/notes/plumb)
- add note/mv: move note from one dir to another, clean up parent dirs
- [populate with notes](proj/notes/populate)
- check rendering with discount, pdf generation ± kertex
- get started on underlying mechanisms
- update dcd lectures from linux disk
- rewrite dcd in a more coherent and prosaic style,
this isn't very convenient for this type of info
- algo, dcd, taa + taocp → asif
- integrate lectures + formatting
- use unionfs to bind note resources together: unionfs -m /mnt/notes p/notes2 p/notes ⇒ fn or script

### dporg
- load fonts
- primitive drawing functions
- map loading
- sprite/texture loading
- renderer
- scripts
- rest of game fsm's

### city
- see [notes])(proj/city/notes)
- graphical tiles
- select tile, actions, menus, hud stats
- spawn a building: fee, upkeep, production
- stats/hud: resources + income/cost, clock, upkeep
- drw: select a building → status
- input: destroy a building
- later: map size and huge maps
- later: go from wheat fields to industry to spaceships, automation

### 3d/game programming
- aap vectors, rodri programs, deuteron's 3d, burnzez 3d?
- portals, doom-like engine with its tools
- bisqwit block-based opengl dos engine
- doityourself series, see w520:'~/tr/game'
- wolf3d engine based on sanglard's book
- tinyref tutorial
- starred github repos with minimal software 3d libs, etc
- some small mit-licensed software renderer


### sce
- [mineral patches](proj/sce/minerals)
- gather command, without pathfinding shortcuts/hacks
- build command
- spawn command
- things get really hairy once we add attacks: range, movement, pathing, etc., so: wait
- [notes](proj/sce/notes)
- [playable game stack](proj/sce/everything)


### pitch
- [notes](proj/pitch/notes)
- modify to draw a spectrogram (vertically)
- additional processing
- we want to know what notes are played in a music track
- find simple pitch shifting/tempo shifting lib
- split pitch todo into proj/stack entries
	* display
	* notes on fft
	* analysis
	* etc
- pitch: check that mic recording replicates left chan to right
- pitch: can use norm to amplify when above threshold


### wl3d
- [notes](proj/wl3d/notes)
- reimplement sound resampling, but better (see notes)
- revisit opl2 stuff → maybe fork and use opl2(1) like doom(1)
	* do the same with pcmconv? turn it into a lib?
	* see notes
- simplify fsm shit, look at dporg


### weu
- [notes](proj/weu/notes)


### pplay
- zooming is unbearable: at least let one use arrow keys to scroll
- check the controls
- loading the waveform stops when pausing, sucks, and not fast enough
- loop selection is stupid too
- fplay integration
	* memory corruption, segfaults when resizing multiple times
	* play with imf007.sod, a short track:
	pplay 567224: suicide: sys: trap: stack exception pc=0x215ce9
- basic editing
- fork into another project for all the new shit,
keep the simple visualization/player separate
- look into simple libraries for altering pitch and tempo independently
- fork pplay for guitarshit and editing, come up with a name
- pplay: shorter input -> why aren't we zeroing out the rest of the fft buffer?
- pplay: fix fft from pitch, nuke fplay
- pplay: spectrogram scaled to musical notes c0 - b8
- pplay: once we have another program for editing, remove shit like writing short buffers
- pplay: better controls, esp looping/panning
- pplay: better performance, esp redraw (riow -> force redraw)
- pplay fork: modify controls


### asif
- asif: dcd shit
	* mine directory
- asif: add pathfinding here
	* path directory
	* will contain testing algos/impl shit too?
	* path(1) would use drop-in files from asif
- asif: add simple pitch tracking?
	* dsp directory
	* fft file
	* filtering
	* windowing
	* etc
	* single file simple fft audio pitch tracker that pitch would use
- game programming theory from plantevit courses (dcd/cm.plantevit/doc)
- dcd pattern mining tp in plan9 c (also other course shit, m1 and m2)
	→ asif, etc
- numerical recipes has sections on linear and dynamic programming,
among others

### pico
- syntax improvements
- add a -q flag to inhibit automatic plumb to page?
- add a -e flag to exit on error?
- add a way to add or preserve image offsets
- bugs bugs bugs

### dmap
- make a standalone nodebuilder
	* fs.c from dmap
	* deu nodebuilder
	* or (then?) try to fix idbsp for large maps
- different colors for values in stats, make them stand out
- selection

### sch.wad
- merge latest

### setup
- see [notes](setup/global)

### deathfront
- see [notes](proj/deathfront/stack)

### sawk
- language based on awk + add hex shit + statistical functions library etc,
vector calculus (other tool probably)

### omidi
- get fixes from dmid0
- extract bank -> fmdrv.op2 for dkey

### dmid/opl3
- find a way to encode extended opl3 registers into 1 byte
and merge with opl2 + adjust/simplify dmid
	. special command?
- verify asspull workaround, see omidi etc

### mus
- where the fuck did we get the fucking tempo value from and why
isn't it 60*Te6/35 ??? (also where does the 0x101 value for quarter
note duration come from) ⇒ recalculate right vars
- old notes:
	* maybe better to write only once to stdout for piping shit ← ?
	* not checking if mcmd buffer gets too small (it shouldn't, but...) ← ?
		. midi track length is u32int, not variable length
		. variable length is 0xffffffff max in mus; max track length is
		  0xffff → so calculate proper fucking size

### rio
- [subrio labels on startup](9front/rio/subrio.labels)
- [riostart adjustments for riow](9front/rio/riostart)
- [riostart sessions](9front/rio/riostart.sessions)
- [riow bugs](9front/rio/riow)
- rio exit menu item confirmation
- [rio file menu too obstrusive with riow](9front/rio/riow.filemenu)
- [rio integrated with sam](9front/rio/rio+sam)
- generate /bin/riow patch ← what?
- tmux-like functionality: don't lose long jobs when killing rio, etc.
- rlook or something
- fork naming: rыo/ruin/riø/Ri₀/... ← ruin is nice

### samterm
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
- fork naming:
	* såm/såmtörm/samuil
	* suckterm/sadterm/shiterm/snarkterm/sodterm, sicterm
	* sidsucker/sidterm, siouxterm, scottie, satan/saterm
	* sauerterm/scabterm/scaterm
	* schlepterm/schmuckterm
	* sisyph*, tantal*, solipsism
	* bref

### rc
- rc: ok, fuck rc quoting: add backwards compatible quoting system
using an ascii delimiter like sigrid suggested
	* this will help with passing shit to rio -k, window, eval, etc
	* note: probably won't be merged since use will break compat with others
	but patch is available
	* how would that even help anything though? just char isn't enough

### vncv
- vncv: match key against some fingerprint or something? i'd like to not have
  the same fucking key for the same host on different addresses
- fucking clipboard issues
	* fucking clipboard with openbsd firefox
- vncv performance fucking BLOWS, ssh too(?), faster to vncviewer within vncv to another machine
	* vncviewer uses compression etc
	* because 9p? ssh→tmux sucks too
	* if that's it, could we import obsd's network stack somehow? drawterm?
	* hextile sucks for low speed networks!
	* why do we not have zlib compression? if it's better, implement it
		- zrle more interesting, but zlib should be trivial
			http://github.com/rfbproto/rfbproto/blob/master/rfbproto.rst
			rfc6143
			http://euccas.me/zlib
			https://github.com/amitbet/vnc2video, go package
		- tight combines zlib with (lossy) jpeg compression

### png
- support for mTRS chunks (proper implementation)

### 2600
- fix timing issues

### nes
- record and play demo files (?)

### mothra
- mod mothra to fix fucking selection shit and don't have it interfere with scroll bar

### gb
- add linking
- debug pokemon crystal ppu bugs

### gba
- gba: working gba toolchain (haha)
- gba: add linking
- doom2 apu bugs: many sounds/music fx aren't heard
- apu bugs: mario kart
	* left channel stuff's volume is too low, barely heard
- vrally bugs
- rogue spear not drawing anything and getting stuck
- ssx3, pokemon not drawing anything

### opus
- improve opusdec performance

### usb, paint
- pen stroke size with pressure for paint(1)

### doom
- ud e1nm0646 desync
- doom playerview patch: score between viewers is fucked (see d2 deathmatch demo)
	. either overwritten or 0 - score for one of the players (p2?)
	. or not incremented or decremented
- fix our chainsaw fix
- proper networking protocol

### igfx
- regression: t61p hwgc igfx no longer functions (w500 works fine)
- add support for haswell vga
- gm965 gtt fix: try a kernel side implementation, with a function
called in kernel on modeset?

### ether589
- i mean i know the adaptor is sort of broken, but it kind of works on linuks;
here it just fucks out and the driver just loses it after init,
it's unusable
	* check obsd maybe

### sys
- reboot: hangs on u19, u20, u25 when on 2x2gb ram
- report arm rc breakage
- debug file(1) flac failures
- page doesn't display a8r8g8b8 correctly (?)

## master

### mbioinf
- vdb shit, nuke vdb repo
- sba shit
- shiny.vdb shit and remove repo
- xl, nanostring, cnv, tk
- other shit from courses
- simple dcd shit
- etc


## music

### guitar
- make an actual practice routine for technique and theory and tracks

### drums
- learn all basic rudiments
- make an actual practice routine

### voice
- vocal exercises
- make an actual practice routine
- breathing, vocalization, range, accuracy, lung capacity, resonance, etc


## books, topics
- dsp
- guitar effects and wiring
- lutherie


## jobs

- reinstall kertex, latest tlaronde mails, take notes
- m1 report in kertex
- rework cv in latex
- build cv with kertex
- cover letters in kertex
- prospect and send shit out: private/public, lausanne/geneva
- countries


## irl

### software
- martial arts, both selfdefence and discipline
- check out selected articles on learning methods + re-read notes
- find out about quick memorization techniques,
and long term massive information stores
	* vidid fun stories with strong connections for quick sequences
		- this is practically sufficient, when well used,
		for memory world competetions
		- but have to learn to do it
	* spaced repetition for massive knowledge
	* others? more fun? easier? more adapted?
	* mindmap/wiki -> ?


### hardware
- soundproof closet? real black metal recording setup!
