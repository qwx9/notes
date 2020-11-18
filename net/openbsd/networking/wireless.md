# openbsd wireless networking

- see faq on networking


## basic setup, multiple wireless networks

- don't really need to put wpa, wpaproto
- single ap: nwid ESSID wpakey KEY
- multiple ap's: join ESSID wpakey KEY, one per line
- dhcp at the end
- order: ?


## trunking

- replace dhcp with up in each if's hostname file
- set a trunk0 if:

	trunkports failover trunkport em0
	trinkport iwn0
	dhcp

- /etc/netstart will then start trunk0
- magic!
