# networking: general notes

## decoupling


- requests: move
- client requests a move, it either works or it doesn't, period
- atexit: send quit to all remote clients if master...

a single packet should suffice for any message between two hosts
	(including master to each client)

listen: we opened a connection, but we haven't accepted a player or anything,
we wait for an actual request from the client on the open connection


local server by default; *argv != nil → remote; -L: listen on port -P


give units unique identifiers between client and server
so we don't guess number of units for other teams,
we get a large enough range instead
8 teams → 32 - 3 bits (4 if observer, but that has no units)

don't execute any command client side until server has signed off?
"prediction": execute before server gives the command, but be ready to resync


- cl/drw: Mobj *block shouldn't be set from drw, but from the server (race condition)
	we need another, better way to get a unit (or uid) from a pixel value/path node
	simple path matrix with unit uid? instead of visbuf/etc?
- associative data structure for mo uid's, or a simpler way
	. lists by subcategory of object and/or team? (selected by bitfield in uid?)
	. simple hash table?
	. more complex hash table, multiple functions? cf algo
	. growing array with nonreusable buckets?
- local<>remote communication
	. sync and errors
	. if local and client input, can just reject it
	. if remote to local, something is fucked, unsynced state
	. if master, send a syncing Rerror, invalid move: mo position, etc
	. have to think about what can go wrong and how to recover
	. also remote sending state to local (spawns etc)
- protocol
	. tmove: no error and not local only: prepare a Tpath to send through the wire
	. tquit/drop/whatever: drop/detach remote client

- server only: listen for network events, as before
	. initial map spawns like before
	. heartbeats
	. global sync message
	. attaching new clients (at any time)
- local client + remote server
	. input on local is validated then executed by the local server
	and sent to the remote one
	. conflicts are resolved by sending sync messages
	. remote server sends sync messages either for stale units/new commands/conflicts,
	or for everything
- graphical server only: toggle for no graphics,
otherwise provide a godmode that works exactly the same


## random notes for now

- client: join observers on connect
- program a lobby for both client and server
 	. server: kick/ban ip's, rate limit, set teams and rules
 	. master client -> server priviledges
 	. master client == local client
 	. if spawning a server only, local client just implements the lobby
 	  and a console interface, same as lobby, for controlling the server
 	. client: choose slot
- both server and client do pathfinding etc, and maintain their own simulation,
however only the server sim is authoritative,
only the server does any rand() shit,
only it commits spawns, etc.
	. client performs pathfinding and basic checks before sending to the server
	. the idea is to keep network traffic to a minimum
	. to make sure sims are synchronized, there can be sync messages at certain points
		* spawn a unit
		* move a unit, change angle, stop etc
	. client only has a partial view of available information,
	server handles fog of war etc and only hands information that the client can have
	. traffic to a client must not contain information about other teams
