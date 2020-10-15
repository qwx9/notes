# rio tricks

## environment

- /dev/winid: id number, prepended by spaces

- /dev/label: window label


## plumb /dev/text to editor

	B /dev/wsys/`{sed 's/[ 	]+//g' /dev/winid}^/text

⇒ p/rc/edit
