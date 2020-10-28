# daily done

## sun 20201018

- drums
- sport
- dishes
- bought food, cooked
- voice
- guitar: technique, song practice
- guitar: song practice, excessive strain
- guitar: learned killing in the name of


## mon 20201019

- sent mail
- classes
- sport, extra time
- gardening
- drum, strain remains
- voice


## tue 20201020

- 5h classes
- 4h gardening
- sport
- drums
- voice, short
- programming: pitch


## wed 20201021

- 5h classes
- drive update
- 4h programming
- sport
- cooking, light gardening
- drums
- guitar: some technique, song practice and new shit
- water plants


## thu 20201022

- drums
- guitar
- gardening
- research: pitch tracking, guitar-related rsi


## fri 20201023

- 3+4h s3proj
- 3h reorganize, clean materials


## sat 20201024

- 1h drums
- RSI!
- improved audio setup
- 3h sorted papers
- 3h rearranged room
- reorganized music
- paid dues
- sport
- copy files over from w520, swap ssds
- repair w520 middle mouse key
- reinstall w520


## sun 20201025

- 2h30 sport
- voice
- light gardening


## mon 20201026

- rearranged room
- cleaned room
- sport
- gardening
- voice
- bug report


## tue 20201027

- 7h s3proj: d3
- 3h classes
- new d3graph repo with data from exercises
- mails pt1
- drums
- sport
- guitar, right hand
- music reorganization


## wen 20201028

- 3h classes
- replaced broken cccp by syncab using derp and awk


- p/sce bin
	. games/sce or sce/sce
	. sce/... tools
	. remove from /amd64 /386 /arm
	. add in profile
- p/pico: add to profile → /bin/pico
- p/rc: readme with description of scripts
- p/rc, p/rc2: add fn scripts
- update rc git, just add rc2 for additional scripts
- p/rc: add a script to add all the bullshit to /bin like in profile
	. with option for prefix for remote
- add p/sm2 repo? /bin/sm2/ ? might be a good idea esp if we do other algos
	. with pinyin scripts, lib/ for dicts etc
- remove cccp repo
- update $home/bin/rc/webshit
- move p/dot/fn to p/rc (update p/rcgit/fn)
	. but make most shit as scripts instead
- s3proj
- sce, pitch
- chrab
- voice
- sport
- sba, vdb
- epfl: notify that will continue until financing certain



## common

- sport
- drums
- voice
- guitar, right hand at least
- light gardening


## stack top and more random shit

- plan food better to reduce costs...
- music, playlists
- d3graph: develop the graph package idea first with d3 only, then ts then vue3 also
- sba+dcd+vdb lectures for next week
	* feed mbioinf and notes with them
- ask for latest pay stub
- ask for att m1 med at bh
- clean appt
- continue w520 and t60p setup
- replace bv9100 battery
- diy: check what other projects we had stacked before
- diy: single coil pickups
- diy: modify fender circuitry
- diy: distorsion pedal
- work on better reward circuits: limit/remove pleasure activities (food etc)
- s3proj: basic design and implementation
	* modelize entries as in nlartillot's lecture
	* reread cdc to list the shit we need to look into today and overall
		. templates
		. interactive entry
		. visualization
	* start from an end node, the phylogenetic process
	* models on branches of the trees etc should be possible
- split pitch todo into proj/stack entries
	* display
	* notes on fft
	* analysis
	* etc
- recipes: tacos, gelatinous shit
- note: figure out how to script splitting todo's
and navigating between them easily
	* just use like mothra?
	* still want to be able to edit them directly with sam => plumbable links?
	* mothra's selection sucks anyway, maybe fix it
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
- pitch: check that mic recording replicates left chan to right
- pitch: can use norm to amplify when above threshold
- pplay: shorter input -> why aren't we zeroing out the rest of the fft buffer?
- pplay: fix fft from pitch, nuke fplay
- fork pplay for guitarshit and editing, come up with a name
- pplay: once we have another program for editing, remove shit like writing short buffers
- pplay: better controls, esp looping/panning
- pplay: better performance, esp redraw (riow -> force redraw)
- pplay fork: modify controls
- mbioinf: populate, nuke vdb repo
- såmtörm: samterm fork with ham tweaks and other ± såm if we change shit
	* -a by default
	* color tweaks (no direct display->white/black, etc)
	* or break it down entirely and integrate with rio directly
- notes: integrate lectures and formatting
- notes: generate html for page or tree or pdf/whatever for page
- notes: modify discount markdown if necessary
- icrop: interactive image cropping via window size
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

## notes

- situation as of next week:
	* no sba/vdb/dcd until week after
	* looking quickly at last vdb lectures
	* sba: 3 td sessions left, then exam in 4 weeks!
	* 2 weeks after sba exam: gge and dcd exam
	* at the same time += taa, algo
	* at the same time s3proj soutenance
	* at the same time vdb/dcd/gge projects
	* 10 3h gge sessions left, 7 done
	⇒ have to go through sba+dcd+vdb as quickly as possible,
	and move on to gge
- numerical recipes has sections on linear and dynamic programming,
among others
