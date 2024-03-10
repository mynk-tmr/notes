##### WebSockets
- provides full-duplex channels(=bidirectional) over a single *TCP* connection. 
- differs from HTTP — gives a **persistent connection** that **both parties can use** to send data at any time.
- Websocket connections begin with a **handshake** to upgrade to bidirectional mode. 

##### WebRTC
- a set of technologies for peer to peer duplex real-time communication between browsers. Data never touches server.
- uses *UDP*. (speed, but unreliable).
- **Signalling** : both peers share 
	- a *session description protocol* -> an object with metadata about session connection like codec, media type, etc
	- *ICE candidates* -> endpoint (public IP+socket) to receive data. Peer obtains it from a *stun* server and relays it to other
- websocket server performs signalling, then webrtc proceeds

### Architecures

**P2P**: private 2 peer communication

**Mesh:**
- Each peer sends their media stream directly to every other peer.
- No central server required
- Low latency for small groups.
- But High bandwidth usage, CPU usage, doesn't scale

**Selective Forwarding Unit:**
- central server (SFU) is involved which acts as forwarder
- Pros
    - solve mesh issues
    - Allows for features like *simulcasting* (sending streams at different qualities) and recording.

Mesh is ideal for small, private calls where minimizing latency is crucial. SFU is better suited for larger audience