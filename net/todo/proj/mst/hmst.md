# hmst

## steps

- implement skeleton multitracking
- add instrument → add voices → output simple multitrack midi
- for gtab, generalize hmst parser to set state from the same type of file
- note.h: midi note indices (enum) and names (char), percussion
- gm.h: instrument patch number and names
- guitar.h: tuning names, later chords
- midi.c: commands, etc: append byte command to a buffer
- use these in vmst/tovmst/mkey
- write gtab
- write tohmst (also fixing tovmst?)
- obvious extension: std sheet music


## basic syntax

- one line per bar per instrument, or a command
- measure: [duration]:[note1]:[note2]:...:[noten]


## commands

- command: [code]:[args[,args]]
- t:[bpm]: set tempo
- i:[gmname],[nstrings],[tuning or name]: set instrument + tuning
	* instruments: identify by gm patch number or name
		. 31: distortion guitar
		. 26: acoustic guitar (steel)
		. bass patches, etc
	* tunings: ideally;
		. [lowest string note][modifier]
		. or highest string? e4 for estd? that way same for 7-9string
		. modifiers:
			- nothing: standard
			- d: drop tuning
		. examples
			- e2 → e std
			- d2d → drop d
- T:[a/b]: time sig
- #...: comment
- empty line skipped (but counted)


## durations

- [duration][modifier[,args]]
- 1,2,4,8,16,32,64
- u-plets: 4u3 for triplet?
- alternative: 3 → 1u3, 6 → 2u3, 12 → 4u3
	* not sure it's better,
	* somewhat less intuitive, dunno...
	* other u-plets → shit


## notes

- note: [string,fret][modifier/stroke code[,args]]
- nothing → rest
- fret: number or - (carry over)
- stroke/modifier
	* nothing → regular stroke
	* u,d: up/down picking, sweep picking and strumming
		just notation, won't affect sound really
	* p: palm mute
	* l: legato/hammer on
	* t: tap
	* L: legato/hammer off
	* h[,type]: touch harmonic
	* H[,type]: pinch harmonic
	* s: staccato
	* v[,strength]: vibrato
	* V[,strength?]: dive
	* x: mute
	* g: ghost
	* T[,speed]: tremolo
	* r: let ring
- nice: chords
	* chord library
		. 4:c3
		. 4:c3m
		. 4:c3maj7 (4:c3M7?)
		. 4:c3aug5
		etc


## examples

- rest instrument, full bar
	1
- ex 1: fermented offal discharge: triplets and sweep picking
	i:distortion guitar,6,e2
	i:electric bass (pick),4,e2
	t:120
	T:6/4
	8u3:6,15d 8u3:5,14d 8u3:4,14d 8u3:3,12d 8u3:2,12d 8u3:3,12u …
	[bass line bar 1]
- ex 2: new millenium suicide christ: palm muting, dotted note, rest
	i:distortion guitar,6,a♯2
	t:165
	T:6/4
	16:6,0p 16:6,1p 16:6,0p 8.:6,2 16:6,0p 16 16:6,0p 16 16:6,0p 16:6,1p 16:6,0p 8.:6,2

## drums

- channel 10 → drum sounds = notes 35-81
- specific note names/alias?
- specific syntax? simpler than guitarshit


## harmonics, fret noise

- gm 32: guitar harmonics?
- gm 121: guitar fret noise


## one voice per string

- since notes on different strings may be played
on same measure with a different duration
- multitrack midi seems easiest for this
- if we do condense representation to one line per bar per instrument,
we need syntax allowing such situations, instead of representing
every voice (string) on its own line
- without other visualization, writing 6 lines in parallel is
awkward; what we want is in fact what happens on every measure
across strings
- on the other hand, vmst as is isn't helpful, since one note
per line is also difficult
- this won't happen all that often, so we need to allow it, but
not necessarily add more complex mandatory syntax
- maybe just multiple ways of specifying this
	. same duration: dt:note1:note2:...
	. different duration: dt:note1 ∧duration:note2
	. carry over (same fret): ∪ or -: dt:note1:note2 dt:-:note2´
		* maybe specifying string: dt:1,14 dt:1,- or dt:1,14-
