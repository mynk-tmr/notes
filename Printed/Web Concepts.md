##### Model View Controller
- An architectural pattern which aims to separate app into 3 main logical components
- **Model** manages data and business logic. When data state changes, it notifies *View* to update UI or *Controller* if update needs extra logic to occur
- **View** handles display of data to user (templating / rendering)
- **Controller** routes commands to model and view when requested by client

##### Basic Server
- A server using HTTP is called **web server.**
- Server runs a **web application** that contains logic about how to respond to requests based on `routes` (`http-verb+uri`). Each `route` can have one or many request `handler` functions.
- A **Web API** is collection of *endpoints* and the *resources* they expose. It is defined by requests it handles and responses it gives eg. `get/users/` having endpoints `/:id` or `/:type`

##### AJAX Asynchronous JavaScript and XML
- a technique in which a web app fetches content from server by making async HTTP requests, and uses new content to update parts of page
- Used for SPAs. 
- Initially,  implemented using the [`XMLHttpRequest`](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest) interface, now [`fetch()`](https://developer.mozilla.org/en-US/docs/Web/API/fetch) 

##### Single Page APP
- A single webpage sent by server that makes _incremental_ updates to itself over user session
- It performs _client-side routing_ to eliminate the need to fetch new pages from server and avoid costly repaints
- CSR enables developers to manipulate *history* stack without making a document request to the server.

##### Progressive web Apps
- web apps built and improved with modern APIs to enhance UX.
- Benefit : with a single codebase, we can utilise broad reach of web apps and rich capabilities of platform-specific apps

##### Server Components 
- allow you to write UI that can be rendered and cached on server.
- 3 server rendering strategies: static, dynamic, streaming
- Benefits
	- Data Fetching in secure & low latency manner
	- caching rendered output 
	- performance : moving non-interactive UI to server, means smaller bundle is sent to client
	- First Contentful Paint (FCP) is fast, SEO is better

##### API 
- interface or standard of communication between softwares
- Types of API
	- **Remote APIs** : interact via network and manipulate resources on network.
	- **Web APIs** : remote API based on http protocol. 
	- **Local APIs** : to get local middleware services
	- **Program APIs** : makes a remote program appear to be local by making use of RPCs (Remote Procedural Calls). 
- Server-side APIs
	- **REST**
		- a *architectural* system based on HTTP response/request cycle.
	- **GraphQL
	    - a query language and server-side runtime.
	    - gives clients exactly the data they request and no more.
	    - can pull data from multiple sources in a single API call.

##### Webhooks
- let you subscribe to **server events** and receive data on client when those events occur.
- As opposed to polling an API for data, you create webhook to get data whenever it is available. Hence called **reverse API** since 
- Use cases
	- CI/CD pipelines 
	- notifications, download 
- Advantages over REST API
	- no polling (less resource)
	- scale better than API (no API limitter --> less cost)
	- real time updates

##### Cross Origin Resource Sharing (CORS)
- allows client-side script to communicate with a server *different* from one that loaded it. 
- Client sends a cors request to cross server  : `OPTIONS` verb with `Origin` header
- server responds with a header `Access-Control-Allow-Origin : <domains>` 
- If origin is part of allowed domains, cors proceeds

##### Web Sockets
- provides full-duplex channels(=bidirectional) over a single *TCP* connection. 
- differs from HTTP — gives a **persistent connection** that **both parties can use** to send data at any time.
- connections begin with a **handshake** to upgrade to bidirectional mode. 

## WebRTC

##### What
- a set of technologies for peer to peer duplex real-time communication between browsers. Data never touches server.
- uses *UDP*. (speed, but unreliable).
- websocket server performs **signalling**, then webrtc proceeds. In signalling, both peers share
	- a *session description protocol* -> an object with metadata about session connection like codec, media type, etc
	- *ICE candidates* -> endpoint (public IP+socket) to receive data. Peer obtains it from a *stun* server and relays it to other

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
