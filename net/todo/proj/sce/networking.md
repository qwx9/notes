# networking: general notes

## decoupling

- local server only: as before, but using a shunted network protocol
as if the server is responding to network commands
	. channel for communication
	(always there if there is a client, remote server or not)
	. no listener, no networking
	. input on local is validated then executed by the local server
	and sent to the remote one, if there is one (not here)
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


give units unique identifiers between client and server
so we don't guess number of units for other teams,
we get a large enough range instead
8 teams → 32 - 3 bits (4 if observer, but that has no units)

don't execute any command client side until server has signed off?
"prediction": execute before server gives the command, but be ready to resync



## random notes for now

- networking
	. recvbuf and sendbuf for accumulating messages to flush to all clients
 	. server: spawn server + local client (by default)
 	. client: connect to server
 	. both initialize a simulation, but only the server modifies state
- command line: choose whether or not to spawn a server and/or a client
   (default: both)
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
