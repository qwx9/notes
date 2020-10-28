# networking: general notes

## random notes for now

- network protocol and clear client/server separation
	. split client sleep etc from server
	. that way we can move the camera around etc while paused or while
	  sleeping a tic delay
- networking
	. recvbuf and sendbuf for accumulating messages to flush to all clients
 	. server: spawn server + local client (by default)
 	. client: connect to server
 	. both initialize a simulation, but only the server modifies state
- command line: choose whether or not to spawn a server and/or a client
   (default: both)
- always spawn a server, always initialize loopback channel to it; client
   should do the same amount of work but the server always has the last word
   	. don't systematically announce a port
- client: join observers on connect
- program a lobby for both client and server
 	. server: kick/ban ip's, rate limit, set teams and rules
 	. master client -> server priviledges
 	. master client == local client
 	. if spawning a server only, local client just implements the lobby
 	  and a console interface, same as lobby, for controlling the server
 	. client: choose slot
- server (sim.c) keeps track of all commited paths and uses map.c pathfinding to
  recompute them when necessary; client only issues commands to change state or set
  movement, etc. (if allowed)
	. in other words, there is no need for a client blockmap, the client doesn't
	  do pathfinding
	. selection and issuing orders is done client-side
	. server should not send information on non-visible objects to the client to
	  avoid cheating, etc., but this is besides the point right now
	. so, maybe client SHOULD maintain a blockmap based on known information? and
	  it should do pathfinding to validate a command before issuing it?
	. in other words, sim.c is server only; client and server have disparate
	  views on the simulation
		* however, the server should be able to inform the client when an
		  object becomes visible or disappears -> how?
		* it has to maintain some structure to check for visibility etc to
		  inform the client of any visible changes
		* client-side equivalent of sim.c which receives messages from server
		  and updates known state
	. the client/server separation can be done later
