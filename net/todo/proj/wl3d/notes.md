# wl3d todo

## Bugs
- sound cracks up when changing the music track, and during sfx playback,
  and is infuriating
	* imf cracks seem inevitable and happen on hw too; the ones on
	  loop are due to the lump containing reset shit at its start
	* al cracks are fmopl.c's fault, and anything based on it (like
	  dosbox and wolf4sdl) have them, but hw does NOT (at least not
	  the most annoying ones)
	* possible hack: if the clean up shit in each imf is the same or
	  of the same length, just skip it on loop; another idea is to
	  skip and delays during this
	* maybe it's also caused by interpolation? (cracking at start
	  of sfx)
- performance: reaching 70Hz is hard esp. with scaling
	* nothing is really done to adapt to this, besides processing
	  multiple tics at once in gstep() or skipping steps during
	  fades
	* doom performs better and sound doesn't cut up (as much) at this
	  point (yeah but 35Hz)
		- warning: comparison with patched doom on u14
	* timing is problematic, can't get tprof to give good results,
	  and prof smears everything with proccreate taking 95% cputime
	* we can't really even get any reliable numbers before we get
	  the raycaster complete with scaling AND playable game, because
	  of all the drawing, and the different framerate at demos
	  (1 of every 4 tics is stepped through in rendering)
	* sound cuts up when below 70hz since step() takes too long
		- also the case with the emulators
	* possibility (if doom is a good example for this problem):
	  game logic at 70Hz, draw at 35Hz; our screens can generally
	  only handle 60Hz at most anyway
	* possibility: partial flushes (viewarea, etc.)
- not having sfxon (opl2 emu) has side effects
	* wl6 demo2 desync because of waiting for Sgatling to finish
	  playing
- snd: if sound is enabled, sndstep always writes a full buffer to sfd,
  and opl2 always go through every operator; this takes a lot of time
- pcm: the upsampling algorithm is overkill, given that we always
  process ≤ 100 samples at the same Fs and Fs' (also cargo cult)
	* it doesn't take much time to process all samples per step even
	  on u20, but that doesn't make it right
- fadein/fadeout differences: ref does n steps, doing 0 to n-1, then as
  part of the next frame, copies the final palette (reference or solid
  color) to the current palette, after which it doesn't wait for vbl;
  we do n steps going from 1 to n, where n calculates the final step
  color just fine (verified with assert(2)), and step0 is just the
  original color or palette; there shouldn't be a difference in timing
  since both technically take n frames
- fizzout differences: correctly bounds width and height to one lower
- pict[] use: this sucks and is bugprone; the problem is that moving the
  pict[] indirection to pic() requires adding practically 100 members to
  each array, and correcting all indices in dat.h, so both suck, but this
  one is at least brief; just ALWAYS use pict[P...] when invoking a pic
- Obj .tl and .tx,.ty point to the same thing and always have to be in
  sync; this is bugprone and redundant, however both are useful in
  different circumstances
- on demo playback with different sound settings, cam() stuff is shunted
- Sgatling and Smg: there's an annoying clicking right at the start of
  the sfx, which shouldn't happen if the sfx is digital: bug in
  resampling?
	* also with Spistol
	* does not happen in ref; wpcm clicks for the first sound only
	→ interpolation issue?
	* check when setvol is skipped
	* if we can't figure this out, just leave it for later and move
	  on with the commit
	* this existed already on the previous commit
- possible source of desyncing: usage of double instead of float in
  launch (a priori was fine, but...); also cam and die (not dangerous)
- sdm has its own memory size/load screen


## Ref bugs, fixed or not
- pressing use on tiles next to a push wall result in an invalid
  reference being used as a door, and memory being written to past array
  bounds
- unmarkable objects are not cleared from the actor array on removal,
  which then contains a reference to a freed node; this node can be
  reallocated later with the reference in actorat intact; should this
  node be used for a markable object, it may cause problems
- demo desync if playback sound settings do not match recording settings
	→ example: wl6 demo 2 if sfxon=0
- demo desync if viewsize setting do not match recording settings
- spawnboss,Ofake: GShitlerdie2 dt is modified instead of GSfakedie2 dt?
- FizzFade bounds width and height too loosely and copies one extra
  scanline and one extra vertical line (no visible effect, harmless)
- unmark: n is uninitialized in SpawnStand; if none of the surrounding
  tiles check out, current tile is set to an undefined value; i guess
  could happen in incorrect maps?
