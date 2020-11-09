# texlive

## port

- texlive packages remove tlmgr
- openbsd-ports mailing list has a few mails on this
- the stated reason for it not to be used
is that it would screw up file checksums
- this would come up at uninstall,
which we won't do outside of upgrades
- great, but you can (allegedly) use tlmgr in usermode
- so now we'd have to do everything manually just for this?
- fuck you


## modifying the port

- seems like a good idea to modify the port makefiles
and just not remove tlmgr,
then use it in usermode
- ports don't install tlmgr stuff in /usr/local/share/texmf-dist/scripts/texlive
- removing explicit checks for tlmgr from texlive/base and texlive/texmf
hasn't had any effect
- seems like just a waste of fucking time
- tlmgr is just a perl script
- one possibility is to just copy the scripts from the distribution
and leave it at that


## user install

- we can just install it locally,
using configure options from port files
