# vncv+ssh

## vnc through ssh tunnel to unix server

### option 1: listen(1) + socat on remote

	; aux/listen1 -t tcp!*!5900 /bin/ssh tcp!9dump!17060 socat tcp-connect:localhost:5900 stdio
	# in another window:
	; vncv $sysname


### option 2: execnet(1) + socat on remote

	; cat <<'EOF' >bin/rc/vnc
	/bin/ssh tcp!9dump!17060 socat tcp-connect:localhost:5900 stdio
	EOF
	; chmod +x bin/rc/vnc
	; execnet
	; echo 'key proto=vnc server=exec!vnc !password=setecastronomy' >/mnt/factotum/ctl
	; vncv exec!vnc

In this case, if using a password, it must already be in factotum,
otherwise there is no prompt.