- spawnstc: seems like the very last member of the array is inaccessible
  and that the practical limit of static objects is actually Nstc-1,
  since the bounds checking is done at the end rather than at the start
  of the function
- Sschbdie plays twice before even reaching cam, no idea why
- ScaleShape reads past lower bounds of sprite lump when lx is >= 32;
  this doesn't produce pixels since slinewidth is 0 for the excess
  pixels (luck?)
- wilh dead sprite out of order? have not checked effect
- normal game start: fizzlein occurs but screen is faded out → just hangs
  at black screen for 44 tics
- wl6 data: Pguy and Pguy2 are the fucking same
- items can be picked up after death during rotation
- sd[23]: since these do not have their own vga* files, the demos are
  sod's and are thus invalid
- tile8 lumps are decoded incorrectly and result in reads past allocated
  array in all common versions
- (?) double stepping in scaler generation results in excessive
  quantization when looking at walls from up close (done to save memory)
- lies in headers and comments (numbers, offsets, etc.)
- typo in ENDSTR4
- (?) zero-length savegame names are valid
- blast processing Sshoot when confirming overwriting a save game


## More notes
- our sd3 is uac version rather than original, correct that, or at least
  find out what the differences are
- MM_SortMem calls SD_StopSound
- main: save/load game: skip/ignore joystick fields
- menu: commented out a bunch of joystick shit
- wolf4sdl fucking sucks, don't hold it as a reference
	* fmopl.c has new bugs and adjusts volume (too much)
	* pcm upsampling sounds wrong
	* red/white shift palettes are generated from the transformed
	  based palette, rather than its base values, making for slight
	  deviations (± 3)
	* sleep function is total bullshit
	* sod introscn has one pixel wrong (a lot)
	* (?) raytrace functions don't quite produce the same results
	* (?) spr in wolf4sdl are zero padded at the end for alignment
	  or some shit; dunno if the code needs that
- removing ifdefs: remember that ifdef SPEAR is equivalent to ver>=SDM
  and not ver==SOD!
- txtnl/txtw will fuckout if the last character is a \n
- there are some differences with menu drawing shit, such as the cursor
  not disappearing when entering a new menu (say, quit → but it doesn't
  disappear if going there again)
- fadein steps from 0 to dt-1 (rather than 1 to dt) and then expects a
  reset() resetting the palette to be done next
- VWB_DrawPic does x &= ~7
- there's no way to do accurate 70Hz frames, since the resolution of
  sleep(2) is 1 ms (and 1000/Tb == 14.2857). so, hopefully the shit in
  wl3d.c should work on both ≥ 70Hz and below, but the real test is the
  game, since that's where it's important. the menu shit can suffer a
  bit.
- opl2: Clk might be 3579544.0, dunno where the value comes from
- IN_StartAck ignores keys already pressed, unlike csc
- game can be paused, like in dswolf
- map number shit (all 0-indexed!)
	* mapon: map % 10
	* gamestate.mapon: same as mapon
	* gamestate.episode: map / 10
	→ gm.map: just 0-indexed map number
- missile/rocket
	⇒ wl6 rocket → missile
	⇒ sod hrocket → rocket (and third rocket type: knightmissile)
- DrawWindow → box
- DrawOutline → outbox
- intro music doesn't change at the same time compared to ref, check if
  it does when it skips no frames
	* there's no load time between the different frames in the loop,
	  unlike in ref, so who cares
- sprite nums: each page is 8 sprites → 9 pages (spr1v - spr9v)
- data restr
	* actorat[][] → Tile .o
		- actorat < 256 or actorat < objlist → Tile .ot
		- stored as [x][y]
	* tilemap[][] → Tile .tl
		- stored as [x][y]
	* mapsegs[0][] → Tile .p0
	* mapsegs[1][] → Tile .p1
	* spotvis[] → tl .vis
	* loadedgame → gm .load
	* demoplayback → gm .demo
	* demorecord → gm .record
	* areabyplayer → plrarea
	* areaconnect → conarea
	* thrustspeed → oplr .v
	* scale → dscale
	* heightnumerator → dheight
	* displayofs: current drawing buffer programmed as crtc buffer
	  used by the vga card on refresh
	* screenofs: offset from draw buffer into viewable area
		vw .ofs
	* bufferofs: current page being modified
	* static obj
		- shapenum → spr
			check for nil instead of -1
		- flags → f
		- itemnumber → item
		- visspot deleted: is always visspot+(tl-tiles)
			→ &tl->vis
	* tics → Δtc
	* statetype
		- think → up
		- action → do
	* buttonstate → kon
	* buttonheld → kold
	* InitAreas → done in oinit(1)
	* doorposition → Door .dopen
	* T_Path → oupwalk (or just owalk)
	* T_Stand → owait
	* madenoise → gm.noise
	* pwallx,pwally,pwallpos,... → Door pusher
	* ylookup[i] = i * 80
