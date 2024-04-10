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

## JSON Web Token (JWT) 

A *stateless* method of securely transmitting info as JSON. Stateless means each request is *self-contained* and can be used to authenticate user without having to store session data.
##### JWT parts
- A *Header* : token type and hash algo used
- A *Payload* : auth verification data (like `userID` in database) 
- A *Signature* :  to verify token’s integrity (output of `jwt.sign`)
##### JWT authentication working
- *Token Generation*: server generates a JSON token identifying user and auth session (like expiry date)
- *Token Storage:*  client stores that as `http only` cookie
- *User Verification*: In a request, client sends **stored jwt** in headers (`credentials: include`) to server to get access to protected resources
- *Token Expiration:* now re-login to obtain new JWT
##### Best practices
- when to use: stateless api, microservices, reducing database calls
- when NOT: payment related data (credit card no.) should never be sent in payload
- short expiration, `http-only, secure`
- Implement Token Revocation for compromised ones