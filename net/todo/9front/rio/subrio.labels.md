# subrio labeling

- could be nice to add an option to label rio's window on start up
- or a way for label(1) to attempt it on its rio window
- a riostart script with this works:

	echo -n $label >/dev/label

- heredoc wouldn't work, the file needs to be executable
- but that's not really the issue is it?
- it's the window the rio is within that needs its label set
- the call to initdraw will set the window's label,
do we really want to set that based on an option?
seems like a weird thing to do