- spawnguy: if patrol, x, y and areaid are not updated even if tx,ty and
  tl are: why?
- scaltab: not really saving any memory with double stepping, since we
  have a static array; also, excessive quantization from walls up close;
  hence, not doing it.
- slider for mouse sens: up/down keys set min/max with delayed steps, but
  it's useless to emulate (also might be a bug?)
- sod/sdm: new game when a game is already underway: message appears
  *after* transition to difc screen, but it sucks; the way we do it is
  simpler given our implementation of difc, and better (no flash and
  immediate)


## Cur: add high scores
- big problem: prompt() was not made modular, and is only used for save games;
needs to be rewritten partially to adapt for high scores
- scores need to be stored in /sys/games/lib/wl3d/scores.$ext
	* handle failure
	* add to wl3d(1) under FILES and elsewhere


## Next
- attempt again to offload opl2 shit to opl2(1) (why maintain several
  copies of opl2 shit when we can just use one)
- opl2: setk: o->key==0 -> bug? (same as in fmopl.c)
- possible bug in ldmap: when setting o->tl->o, there seem to be many
  cases (in sod saves) where .o is already set to something, and .o being
  overwritten multiple times; the heuristic used for this might be wrong
- positional attenuation bugs
	* effect is too shallow
	* 360 turns reveal huge steps in attenuation between angles
	* perhaps throw table in the trash and calculate attenuation
	  with trigonometry and fp
	* compare with ref
- recording demos
  	* initg: rndi = 0
	* test playback on dos (needs modified binaries)
	* cheats and quick keys disabled (or shunted)
	* terminating recording
		- menu access
		- level exit (after cam, jump, sfxwait, etc.)
	* can you carry over accross spear shunt?
	* [-r map path]
	* demo format is map[1] len[2] pad[1]; the pad byte could be
	  used for len if size exceeds 16bits; won't work on dos, but
	  whatever; just issue a warning on crossing 8192 and 65535
	  bytes; make sure third byte is ALWAYS 0 for internal lumps,
	  or manually zero it
	* have gm.record imply gm.demo somehow, or just always set both
- add menu quick keys
- f10 quick key quirks
	* resets game palette (and dmg tics etc.), it shouldn't, even
	  though ref resets it (reset in gend then again when
	  transitioning to another state)
	* resets mus, which fucks opemus up (Lgcont->cont())
- die: check if palettes get reset such as we only get a flash of red
  (if hit hard) before reset and immediate fizzin 
- revise load/save shit with cinap's pack/unpack idea
- man page corrections: look at aiju's change to acid(1) on changing
  style inside a line (or learn fucking troff)
- wl3d(6): map section: define tile and global coordinates
- on load, the first frame of the game is processed before doing fizz out
  check if this is true on dos as well
	* maybe note in wl3d(1)
- firing addition to save fields and camvis bug: all due to player
  state never changing
	* data programming → use fucking player states
	* this will avoid the need for 'firing' var and will solve camvis
	  bug (see ref)
- simplify hub shit: amalgamate redundant seqs, etc.; come up with a
  way to simplify handling (.fork Seq member, forking functions to set
  stuff, etc.)
	* some states are exact duplicates and are only used to avoid
	  having some global variable
	* wrap common cases/hacks in functions (like the qtc=0 shit)
- scaling and out() (provided this still works ok on our 380d)
	* perhaps when ingame, only update viewarea? but what about hud?
	* perhaps use XRGB32 instead of RGB24; this will speed up
	  out() (maybe)
	* perhaps have a bigger out buffer and draw once
	* perhaps use duff's device in out(), if we don't use RGB24
	* optionally or not, refresh at 35Hz at most, like doom, while still
	  updating at 70Hz
	* measure impact, only do these if they offer a significant
	  improvement; remember: on 380d, it works like shit even
	  without scaling
