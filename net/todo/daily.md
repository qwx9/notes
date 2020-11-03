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
- drums
- sport
- learned silly drumstick tricks
- food challenge day 1


## thu 20201029

- light cleaning
- cosmetics
- 3h gardening
- sport++
- food challenge day 2


## fri 20201030

- fender research
- synced u26b appropriately
- resized, backed up radeon winxp disk for w500
- sport++
- food challenge day 3


## sat 20201031

- 2h gardening
- merged some notes
- drums
- guitar
- sport
- cooking
- food challenge day 4


## sun 20201101

- fixed rsam namespace/plumbing issue: problem with window -m
- added plumbing to notes
- gardening
- s3proj
- more rc scripts fixes
- voice
- food challenge day 5


## mon 20201102

- 7h classes
- s3proj
- 3h epfl
- sport
- light gardening
- new challenge day 1
- food challenge day 6


## tue 20201103

- 3h classes
- 2h sba
- 5h s3proj
- installed work obsd disk
- new challenge day 2: 1
- food challenge day 7


## common

- sport
- drums
- voice
- guitar, right hand at least
- light gardening


## stack top and more random shit

- s3proj: ok, fuck
	* we need notes dir
	* problem is as complicated as it seems, we're not crazy
	* two approaches: forms and graph
	* both attractive, both complex
	* need a prototype for at least one
- openbsd notes: dpb, texlive
- cover letter
- recheck cv us/fr
- send requests
- sba: this week and another 2 left, imperative all is done this week
- dcd
- vdb
- get started on gge
- dcd: check out project guide and get started
- cv in latex
- reinstall kertex, see tlaronde mails, take notes
- cover letters in kertex
- cv in kertex
- report in kertex
- notes on latex
- u28 obsd disk: make it a work env
	* R, python, etc.
	* all the shit we'd need from the linux
	* ffmpeg and env
	* scripts
	* inkscape, gimp and the other one
	* see all installed programs + ~/s (=> some sys dir)
	* latex
	* discord et al
	* default shit to communicate with plan9 and use from there
		- drawterm
		- vnc
		- 9pfs
		- etc
	* reinstall if root isn't ffs2
	* back up first: archer7, etc => skr/skp
	* etc
- voice: breathing exercises + warm ups
- mkey seems off by one octave, jesus
	* mst as well
	* piano, piano32 are fine
	* i don't understand wtf Amavect's pianoJI is about
		looking at a chromatic tuner,
		umbraticus' piano is hitting exact frequencies
		whereas pianoJI is noticeably off
		different application? amend post
- p/sce bin
	* games/sce or sce/sce
	* sce/... tools
	* remove from /amd64 /386 /arm
	* add in profile
- sce: rgba for shadows
- pitch: basic implementation
- merge more notes...
- notes: note/mv
- notes: integrate lectures and formatting
- notes: generate html for page or tree or pdf/whatever for page
- notes: modify discount markdown if necessary
- rc: ok, fuck rc quoting: add backwards compatible quoting system
using an ascii delimiter like sigrid suggested
	* this will help with passing shit to rio -k, window, eval, etc
	* note: probably won't be merged since use will break compat with others
	but patch is available
- chrab
- p/rc: readme with description of scripts
- p/rc/fn -> rc scripts
- bindbins: option for prefix for remote
- replace lr with walk in scripts and fn, remove lr from misc
- consider doing classes for skillshare/whatever or something like it
- music, playlists
- d3graph: develop the graph package idea first with d3 only, then ts then vue3 also
- sba+dcd+vdb lectures for next week
	* feed mbioinf and notes with them
- clean appt
- continue w520 and t60p setup
- replace bv9100 battery
- script to regen cmus playlists on u26b
- fender mods: come up with a test bed so we can test things directly
	* either on the guitar itself
	* or a mock setup with a strings/pickups outside of body
	* maybe get discarded pickups to fuck around with
	* or just make one
- more notes on diy pickups etc, from bookmarks
- split pitch todo into proj/stack entries
	* display
	* notes on fft
	* analysis
	* etc
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
- pplay: spectrogram scaled to musical notes c0 - b8
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
	* scroll past screen
	* large file support
	* list other grievances...
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
