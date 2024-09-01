A **protocol** is a set of rules and standards for exchanging data. Most Internet protocols are specified by Internet documents known as **Request For Comments** or **RFCs**

*Packet* - A small unit of data that is transmitted over internet. **Latency** (ms) is time taken to reach its destination. **Bandwith** (bps) is the amount of data transfered per second

*IP Address*: A unique ID assigned to each device on a network. IPv4 is 32bit (192.0.0.1 ⇒ four 8bit no.s). IPv6 is 128bit, uses hex numbers and is faster, cheaper and secure. Machines can use both via dual addressing

*Domain Name:* A human-readable alias for IP address.

*Port:* unique no. that identifies an app or service running on a device.

*Socket:* combination of IP address and port number, representing a specific endpoint for communication.

## DOMAIN NAME SYSTEM

A protocol used to **translate** domain names into IP addresses. **BIND** is used to implement it.
Domain name heirarchy — [www.google.co.in](http://www.google.co.in)
- A domain name consists of **labels**, separated by dots.
- root domain ("."), TLD (in), 2nd LD (co), domain (google), subdomain (www)
- Domain names are managed by domain registries.
##### DNS lookup process
- First client searches its cache and local files. If not found, it issues a DNS request/ lookup, providing a hostname such as “[example.com](http://example.com)”.
- this request is received by **recursive DNS resolver,** which searches for IP address with a series of recursive queries.
    - it queries root DNS server, then TLD server (“.com”) and so on.. down to master sever (or authoritative name server)
- name server sends IP address to resolver which sends it to client
- DNS queries use **UDP** and port 53

## Internet protocol suite (TCP/IP)

Most common framework for organizing internet protocols. It loosely defines a **4 layer model,** which is built into OS and hardware.
- **Application Layer** : responsible for handling data from/to processes or apps.
- **Transport Layer :** responsible for end-to-end communication between two **sockets**
- **Network layer** : responsible for routing data packets on network from source to target
- **Hardware Layer** : Converts binary data in packet to network signals and back.

At source, data travels **down** the layer — each layer adds routing data and break data into chunks. At destination, it travels up — each layer strips routing data and reconstruct data from chunks.
![[Pasted image 20240113151401.png]]

#### Transmission Control Protocol : TCP
- Features
    - **connection-oriented** — opens a connection to transmit data, then closes it
    - **reliable** — for each packet received, an *acknowledgement* is sent to sender to confirm the delivery. TCP also includes a *checksum* in headers for error-checking
    - transmits data as **stream of bytes** so they reach application layer in same order
- Each TCP connection begins with a 3 way handshake
    - During this, various parameters like _maximum segment size & window size_ are negotiated.
    - Problem — Client may start sending data as soon as it dispatches last ACK packet. Server will not fulfill request, until it receives ACK packet.

	![[Pasted image 20240113151931.png]]

	- Once a connection is established, data is broken into TCP segments having - `header`, `port no. of target`, `sequence no.` , etc. then transmitted.
	- At target, segments are reconstructed and sent to a specific application using port number.

##### User Datagram Protocol (UDP) 
- a **connectionless** protocol and provides **minimal** services to applications. 
- **faster** than TCP because it’s connectionless but **less reliable** because No error-checking or retransmission of lost packets.
- Used in — video & audio streaming

##### Transport Layer Security (TLS 1.2/1.3)
- a *cryptographic* protocol designed to provide security over a network. e.g. https uses it
- It is built on top of TCP. Once TCP connection is established, client authenticates a server’s TLS certificate
- Then, TLS handshake happens where the client and server exchange messages to negotiate encryption algorithm and a session key
- Once **secure** connection is established, data is encrypted and exchanged.

#### Internet Protocol
- It is the only **network layer** protocol and gives each packet it's destination IP address
- Unlike TCP, **IP is unreliable, connectionless** protocol
- IP's job is to send and route packets to their destination computer. TCP’s job is to make sure all packets arrive and are in correct order.
- IP adds its own headers to TCP packets. IP treats each packet as a self-contained data.

##### What is HTTP and HTTPS ?

*Applayer* protocols for data exchange on WWW. They use port 80 & 443.
Parts of http-based system : `client => proxies => server`

##### What are Proxies ?
perform caching, filtering, load balancing, authentication etc to help out servers. They can be `transparent` or `not` (mutates original request)

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

##### What do you understand by RESTful Web Services ?

Services that follow REST architecture. REST stands for **Representational State Transfer** and uses *HTTP protocol* to communicate b/w server and client. 
**Pros** -> lightweight, scalable, supports different languages/systems
**Cons** -> stateless (so no sessions), depends on security of underlying protocol

Every content in REST architecture is considered a **resource**. The resource is analogous to the object in OOP. e.g. text files, photos. Client consumes or changes these resources. Every resource is identified globally by means of a URI.

##### Why REST services are scalable?
They are **stateless** so easy to scale horizontally because the servers need not communicate much with each other while serving requests.

##### How can you test RESTful Web Services?
using various tools like **Postman**, **Swagger**, etc. 

##### How does HTTP Basic Authentication work
- “username: password” --> encoded into base64 string and sent in “Authorization” header on every HTTP request
- should not be used.

#### What is URI ?

**Uniform Resource Identifier** is used for identifying each resource of the REST architecture. URI is of the format:

```plaintext
<protocol>://<service-name>/<ResourceType>/<ResourceID>
```

**Addressing** is the process of locating a single/multiple resources on server. URI does it.

There are 2 types of URI:
**URN:** identifies the resource via name (unique and persistent)
- resolvers translate them to URLs to identify resource
- Schema example `urn:isbn:1234567890`
 
 **URL:** has information regarding fetching of a resource from its location.
 - `protocol://domain/path?params`

**Best practices**
- plural nouns e.g `users/{id}/purchases` 
- hyphen or underscore ; lower case
- maintain backward compatibility. Older URI must be redirect to new with `300` HTTP status
- Use appropriate HTTP methods like GET, PUT

#### HTTP headers
- allow passing **additional info** in each http message.
- MIME Types : specify content types in `Accept` field, consist of a `type` and a `subtype`. 
> RESPONSE
> `Server` `Set-Cookie` `Content-Type` `Content-Length` `Date`
> 
> REQUEST
> `Cookies` `Accept-xxx` `Content-*` `Authorization` `User-Agent` `Referrer` `Host`


#### HTTP Methods / Verbs

- Specifies what action has to be followed to get the requested resource. 
- POST, GET, PUT, DELETE corresponds to **CRUD Operations** (basic ops of API)
- Methods
	- GET retrieve data.
	- HEAD like GET, but only ask for headers (no body)
	- POST create a resource on server
	- PUT update a resource with sent payload body
	- PATCH applies partial modifications to a resource
	- DELETE resource
	- CONNECT for tunneling 
	- OPTIONS retrive cors-related configuration
	- TRACE performs a message loop-back test 

#### HTTP Status codes

standard codes that refer to task status at server
- 1xx - informational responses
- 2xx - successful responses
- 3xx - redirects
- 4xx - client errors
- 5xx - server errors

| GET `200 OK` <br>POST `201 CREATED`  <br>PUT `200 OK` <br>DELETE `204 NO CONTENT` | `404 NOT FOUND` resouce isn't there <br>`403 FORBIDDEN`<br>`400 BAD REQUEST` <br> `500 INTERNAL SERVOR ERROR` server failed<br>`502 BAD GATEWAY`  server was not able to get the response from another upstream server.` | `301`  redirect GET/HEAD<br>`308` redirect POST etc<br>`304` sent cached data |
| --------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | ----------------------------------------------------------------------------- |
#### 6 Features of REST systems 

> Q. format : explain --feature-- in REST

**Statelessness** : In REST architecture, client + server can work without knowing each other's state and content of previous msgs. The context is provided in message.

**Separation of Client and Server** : Client & server code evolve independently and interact via standard operations on resources. Clients use *same* endpoints and *structure* of request/response

**Cachebility** to reduce load on database and handle large traffic. e,g, `Redis` 

**Layered system** inherits security from underlying protocol

**Code on demand (optional)** : servers can extend client's functionality by transferring executable code.

**Uniform interface** : resources can be identified via request and client can CRUD them. Each response is self-explantory

##### How REST is better than SOAP ?
Simple Object Access Protocol is **strict** protocol to implement web services. But REST is just a **flexible design pattern** for developing web services

| SOAP                                                       | REST                                       |
| ---------------------------------------------------------- | ------------------------------------------ |
| SOAP cannot use REST as it is a protocol.                  | REST can use SOAP for implementation.      |
| strict slower                                              | flexible faster                            |
| tight coupling b/w client server                           | no                                         |
| supports only XML transmission                             | XML, JSON, MIME, Text, etc.                |
| not cacheable.                                             | yes                                        |
| uses service interfaces for exposing  resource logic.      | use URI                                    |
| defines its own security measures.                         | inherits the security measures of protocol |
| used when stateful data transfer and reliability is needed | more scalability and maintainability.      |

##### When to use SOAP or REST
- for biz logic (SOAP),  for data (REST)
- strict high level security contract (SOAP)
- transactions (SOAP)

##### Best  practices to develop RESTful web services
- use `JSON` whenever possible 
- use `heirarchy` 
- use appropriate error codes
- use `filter` and `pagination` for large datasets to avoid blockage
- role-based access controls
- caching
- API versioning:  to make seamless changes in endpoints. Semantic versioning can be followed. Use `/v1`,`/v2`, etc at the beginning of the API path

##### How to design RESTful API/system ?
1. create data schema of resources (key-values, types, classes etc.)
2. `Content-Type` of each resource
3. request format to perform CRUD operations eg. `POST user/:id/todo/new <body>`
4. reponse format that server sends for each e.g. `200 (OK) Content-type:text/css <body>`

##### What are Idempotent methods? How is it relevant in RESTful web services domain?

**idempotent** : a method that can be called multiple times without changing result of resources
**safe** : those that do not change any resources internally. Their result can be cached

- GET, TRACE, HEAD, OPTIONS are *safe and idempotent* methods
- PUT and DELETE methods are only *idempotent*. When called first they change resource, but subsequent requests don't change anything.
- POST and PATCH methods are neither safe nor idempotent (resource changes)

##### REST and AJAX

| REST                                                                      | AJAX                                                                             |
| ------------------------------------------------------------------------- | -------------------------------------------------------------------------------- |
| uses URI for accessing resources by means of a request-response pattern.  | uses XMLHttpRequest object to send requests  and interprets response dynamically |
| architectural pattern for developing client-server communication systems. | dynamic updation of UI without the need to reload the page.                      |
| needs interaction between client and server.                              | no constant client-server interaction.                                           |

##### 5 Core components of HTTP Request
- Method/Verb − request operation 
- URI − identify resource on server
- HTTP Version
- Request Header − request metadata such as client type, content format supported, message format, cache settings, etc.
- Request Body − actual message content

![Request body is optional, but required in POST (info to post)](Untitled%201.png)

##### 4 core components of HTTP Response
- status code
- HTTP Version
- Response Header
- Response Body

![Untitled](Untitled%202.png)

##### Web sockets v/s REST

| REST                                              | Web Socket                                                                            |
| ------------------------------------------------- | ------------------------------------------------------------------------------------- |
| stateless architecture                            | stateful protocol & session-based data storage.                                       |
| unidirectional communication                      | bi full duplex                                                                        |
| overheads like URI, verb, etc.                    | none                                                                                  |
| opens new TCP connection                          | No                                                                                    |
| support both vertical and horizontal scaling.     | only support vertical scaling.                                                        |
- **Unit tests** : test small isolated parts of codebase. Interaction with other units is *mocked* eg. class, methods, etc.
- **Integration tests**: test interaction across *real* unit-tested portions of code like modules, database, etc.
- **Smoke test / sanity test**: done before deep tests. Checks critical software functionality using unit+integration tests
- **Functional tests**: test business requirements of app and verify output.
- **End to End tests**: replicates a user behavior with the software.
- **Performance tests**: evaluate system's performance under different conditions, identify memory leaks, bottlenecks, and scalability issues. Major ways -> *load testing* (to identify breakpoint) and *stress testing* (to identify behaviour) under heavy load.
- **Regression tests**: ensure new changes don't break exisiting functionalities.
- **Acceptance tests**: testing against predefined acceptance criteria set by stakeholders.
- **Usability tests**: ensure software is user-friendly and improve UX
- **Fuzz tests**: test with random or unexpected inputs to identify how it responds to unexpected data.
- **Compatibility Testing**: ensuring that software works seamlessly on different platforms.

### Levels of tests
- **Whitebox tests**: test logic of app
- **Blackbox tests**: test behaviour of app, not logic
- **Greybox tests**: combine both
- **Alpha tests:** test in controlled environment
- **Beta/UAT tests:** test in real environment by selected end usersA Web API acts as `interface` to use browser's or system's data/functionality. A web app uses many `APIs` to perform tasks.
##### Types of APIs
- `Browser APIs` — built into browser and are able to expose data from browser and system. e.g. Web Audio API
- `Third-party APIs `— built into third-party platforms (e.g. Twitter, Facebook) that allow you to use some of platform's functionality in your web pages
- `JavaScript libraries` — JS files containing **custom** functions that you can use. e.g React.
- `JavaScript frameworks` — packages of HTML, CSS, JavaScript, and other technologies that you use to write an entire web application from scratch

Library vs. Framework — "Inversion of Control". _A developer calls a method from a library. A framework calls the developer's code._

Features of Client-side web APIs
- **Objects-based —** code interacts with APIs using objects, which serve as containers for the data and functionality that API uses and exposes
- **Fixed recognizable entry points.** e.g in Web Audio API — it is the `AudioContext` object. In DOM API, it is Node object
- Use of **events** to handle changes in state
- **Security mechanisms** where appropriate

##### Model View Controller
- An architectural pattern which aims to separate app into 3 main logical components
- **Model** manages data and business logic. When data state changes, it notifies *View* to update UI or *Controller* if update needs extra logic to occur
- **View** handles display of data to user (templating / rendering)
- **Controller** routes commands to model and view when requested by client

##### Basic Server
- A server using HTTP is called **web server.**
- Server runs a **web application** that contains logic about how to respond to requests based on `routes` (`http-verb+uri`). Each `route` can have one or many request `handler` functions.
- A **Web API** is collection of *endpoints* and the *resources* they expose. It is defined by requests it handles and responses it gives eg. `get/users/` having endpoints `/:id` or `/:type`

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

##### Web Sockets
- provides full-duplex channels(=bidirectional) over a single *TCP* connection. 
- differs from HTTP — gives a **persistent connection** that **both parties can use** to send data at any time.
- connections begin with a **handshake** to upgrade to bidirectional mode. 