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


## common

- sport
- drums
- voice
- guitar
- light gardening


## stack top and more random shit

- s3proj: basic design and implementation
	* modelize entries as in nlartillot's lecture
	* reread cdc to list the shit we need to look into today and overall
		. templates
		. interactive entry
		. visualization
	* start from an end node, the phylogenetic process
	* models on branches of the trees etc should be possible
- sba+dcd+vdb lectures for next week
- no voice practice until we've watched the basic exercises section
- epfl
- more samples and verbose output for sigrid
- sce: convert shadows to argb images
- pitch: check that mic recording replicates left chan to right
- pitch: can use norm to amplify when above threshold
- asif: bitwise round up to nearest power of two (from fplay)
- asif: single-file complex and real fft (from pitch)
- pplay: shorter input -> why aren't we zeroing out the rest of the fft buffer?
- pplay: fix fft from pitch, nuke fplay
- fork pplay for guitarshit and editing, come up with a name
- pplay: once we have another program for editing, remove shit like writing short buffers
- pplay: better controls, esp looping/panning
- pplay fork: modify controls
- mbioinf: populate, nuke vdb repo
- såmtörm: samterm fork with ham tweaks and other ± såm if we change shit
	* -a by default
	* color tweaks (no direct display->white/black, etc)
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

- numerical recipes has sections on linear and dynamic programming,
among others
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
