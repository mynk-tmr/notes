##### WebSockets
- provides full-duplex channels(=bidirectional) over a single *TCP* connection. 
- differs from HTTP — gives a **persistent connection** that **both parties can use** to send data at any time.
- Websocket connections begin with a **handshake** to upgrade to bidirectional mode. 

##### WebRTC
- real time communication between browsers. Data never touches server.
- uses *UDP*. (speed, but unreliable). Has no built-in signalling 
- websockets use server (& can signal)
- **Signalling** : both peers share 
	- a *session description protocol* -> an object with metadata about session connection like codec, media type, etc
	- *ICE candidates* -> endpoint (IP+socket) to receive data. Peer obtains it from a *stun* server and relays it to other