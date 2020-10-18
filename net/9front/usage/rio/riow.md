# riow for window management inside rio

## setup

- lib/profile (see [riow weirdness](/todo/9front/rio/riow)):

	cat /sys/lib/kbmap/us >/dev/kbmap
	rio -i riostart

- riostart

	window -hide -r 1818 1152 1920 1200 'label riow; riow </srv/riogkbd*'


## usability

- riostart scripts need changing, we need few windows per desktop wrt performance

- if we want specific environments with different namespaces,
we still have to use subrios

- the point is to be able to switch easily between contexts

- so, integrating this into rio itself wouldn't be of benefit


## tweaks

- remove everything but 'desktop' from $sticky at the top
in order to disable forced sticky windows besides desktop
number
