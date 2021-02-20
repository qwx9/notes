# overall todo

## practice of programming

### languages
- learn ocaml, try out on data manipulation problems à la dplyr?
- check out julia


## projects

### global priorities
- see programming todo's below
- gtab, sce
- pplay
- gfx editing
- 3d
- mod: sam, mothra
- fix: vncv
- debug: igfx

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
- use unionfs to bind note resources together: unionfs -m /mnt/notes p/notes2 p/notes

### city
- see [notes])(proj/city/notes)
- spawning/despawning
- input and graphics
- later: map size and huge maps
- later: go from wheat fields to industry to spaceships, automation

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

### sce
- [notes](proj/sce/notes)
- decouple simulation time from client graphics
	* fixes scrolling
	* unit sprites etc still work in real time
	even on minimum speed
- fix halting distance using db halt values instead of shitty heuristic,
still fails sometimes
- [attack command](proj/sce/attack)
- [mineral patches](proj/sce/minerals)
- start working on the client/server part
- no ipconfig -> dial: no route -> fail to launch
	* maybe don't use ip until we actually have to connect remotely,
	and shunt all the networking stack for local
- add an air unit: mutalisk (mutalid.grp):
extract it, add it to scripts, get the offsets
- it's slightly weird that units move
with their upper left corner towards the destination,
rather than the unit's center (esp. when scaling)
- move to using sigrid's clock logic;
reused in various projects: nanosec() etc in treason et al
- grab mouse and mouse scrolling, scrolling with keyboard, remove middle click
- unit selection -> selection ellipses of varying size (on top of shadows?)
- cursors?
- zerg spawn with 3 larvae THEN 4 workers (for blockmap)
	. zerg species
	. larva behavior
- drones should still spawn below hatchery? (wrt larvae)
- movement waypoints, building rally points
- implement unit behaviors in a lib.c file or something
	. either point specifically to a db name
	. or specify behaviors and attribute them in the db arbitrarily
- implement creep
- player races (and race-unspecific map starts)
- gathering command and behavior
- fog of war, sight range: building sight range, set to 1 for now
- music and sound effects
	. in their own proc?
		* schedule sound effects via channels
	. could just spawn audio/wavdec or audio/opusdec or even play(1)
- better maps, converting from real maps to our own format
- hud improvements
	. multiple selection
	. display resources
	. reuse scbw graphics for this
- spawning
	. animations
	. requirements: resources, tech
- building constraints per race
- build trees

#### small timeconsuming fixes
- [ofire.pcx adjustments](proj/sce/ofire)
- [sprite improvements](proj/sce/sprites)
- pathfinding fucks out in rare instances (while moving?)

#### refactoring/optimization
- p/sce bin
	* games/sce or sce/sce
	* sce/... tools
	* remove from /amd64 /386 /arm
	* add in profile
- [draw lists](proj/sce/drawlists)
- [clean ups](proj/sce/cleanup)
- extraction scripts could probably use an overhaul, after we deal with sprite db
- genspr script
	. add extract script, which should do grp -s and sctile and genmap
	. add terrain sprites
	. add grp -s shit to automate all extraction (in tree)

#### research
- [pathfinding fixes and notes](proj/sce/pathfinding)
- [networking notes](proj/sce/networking)


### pplay
- zooming is unbearable: at least let one use arrow keys to scroll
- check the controls
- loading the waveform stops when pausing, sucks, and not fast enough
- loop selection is stupid too
- fplay integration
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

### 3d
- starred github repos with minimal software 3d libs, etc
- aap vectors
- rodri programs
- portals
- bisqwit block-based opengl dos engine
- wolf3d engine based on sanglard's book
- doom-like engine
- tinyref tutorial

### pico
- syntax improvements
- add a -q flag to inhibit automatic plumb to page?
- add a -e flag to exit on error?
- add a way to add or preserve image offsets

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

### opl3
- find a way to encode extended opl3 registers into 1 byte
and merge with opl2

### mus
- where the fuck did we get the fucking tempo value from and why
isn't it 60*Te6/35 ??? (also where does the 0x101 value for quarter
note duration come from) ⇒ recalculate right vars

### dmid
fix for noscroll etc?

### rio
- [subrio labels on startup](9front/rio/subrio.labels)
- [riostart adjustments for riow](9front/rio/riostart)
- [riostart sessions](9front/rio/riostart.sessions)
- [riow bugs](9front/rio/riow)
- rio exit menu item confirmation
- [rio file menu too obstrusive with riow](9front/rio/riow.filemenu)
- [rio integrated with sam](9front/rio/rio+sam)
- generate /bin/riow patch
- tmux-like functionality: don't lose long jobs when killing rio, etc.

### sam
- [default windows](9front/sam/windows)
- [rsam plumbing](9front/sam/rsam.plumb)
- bug: messing with window placement and plumbing files
can cause freezing (?)
- default focus on start up should be command window
- resize -> window too small -> crash
- sam: 1,.d → /dev/snarf doesn't get the contents, normal?
- såmtörm: samterm fork with ham tweaks and other ± såm if we change shit
	* -a by default
	* color tweaks (no direct display->white/black, etc)
	* or break it down entirely and integrate with rio directly
	* scroll past screen
	* large file support
	* list other grievances...
- annoying jumps for u command, searches, where selection should be centered, not off-screen,
and shouldn't move the display if it's visible, it's super confusing

### rc
- rc: ok, fuck rc quoting: add backwards compatible quoting system
using an ascii delimiter like sigrid suggested
	* this will help with passing shit to rio -k, window, eval, etc
	* note: probably won't be merged since use will break compat with others
	but patch is available

### vncv
- vncv: match key against some fingerprint or something? i'd like to not have
  the same fucking key for the same host on different addresses
- fucking clipboard issues
	* utf8 from remote → garbled
	* add utf8 shit to /dev/snarf → blabla not in bla
	* fucking clipboard with openbsd firefox
	* input of utf8 runes eats too, try placing ê, copy paste puts an invalid rune
	* input of utf8 runes sometimes works after two tries, sometimes doesn't ever
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

### igfx
- regression: t61p hwgc igfx no longer functions (w500 works fine)
- add support for haswell vga
- figure out haswell edp link training time out ← out of haswell
laptops
- fix haswell code for x240/x250?
- gm965 gtt fix: try a kernel side implementation, with a function
called in kernel on modeset?

### sys
- reboot: hangs on u19, u20, u25 when on 2x2gb ram
- report arm rc breakage
- debug file(1) flac failures
- page doesn't display a8r8g8b8 correctly (?)
- image memory or w/e leak vncv/vt? dpms? slowdown of draw performance after a while

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