- spshunt: disable cheats?
- sound improvements:
	* generate more samples per sndstep when on low fps, this makes
	  the game playable on low fps, just by having sound not cut up
	* generate a bit more samples to avoid underruns?
- use opl2(1) somehow, instead of including opl2 code; it would
  basically just write commands to a pipe
- wl3d(6): describe generated palettes
- inter screens, victory, etc: check for mouse buttons 1/2/3 as well,
  like in doom (or maybe in seqs in general, incl skips, etc.)
- implement quickkeys
- verify stopmus points
	* actually, whenever PlayLoop ends
	* gm→ctl as fadeout to black starts
	* on EDup/EDsetec through switch
	* newgame: between fadeout to black and psych
	* ctl→gm: after last fadein
		- right now, transition is too brutal between ctl and gm
- style(6)
	* (haha) don't do ~a & 1, do (a & 1) == 0; do (a & m) != 0...?
	* don't be overzealous with static functions? test, ask
	* fuck this static functions first, global later shit
- wl3d(6)
	* describe compression methods for maps and graphics and huff,
	  describe planes 0 and 1
- palette changes seem too quick; maybe timing as a whole is off?
- sound menu: none only available option if /dev/audio is not available
- CheckHighScore, load/save game: text prompt and save name
	→ Lhigh, high()
- implement opemap
	* uncomment in a dos binary to see how it works
	* fuck this; no idea if it should rightly be used as a cheat or not;
	  lots of code; no point → we're supposed to have a map editor
	  instead, so to view map, fucking open it (requires wlfs and wmap)
- implement game+demo pause? (probably in kproc) → F1 (or unused F key)
- epi text handling
	* wl6 epis
	* help menu
	* read this! menu
	* wl3d(6): epi text format
- option or cheat to remove all regular or just all enemies from map (a
  la difc baby bug)?
- implement optional use turning keys to strafe?
- opl2 sfx decay: maybe have opl2.c provide a way to tell whether
  playback has ended, either on channel 0, or on all channels
	* opl2(1) also needs to keep playing until decay is over
	* this would help switching opl2 empty playback off
		- if !imfon, keep going until playback on chan0 ends
- seems fades happen too quickly (compare i.e. mscore enter/exit) than
  ref, dunno why
	→ time how long the fades take in ref
	* maybe it's just that the updates end up taking more than a
	  single tic in ref and waitforvbl often makes it wait for
	  another one?
	* when framerate is slower, it works fine (e.g. drawterm), try
	  larger screen
- we need to write some shit to synth and vizualize wave types (square,
  saw, sine, etc.) like in music synth /fm modulation; this will greatly
  help understanding wtf
	* then combinations, panning, etc.
	* application → write a standin equalizer or something, or just
	  an audiofile wave viewer
		→ like mp3dec <$f | equ >/dev/audio
		→ like mp3dec <$f | vis >/dev/audio
	* use fplot(1)
	* great example: midi.c!
- weu: perhaps wpic + unspr is not the best way; we want to:
	1) convert wl3d gfx/pal to spred spr/pal
	2) produce plan9 image from spr/pal → unspr
	3) convert spred spr/pal to wl3d gfx/pal
	* ideally 1) and 3) should be awk scripts like unspr; the files
	  are always small → wpic + towpic?
	* seems a good idea for 1) and 3) to be single scripts or
	  programs that can handle all of wall/spr/pic/pal
	* also need something like fn#wp (but also with palette arg, cf
	  sod pics)
- f1 and read this! → help menus
	* text shit will have to be implemented for epilogues anyway
- more stuff for -b: optionally fix noise leaking incorrectly to other
  sectors?
- wl3d(6) troff bugs: .SM and concatenating .R text
- performance improvement
	* use -p, prof
	* out: seems there's a lot of work to do here
		- unrolling
		- paralellism
		- do vertical scaling ourselves?
		- etc. see csapp pp. 507-..
	* opl2 seems to be a major slowdown: chan, adv, op
- wl3d(6): document compression methods
	* rlew
	* carmack compression
	* huffman
- wl3d(1) bug section: not sure that opl2 emulation's to blame for the
  cracking, it cracks a lot on real hardware too
	* it cracks a lot MORE here, wrt pcm resampling
- raytrace is a core loop, measure shit
	* find out what takes the most time in the wl3d.c loop
