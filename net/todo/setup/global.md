# global setup tasks

## 9front
- proper aib vnc riostart
- tweak theme for visibility or use sigrid's tools, esp for t60p
- kvik's rc scripts, x
- look into mycroftiv's patches again
	* modify parent namespace
	* root namespace
- bindbins: option for prefix for remote
- replace lr with walk in scripts and fn, remove lr from misc
- p/rc: readme with description of scripts
- p/rc/fn -> rc scripts

## namespace
- use unionfs for p/lib + $home/lib; note namespace

## storage
- better music dirs organization
- cron job for radio, share, backup, extra, /sys/games/lib, etc transfers
- cron job for selective dir updates (lib/i etc)
- cron job for updating tarballs, site pages, etc.
- centralized fs resources, dedicated machine, backups, network access
- access to unix resources
- access from unix: 9pfs, drawterm, 9pro
- we went to cwfs, might as well try fossil+venti like mycroft? (+ writeup)

## auth
- use setup similar to sekretz, sexstore doesn't help at all
- one-time key extraction/sandboxed factotums, check info on dp9ik multiple keys/host factotum
- reread fqa 7.3 and auth papers, fix our shit

## networking
- check out hiro's and cinap's config, ipv6, complaints wrt nat
- mail server
- rework/clean /lib/ndb and share
- remote access: vpn, 9dump imports
- 9pro setup

### nopenopenope.net

- [rework backend](proj/nopenopenope/backend)
- [new posts](proj/nopenopenope/posts)
- [new pages](proj/nopenopenope/pages)
- automate updating site tars (+ contrib), cron or w/e
- git server
- local setup, we never did it
- make it look a bit better via just css, check newest werc's
- track on git, expose .md files?

## 9dump
- site using same setup as nopenopenope.net
- paste service?
- private file share
- better radios and streaming via 9pro
- fix gridreg scripts
- serve git on nope, and other improvements
- http(s) access to files, esp music; separate dir/namespacing?
- export with samba/nfs for bv91 and others? 9pfs→sftp?

## vpn
- set bbb up: basic setup + wireguard, take note of packages
- set torrenting up
- rutracker et al without browser

## openbsd
- make viewing shared youtube and video links easier and automated
- use youtubedr for audio downloads
- possibly, jitsi server
- https://www.c0ffee.net/blog/openbsd-on-a-laptop/#networking

## work setup
- more professional site? 9dump.net/~qwx? but more unixy with html/css? js bullshit?
- check out http://github.com/cli/cli

## bv91
- automate backups, extend syncbv91

## repos
- cron job?
- get all repos
	* https://github.com/Plan9-Archive
	* sr.ht
	* other github
- automate repo updates for contrib via syncab/mtime and site (or remove from site)
	* alienpatch
	* asif
	* dmap
	* fplay
	* mkey
	* mst
	* notes
	* omidi
	* opl2
	* pcx
	* patch
	* peu
	* pico
	* pplay
	* qk1
	* qk2
	* qk3
	* rc
	* sce
	* sm2
	* tbs
	* u6m
	* weu
	* wl3d
