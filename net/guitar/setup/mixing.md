# mixing setup

## current problems
- insanely noisy setup; the power supply itself amplifies the fuck out of it
- passive pickups are just a horror show because of this,
while kh602's active ones are silent

## current stack

- caline cp-02 power supply
- behringer um300
- caline cp-24 10-band pedal
- behringer xenyx 802
- behringer uca202
- beyerdynamic dt770pro
- guitar → um300 → eq → xenyx linein1
	⇒ mix output → headphones
	⇒ master output → speakers
	⇒ both equivalent right now


## mixer: xenyx 802

- for the guitar, there are two possibilities
- either plug guitar directly to effects, then to mixer on an input
- or plug just the guitar on an input and use fx send/aux return
	* gtr → line1 → fx dial at max → fx send → um300 → eq → aux return
- the first case is simple and works fine
- the second case is much more involved
- it has more points on which gain is readjusted,
and it is difficult to set it up so that the signal isn't over amplified to hell
- the mono inputs themselves have a gain booster, besides a channel volume dial
- then fx send can be dialed
- then um300 and eq can both increase gain
- then aux return has its own volume
- attempts to work all this in without any measurements proved difficult...
- it isn't readily apparent where to increase gain and where not to
- the best setting we came up with was still worse than just not using the fx loop
- so don't use it
- biggest issue with this thing is the fucking power connector
- it's super loose and falls off extremely easily
- it also has to be shut off periodically
- noise thought to originate from effect pedal fixed by "rebooting" the xenyx
- (why?)


## recording: uca202

- xenyx ctrl room out hooked up to uca202
- uca202 driven by winxp and used by audacity for input
- terminal's output is routed to xenyx line2
- output is monitored via headphones on the uca202
- wav recordings from audacity are split and transcoded to opus on 9front
- 9front usb audio doesn't have recording... yet
- otherwise there's little reason to use the winxp shit


## pedals

- tried a bunch so far
- currently best is um300 + eq
- korg tuner pedal
- looking for "better" distortion pedals is apparently pointless
- metal master and deathmetal pedals are among those regarded as (historically) the best,
and yet they sound like shit
- on the other hand, the um300 is a clone of another such pedal
- in addition, schematics for diy pedals are available,
but they are all based on one these hallmark pedals
- so, building or buying, we are unlikely to improve on much


## behringer um300

- brighter sound than the others and better range of settings
- somewhat noisy and not very durable
- like most behringer products,
this actually uses the same parts that most leet pedals use,
since behringer manufacture them
- the pedal itself is a clone of other leet pedals, like the boss mz2
- therefore it is expected that the output is rougly the same
- compared to all the other options so far, this one is best by far
- but needs the eq pedal behind it
- perhaps we should retest deathmetal and mm pedals with the right eq settings...


## digitech metal master

- more trashmetal oriented
- fits the fender fairly well


## digitech death metal

- muffled low-gain output
- distortion is nice and very "full", but eq is again ass
- doesn't sound good on its own


## behringer od300

- distortion plainly insufficient for our purposes


## behringer nr300

- attempt on a dedicated noise gate
- afaik not on par with leet noise gate pedals, but still good
- unless cabling was stupid, this sucked
- even on the lowest setting, the cut-off is noticeable and brutal
- i'd rather have noise but be able to play softly than listen to this


## digitech rp90

- our first pedal/effects processor
- capable, but noisy and resulting distorsion is unsatisfactory
- best settings ended up using eq and chorus on top of the distortion
- original power supply extremely noisy
- the processor itself is pretty noisy
- some noise gate settings exist and they all suck
