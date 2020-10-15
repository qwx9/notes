# riow for window management inside rio

- start automatically by adding to riostart:

	window 'riow </srv/riogkbd*'


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
