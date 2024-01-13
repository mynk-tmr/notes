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
- DELETE resource
- CONNECT for tunneling 
- OPTIONS retrive cors-related configuration
- TRACE performs a message loop-back test 
- PATCH applies partial modifications to a resource

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
- an HTTP verb -- kind of operation to perform
- a path to a resource
- a _header_, which allows the client to pass along information about the request
- an optional message body containing data

### Response
![Untitled](Untitled%202.png) 

## CORS

[same-origin policy](https://developer.mozilla.org/en-US/docs/Web/Security/Same-origin_policy) : a script can only request resources from the server it was loaded from. To bypass this, we enable **CORS** 

**Cross-Origin Resource Sharing** ([CORS](https://developer.mozilla.org/en-US/docs/Glossary/CORS)) is an HTTP-header based mechanism that allows a *server* to indicate other *origins* from which a browser permits loading resources

## REST

Create, Read, Update, Delete *resources* are 4 basic functions of an API model. In a *REST* environment, CRUD refers to `POST, GET, PUT, DELETE` . Read is a `pure` operation. 

RESTful system use http style response/request protocol. To design RESTful system
1. create data schema of resources (key-values, type, classes etc.)
2. `Accept & Content-Type` of each resource
3. request format to perform CRUD operations eg. `POST user/4/todo/new <body>`
4. reponse format that server sends for each e.g. `200 (OK) Content-type:text/css <body>`

##### Status codes & msg
|  |  |  |
| ---- | ---- | ---- |
| GET `200 OK` <br>POST `201 CREATED`  <br>PUT `200 OK` <br>DELETE `204 NO CONTENT` | `404 NOT FOUND` resouce isn't there <br>`403 FORBIDDEN`<br>`400 BAD REQUEST` <br> `500 INTERNAL SERVOR ERROR` server failed | `301`  redirect GET/HEAD<br>`308` redirect POST etc<br>`304` sent cached data |
### 6 Characteristics of Restful System

##### Separation of Client and Server
client & server code evolve independently and interact via standard operations on resources. 
Clients use same REST endpoints and have identical form of request/response
##### Statelessness
- client server can work without knowing each other's state and content of previous msgs.
- no `interface` is required
##### Cachebility
to reduce the load on database and handle large traffic. e,g, `Redis` to implement this
##### Layered system
##### Code on demand (optional)
Servers can extend client's functionality by transferring executable code.
##### Uniform interface
- Resources are identified in requests and are separate from their representations
- Clients receive files that represent resources. Client is able to modify or delete them
- Each reponse has message that is self-explainatory
- After accessing a resource, client is told of other actions that are currently available (hyperlinks)

## Basic Backend

Server runs an `app` that contains logic about how to respond to requests based on `routes` (`http-verb+uri`) and this is called `routing`. Each `route` can have one or many request `handler` functions. 
Databases provide an interface to save data in a persistent way to memory. 

A Web API is collection of *endpoints* and the *resources* these endpoints expose. It is defined by requests it handles and responses it gives
