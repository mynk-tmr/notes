A server using HTTP is called **web server.**
## HTTP

`http` and `https` are *Applayer* protocols for data exchange on WWW. They use port 80 & 443.
Parts of http-based system : `client => proxies => server`

*Proxies* : perform caching, filtering, load balancing, authentication etc to help out servers. They can be `transparent` or `not` (mutates original request)

### Features of http (1.1)
- Flow : open *tcp* connection to server, send https *req*, get http *response*, close/reuse connection.
- *statefulness*  with cookies (originally *stateless*)
- *extensible* with custom headers agreed upon by client/server
- *1 original request* : for HTML document and then additional requests (css, audio, etc)
- *persistent connections* : - To close it, the header **`Connection: close`** is sent
- *chunked transfers* :- server can send a response in pieces or chunks
- *cache control*: server can instruct proxies and clients on cache handling
- *Cors*: headers can relax same-origin constraint
- *Authentication*: basic
- *Tunneling*: wrapping data packets with extra headers  to change their destination.

### New features in https 2.0
- *Binary protocol* instead of Textual :
	- Each `http msg` is split into `frames` (binary data) each with a unique `stream id` (odd for requests, even for responses). 
	- Frame kinds - `HEADERS` (metadata ; opens stream) , `DATA` (msg body) , `RST_STREAM` (abort stream but keep connection)
- *Multiplexing* : 
	- browser can make 5-6 parallel requests via same connection
	- `frames` are sent asyncly & re-arranged correctly by machine with `order` and `stream_id`
- *Server Push* : 1 request initiates a chain of responses
	- done via `PUSH_PROMISE` header to client
- *Header compression* (HPACK)
- `pseudo headers` i.e. **`:method`**, **`:scheme`**, **`:host`** and **`:path`**
- *Request Prioritization* : a client can assign/change priority of a stream for server to process first

### HTTP3
- designed to complement version 1.1 and 2. 
- **UDP-based** and uses **QUIC**, a multiplexed encrypted transport protocol.
- over 3× faster and supported by 94% of browsers.

### Methods
- GET retrieve data.
- HEAD like GET, but only ask for headers (no body)
- POST create a resource on server
- PUT update a resource with sent payload body
- PATCH applies partial modifications to a resource
- DELETE resource
- CONNECT for tunneling 
- OPTIONS retrive cors-related configuration
- TRACE performs a message loop-back test 

### Headers 
allow passing **additional info** in each http message.
> RESPONSE
> `Server` `Set-Cookie` `Content-Type` `Content-Length` `Date`
> 
> REQUEST
> `Cookies` `Accept-xxx` `Content-*` `Authorization` `User-Agent` `Referrer` `Host`

MIME Types : specify content types in `Accept` field, consist of a `type` and a `subtype`.

### Request 
![Request body is optional, but required in POST (info to post)](Untitled%201.png)
- consists of an *HTTP verb* -- kind of operation to perform
- a *path* to a resource, *headers* and optional *body*

### Response
![Untitled](Untitled%202.png) 

## Cross Origin Resource Sharing

- allows client-side script to communicate with a server *different* from the one that loaded it. 
- Client sends a cors request to cross server  : `OPTIONS` verb with `Origin` header
- server responds with a header `Access-Control-Allow-Origin : <domains>` 
- If origin is part of allowed domains, cors proceeds

## API

**API —** interface or standard of communication between softwares

Types of API
- **Remote APIs** : interact via network and manipulate resources on network.
- **Web APIs** : remote API based on http protocol. 
- **Local APIs** : to get local middleware services
- **Program APIs** : makes a remote program appear to be local by making use of RPCs (Remote Procedural Calls). 

Server-side APIs
- **REST**
	- a *architectural* system based on HTTP response/request cycle.
- **GraphQL
    - a query language and server-side runtime.
    - gives clients exactly the data they request and no more.
    - can pull data from multiple sources in a single API call.

## REST

Create, Read, Update, Delete *resources* are 4 basic functions of an API. In a *REST* environment, CRUD refers to `POST, GET, PUT, DELETE` . Read is a `pure` operation. 

To design RESTful API/system
1. create data schema of resources (key-values, types, classes etc.)
2. `Content-Type` of each resource
3. request format to perform CRUD operations eg. `POST user/:id/todo/new <body>`
4. reponse format that server sends for each e.g. `200 (OK) Content-type:text/css <body>`

##### Status codes & msg
| GET `200 OK` <br>POST `201 CREATED`  <br>PUT `200 OK` <br>DELETE `204 NO CONTENT` | `404 NOT FOUND` resouce isn't there <br>`403 FORBIDDEN`<br>`400 BAD REQUEST` <br> `500 INTERNAL SERVOR ERROR` server failed | `301`  redirect GET/HEAD<br>`308` redirect POST etc<br>`304` sent cached data |
| ---- | ---- | ---- |
### 6 Characteristics of Restful System

##### Separation of Client and Server
- client & server code evolve independently and interact via standard operations on resources. 
- Clients use *same* endpoints and *structure* of request/response
##### Statelessness
- client + server can work without knowing each other's state and content of previous msgs.
##### Cachebility
- to reduce load on database and handle large traffic. e,g, `Redis` to implement this
- It permits any data format
##### Layered system
-  inherits security from underlying TCP layer
##### Code on demand (optional)
Servers can extend client's functionality by transferring executable code.
##### Uniform interface
- Resources are identified in requests and are separate from their representations
- Client is sent resource that it can read, modify or delete
- Each reponse has a message that is self-explainatory
- After accessing a resource, client is told of other actions that are currently available (hyperlinks)

## Basic Backend

Server runs a **web application** that contains logic about how to respond to requests based on `routes` (`http-verb+uri`). Each `route` can have one or many request `handler` functions.

A **Web API** is collection of *endpoints* and the *resources* they expose. It is defined by requests it handles and responses it gives eg. `get/users/` having endpoints `/:id` or `/:type`

Node.js uses an event-driven, non-blocking I/O model. This non-blocking I/O eliminates the need for multi-threading.

### Model View Controller

An architectural pattern which aims to separate app into 3 main logical components
##### Model
manages data and business logic. When data state changes, it notifies *View* to update UI or *Controller* if update needs extra logic to occur
##### View
handles display of data to user (templating / rendering)
##### Controller
routes commands to model and view when requested by client

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

## WebSockets 

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