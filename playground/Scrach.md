## Structure
![[Pasted image 20231018140319.png]]

1. create a socket  `socket()`
2. bind address and port (init?) `connect()`
	1. for client argument of  address is specic which service you want to connect 
	2. for server argument of address is specific which client you want to lisetn 
3. client `send()` and server  `accept()`  then server can  call `recv()` function to get the message which will `send()` back to client after  process.  celinet also have `recv()`func the meaning is equ to server's `recv()`

smmary : two difference: 
  1.  the `bind()` function args meaning.
  2. `lisent()` func only in server side.



## Implements
1

### Single threading

### multi threading

### IO Multiplexing
