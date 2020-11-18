# serial output

## wiring: adafruit PL230 4 Pin USB to TTL serial debug cable:

	pin1 -> black (gnd)
	pin4 -> green (rx)
	pin5 -> white (tx)


## settings

- linux: /dev/ttyUSB0
- disable flow control
- bps/par/bits 115200 8N1
- output DEL for backspace
- enable local echo
	* openbsd wants it disabled

