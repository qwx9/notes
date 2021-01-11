# git/git9 notes

## shithub usage

See: http://shithub.us

	; rcpu -u $user -h shithub.us
	cpu% newrepo -d descstring reponame

Or:

	; rcpu -u $user -h shithub.us -c \
		newrepo -d desc -c mail reponame

Repos stored in _/usr/git/$user/_.

Change description by altering _.git/description_.


## multiple remotes (git9)

Simply specify multiple url= in the config.
May not be as simple on unix git(?).
