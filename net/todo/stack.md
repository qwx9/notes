# overall todo

## master

### sba

- sba s1, s2, mr1-2
- sba td/tp
- sba prior

### dcd

- latest

### vdb

- finish base graphics shit
- finish shiny shit
- finish last vdb tp
- finish lifemap shit

### gge

- gge 1->5
- poster guidelines

### s3proj

- start on frontend

### mbioinf

- vdb shit
- sba shit
- shiny.vdb shit and remove repo
- xl
- nanostring runs2
- tk
- other shit from courses



## work

### chrab

- cross models
- latest mails



## music

### guitar

- practice picking technique and speed
- practice songs
- learn new shit
- learn basic solfege, scales, moving them around, chords

### drums

- learn all basic rudiments
- practice rudiments

### voice

- basic breathing
- vocal exercises


## jobs

- rework cv in latex
- build cv with kertex
- english cv
- cover letters
- prospect and send shit out: private/public, lausanne/geneva
- call back ens dudes



## projects

### notes

- [plumb rules](proj/notes/plumb)
- add note/mv: move note from one dir to another, clean up parent dirs
- [populate with notes](proj/notes/populate)
- check rendering with discount
- get started on underlying mechanisms

### pitch

- [notes](proj/pitch/notes)

### sce

- [rgba for shawdows](proj/sce/rgba)
- [draw lists](proj/sce/drawlists)
- [notes](proj/sce/notes)
- [pathfinding fixes and notes](proj/sce/pathfinding)
- [networking notes](proj/sce/networking)
- [clean ups](proj/sce/cleanup)
- [attack command](proj/sce/attack)
- [sprite improvements](proj/sce/sprites)
- [ofire.pcx adjustments](proj/sce/ofire)
- [mineral patches](proj/sce/minerals)
- fix halting distance using db halt values instead of shitty heuristic,
still fails sometimes
- pathfinding fucks out in rare instances (while moving?)
- start working on the client/server part
- no ipconfig -> dial: no route -> fail to launch
	* maybe don't use ip until we actually have to connect remotely,
	and shunt all the networking stack for local
- add an air unit: mutalisk (mutalid.grp):
extract it, add it to scripts, get the offsets
- decouple simulation time from client graphics
	* fixes scrolling
	* unit sprites etc still work in real time
	even on minimum speed
- it's slightly weird that units move
with their upper left corner towards the destination,
rather than the unit's center (esp. when scaling)
- move to using sigrid's clock logic;
reused in various projects: nanosec() etc in treason et al
- grab mouse and mouse scrolling, scrolling with keyboard, remove middle click
- unit selection -> selection ellipses of varying size (on top of shadows?)
- cursors?
- extraction scripts could probably use an overhaul, after we deal with sprite db
- genspr script
	. add extract script, which should do grp -s and sctile and genmap
	. add terrain sprites
	. add grp -s shit to automate all extraction (in tree)
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

### nopenopenope.net

- [rework backend](proj/nopenopenope/backend)
- [new posts](proj/nopenopenope/posts)
- [new pages](proj/nopenopenope/pages)
- automate updating site tars (+ contrib), cron or w/e
- git server
- local setup, we never did it
- make it look a bit better via just css, check newest werc's
- track on git, expose .md files?

### 9dump.net

- site using same setup as nopenopenope.net
- paste service?
- private file share
- better radios and streaming via 9pro

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



## setup

### overall

- sensible machine names
- reread fqa 7.3 and auth papers, fix our shit
- martin's vpn idea
- check out hiro's and cinap's config, ipv6, complaints wrt nat
- mail server
- rework/clean /lib/ndb and share
- check out http://github.com/cli/cli

### storage

- better music dirs organization
- cron job for radio, share, backup, extra, /sys/games/lib, etc transfers
- cron job for selective dir updates (lib/i etc)
- cron job for updating tarballs, site pages, etc.
- centralized fs resources, dedicated machine, backups, network access
- access to unix resources
- access from unix: 9pfs, drawterm, 9pro

### remote use

- remote access: vpn, 9dump imports
- 9pro setup

### 9front

- tweak theme for visibility or use sigrid's tools, esp for t60p
- kvik's rc scripts, x
- look into mycroftiv's patches again
	* modify parent namespace
	* root namespace

### unix

- make viewing shared youtube and video links easier and automated
- use youtubedr for audio downloads
- possibly, jitsi server

### u16

- proper vnc riostart



## 9front

### bugs

- igfx regression: t61p hwgc igfx no longer functions (w500 works fine)
- page doesn't display a8r8g8b8 correctly (?)
- report arm rc breakage
- debug file(1) flac failures

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
- rsam: higher sam window

### git9

- .gitignore or something
- git log -p