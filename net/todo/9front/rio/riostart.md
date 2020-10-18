# riostart tweaks

- adapt to riow usage: few windows by default
- calculate default window placements based on $vgasize
- propagate default window size and placements in /env
to reuse in psam/rsam, esp if we have multiple rsam's
- rsam can then reuse these window dimensions
