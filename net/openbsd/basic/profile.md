# profile and basic env setup

## ~/.profile

### basic

	export PS1="\$ "
	alias vi="vim"
	alias xterm="xterm -bg black -fg white -u8"


### PATH

	PATH=$HOME/bin:/bin:/sbin:/usr/bin:/usr/sbin:/usr/X11R6/bin:/usr/local/bin:/usr/local/sbin:/usr/games:/usr/local/plan9/bin:$HOME/.local/bin
	PLAN9=/usr/local/plan9
	export PATH PLAN9 HOME TERM


### locale

	export LANG=en_US.UTF-8
	export LC_CTYPE=en_US.UTF-8
	export LC_ALL=en_US.UTF-8
	export LC_COLLATE=en_US.UTF-8
	export LC_TIME=en_US.UTF-8
	export LC_MONETARY=en_US.UTF-8
	export LESSCHARSET=utf-8


### cvs

- this is for stable, not used in a long while

	export CVSROOT=anoncvs@ftp.hostserver.de:/cvs
