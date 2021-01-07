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