- simplify code
	* merge/simplify scalspr, scalvis
	* simplify *wall shit copypasta?
- fix opl2 emulation taking too much time
- fix pcm interpolation shit with an understood and simpler
  implementation
  	* $home/lib/doc/dsp/numerical.recipes.*
  	* $home/lib/doc/dsp/c.algorithms.*
  		→ also chapter on music processing towards end of book
  		  and speech processing
  		⇒ fix midi.c and dimf.c and everything else and start
  		  work on audio processing/visualisation/editing shit
  	* reference is wolf3d on the ibm; maybe record samples?
  	* the paper describing the fir method in pcmconv(1)
- make pic() safer to use by having pict shit done elsewhere
- put jukebox back? does wl6 have a jukebox (seen screenshot)?
- performance improvement tryouts
	* -h hz: sync graphics at hz Hz instead of 70 (or div)
	* use 8bpp instead of 32 somehow, either using libdraw or
	  whatever; test with 8bpp resolutions
	* doesn't quake use 8bit colors? it uses a 256 color palette
	  right? so couldn't we use the 8bpp routines it used then have
	  libdraw deal with the color conversion? can we do the same
	  here?
	→ preliminary findings indicate that m8 images being
	  immutable means that we can only change the system
	  colormap, and only on 8bit modes, not for just an
	  image (?)
	→ check if there's a performance hit when screen->chan is
	  different from an image's (defaults on say, qk1; the
	  selected one is the same as u14, where it runs great)
	* check what doom does
	* maybe have a display buffer and a draw buffer like vga pages,
	  where display buffer is output to screen asynchronously and
	  independently from the program itself, and the program flips
	  between the two at the end of render(); then the display shit
	  can be on its own proc
		→ there are functions that are supposed to output
		  directly to screen tho, others that output to all
		  buffers (StatusDrawPic, etc.)
		→ this might allow to output at the monitor's refresh
		  rate (set from command line) rather than at 70Hz
	* possibly only update part of screen image based on the range of
	  draw operations done on draw buffer
	* possibly shorten path between draw buffer and screen image
	  somehow
	* check using RGBX instead of RGB so that we just set u32int's
	  instead of stepping through bytes → does this make shit faster?
	* drw.c and other core loops: use generic optimizations and
	  compiler-specific ones, cf csapp, use prof(2)
- compare imf conversions vs. ibm
	* wl3d.enemy.around.the.corner.mp3
- fix pict usage being forced on pic() call, rather than inside pic
  itself
- csc: must be flushed somewhere in case user holds down a key
  → infinite repetition
	- see FIXME: maybe just use kbc outside of prompts
- fs: add additional checks for file size where necessary (exts?)
- other menus
	* cursor is remembered when going back to demo and back
	* view size: actual place where pants should be accessed
		- that's not how the code is structured, but the
		  effect might be desired or something
	* make sure items are deselected correctly when passing to
	  other menus
	* left/right arrow keys also work in menu?
- ingame quick keys
- bring helpscreens (read this!) back?
- write fileserver serving converted lumps from data files (for editing
  and testing)
- fs: limit artificially max lump number for ext, etc. (offsets and shit
  are hardcoded, no point in reading more than will ever be used (mods))
	* audiot → done
- instead of P0...P9, use P0+n; instead of Pa...Pz → Pa+n
	* this requires bigger changes in some shit in reference code,
	  which is why it was left for later


## Übernext
- improve opl2 emulation
	* step 1: understand the opl2 code
	* the 380d playback just sounds a lot better (has more bass, not
	  as much cracking/clipping)
	* performance-wise, it takes a significant portion cpu time
	* look at an actual opl2 chip?
- reduce or remove sound clipping/cracking
	* ref has the same issue, where sound cracks all the time: but
	  doom (ud) does not
		- ud: software volume controls; when maxed out, they do
		  produce a lot of clipping
			→ but not on 9front! (well...)
		- just dividing samples by 2 does not work
		- perhaps opl2 settings (register/channel volumes) and
		  interpolation range has to be adjusted or... something
		- need to know more on signal processing for this
		- on the other hand, doom interpolates digital sfx samp
		  differently
- new features
	* second demo file format
		- no restriction to file size (or at least one that
		  would allow more than 2 minutes of data)
		- lower constant ticrate?
		- more metadata info
			. difficulty
			. game version?
			. whatever settings may alter demo playback?
		- don't end recording on level exit
- see how doom renders shit and if it would be faster/better/simpler to
  do the same, while not affecting gameplay (demo playback)
	* haha
	* could be enabled when demo recording or playback is disabled,
	  but what's the point? we'd have to keep code for both
- once all is said and done, experiment with ways to make drawing (not
  rendering) faster
	* we've chosen one approach, there are others to check out; in
	  particular, 200 loads + 200 draws on every update might be too
	  much
	* then again, this saves some memory
	* use u32int writes/reads rather than bytes
	→ test a bunch of approaches: time spent drawing, and memory used
	* case in point: qk1. it allocates a huge uchar buf and a huge
	  image, then it loads and draws once per update. as a result,
	  neither draw nor load are anywhere near the top time consuming
	  functions (D_DrawSpans 8 → 45%!!! next are st3_fixup and
	  D_DrawZSpans, each 8-11%); then there's other shit, and memccpy
	  which might be invoked by draw shit, but it only takes <4%, on
	  nearly 1920x1080, with profiling enabled! (prof shit ~18%)
	* case in point 2: doom. the update function is one of the
	  biggest time consumers (but not the biggest). it does 200
	  loads/draws + 1 final draw every update. but: qk1 has no
	  choice since it doesn't do any scaling
	* wl3d should be a bit better than doom, since the scaling is
	  done during rendering rather than during the update, and the
	  allocimage isn't done there either.
	* check if libmemdraw can't be used instead to allow faster and
	  cleaner shit?
		- test with e.g. qk1
	* consider using RGBX32 or whatever to simplify things somewhat
	* time strain of opl2+write to sfd on every tic relative to the
	  rest, and decide whether additional measures should be taken
	  to limit how much this shit spins
- implement flushing only parts of the screen (those that have been
  updated during the frame)? like viewarea only? (yeah but pal flashes!)
- make sure mods work


## Notes on our shit
- drawing functions in rend.c assume that any pic/... given will ever
  only be completely contained in the screen: drawing offscreen, in any
  circumstance, should be considered a bug in the code (since everything
  is hardcoded anyway), rather than in the output functions
- fade colors will be different than those in reference version,
  albeit it shouldn't be by much
	* this is no longer certain since a bug was fixed in modpal...
- rnd() not used in menu shit → menu shit might not influence demo
  playback


## Reference notes
- bugs in reference code fixed: 3
	* lies in headers (numbers, offsets, etc.) → fuck em
	* TILE8 unhuffing reading past allocated buffer → fuck it
	* typo in ENDSTR4 → fixed
- differences with reference
	* all useful data files are loaded on startup, no caching or
	  memory management is really done
	* no intro screen
	* pressing a key during the PC-13 screen skips to menu rather
	  than to the title loop
	* mouse only used ingame
- drawing to vga, unchained mode (not mode 0x13)
	* four 64K planes, selected with the map mask register
	* pixels are not contiguous; px0→p0, px1→p1, px2→p2, px3→p3,
	  px4→p0, etc.
	* to plot a pixel p, select plane 1<<(p.x&3), then write color
	  value to offset p.y<<6 | p.y<<4 | p.x>>2 (or (320*y+x)/4)
		→ ylookup, since this == 80*y + x/4
		→ in ref code: ylookup[y] + (x>>2)
	* selecting more than one plane means writing the same color to
	  multiple pixels in fixed offsets (zB fast screen clearing)
	* with 16bit writes, can select map mask register in 0x3c4 and
	  write the plane mask to 0x3c5 (data reg) with (1<<p)<<8 | 2
- there appears to be a one-pixel wide vertical black strip on the left
  border of each screen, except wl6 highscore and credits
	* this isn't a drawing bug, it's the spacing between pixel
	  components on the ips panel. the rio window border is dark
	  red, so only the red component lights up. components are in a
	  bgr order. the highscore and credits screen are red, so the
	  spacing is even, but with a blue screen like those in sod, the
	  spacing between the border's last pixel's red component, and
	  the image's first pixel's blue component is perceptible and
	  shows a thin black coloumn right between border and image.
- sod title screen: seems that the image is off-center, translated to the
  upper left corner. this is replicated on dos, so the pics themselves
  are fucked? there's colored pixels offscreen, in the right border.
  fucking hackjob, what the hell.
