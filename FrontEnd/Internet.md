**What happens when you navigate to URL ?**
- First client searches its cache and local files. If not found, it issues a DNS request/ lookup, providing a hostname such as “[example.com](http://example.com)”.
- this request is received by **recursive DNS resolver,** which searches for IP address with a series of recursive queries.
- It queries root DNS server, then TLD server (“.com”) and so on.. down to master sever (or authoritative name server)
- name server sends IP address to resolver which sends it to client

**Internet protocol suite (TCP/IP)** : 
* Most common framework for organizing internet protocols. 
* It loosely defines a **4 layer model,** which is built into OS and hardware.
- **Application Layer** : responsible for handling data from/to processes or apps.
- **Transport Layer :** responsible for end-to-end communication between two **sockets**
- **Network layer** : responsible for routing data packets on network from source to target
- **Hardware Layer** : Converts binary data in packet to network signals and back.
- At source, data travels **down** the layer — each layer adds routing data and break data into chunks. At destination, it travels up — each layer strips routing data and reconstruct data from chunks.

![[Pasted image 20240113151401.png]]

**Transmission Control Protocol**
* TCP segment: a unit of data. It has a `header`, `port no. of target`, `sequence no.` etc
- Features
    - **connection-oriented** — opens a connection to transmit data, then closes it
    - **reliable** — for each packet received, an *acknowledgement* is sent to sender to confirm the delivery. TCP also includes a *checksum* in headers for error-checking
- Each TCP connection begins with a 3 way handshake
    - During this, various parameters like _maximum segment size & window size_ are negotiated.
	- Once a connection is established, data is broken into TCP segments, then transmitted.
	- At target, segments are reconstructed and sent to a specific application using port number.
    - Problem — Client may start sending data as soon as it dispatches last ACK packet. Server will not fulfill request, until it receives ACK packet.

	![[Pasted image 20240113151931.png]]

**Internet Protocol**
- It is the only **network layer** protocol and gives each packet it's destination IP address
- Unlike TCP, **IP is unreliable, connectionless** protocol
- IP's job is to send and route packets to their destination computer. TCP’s job is to make sure all packets arrive and are in correct order.
- IP adds its own headers to TCP packets. IP treats each packet as a self-contained data.

**HTTP** : an application layer for data exchange on WWW. They use port 80 & 443. Parts of http-based system : `client => proxies => server`

**Proxies** perform caching, filtering, load balancing, authentication etc to help out servers. They can be `transparent` or `not` (mutates original request)

**Features of http (1.1)**
- Flow : open *tcp* connection to server, send https *req*, get http *response*, close/reuse connection.
- *statefulness*  with cookies (originally *stateless*)
- *extensible* with custom headers agreed upon by client/server
- *1 original request* : for HTML document and then additional requests (css, audio, etc)
- *persistent connections* : - To close it, the header **`Connection: close`** is sent
- *chunked transfers* :- server can send a response in pieces or chunks
- *cache control*: server can instruct proxies and clients on cache handling
- *Cors*: headers can relax same-origin constraint
- *Authentication*: BASIC
	- “username: password” encoded into base64 string and sent in “Authorization” header on every HTTP request
- *Tunneling*: wrapping data packets with extra headers  to change their destination.

**New features in https 2.0**
- Binary protocol instead of Textual : Each `http msg` is split into `frames` (binary data) each with a unique `stream id` 
- Multiplexing : browser can make 5-6 parallel requests via same connection. `frames` are sent asyncly & re-arranged correctly by machine with `order` and `stream_id`

**User Datagram Protocol**
- **faster** than TCP because it’s **connectionless** but **less reliable** because No error-checking or retransmission of lost packets.
- Used in — video & audio streaming

**Transport Layer Security (TLS 1.2/1.3)**
- a **cryptographic** protocol designed to provide security over a network. e.g. https uses it
- It is built on top of TCP. Once TCP connection is established, client authenticates a server’s TLS certificate
- Then, TLS handshake happens where the client and server exchange messages to negotiate encryption algorithm and a session key
- Once **secure** connection is established, data is encrypted and exchanged.

**Representational State Transfer** 
* uses HTTP protocol to communicate b/w server and client. 
* **Pros** -> lightweight, scalable, supports different languages/systems
* **Cons** -> stateless (so no sessions), depends on security of underlying protocol
* Every content is considered a **resource** and IDed by **URI**. It is like object in OOP. e.g. text files, photos. Client consumes or changes these resources.
* Why scalable? 
	* **stateless**; so easy to scale horizontally because the servers need not communicate much with each other while serving requests.

**Uniform Resource Identifier** 
* used for identifying each resource of the REST architecture.
* Format : `<protocol>://<service-name>/<ResourceType>/<ResourceID>`
* **Addressing** is the process of locating a single/multiple resources on server. URI does it.
* two URI types
	* **URN:** identifies the resource via name (unique and persistent)
		- resolvers translate them to URLs to identify resource
		- Schema example `urn:isbn:1234567890`
	 * **URL:** has information regarding fetching of a resource from its location.
		 - `protocol://domain/path?params`

**Best practices for URI**
- plural nouns e.g `users/{id}/purchases` 
- hyphen or underscore ; lower case
- maintain backward compatibility. Older URI must be redirect to new with `300` HTTP status
- Use appropriate HTTP methods like GET, PUT


**6 Features of REST / Explain ? feature in REST
* **Statelessness** : client + server can work without knowing each other's state and content of previous msgs.
* **Separation of Client and Server** : Client & server code evolve independently and interact via standard operations and API on resources.
* **Cachebility** to reduce load on database and handle large traffic. e,g, `Redis` 
* **Layered system** inherits security from underlying protocol
* **Code on demand (optional)** : servers can extend client's functionality by transferring executable code.
* **Uniform interface** : resources can be identified via request and client can CRUD them. Each response is self-explantory

| REST                                                                      | AJAX                                                                             |
| ------------------------------------------------------------------------- | -------------------------------------------------------------------------------- |
| uses URI for accessing resources by means of a request-response pattern.  | uses XMLHttpRequest object to send requests  and interprets response dynamically |
| architectural pattern for developing client-server communication systems. | dynamic updation of UI without the need to reload the page.                      |
| needs interaction between client and server.                              | no constant client-server interaction.                                           |

| REST                                              | Web Socket                                                                            |
| ------------------------------------------------- | ------------------------------------------------------------------------------------- |
| stateless architecture                            | stateful protocol & session-based data storage.                                       |
| unidirectional communication                      | bi full duplex                                                                        |
| overheads like URI, verb, etc.                    | none                                                                                  |
| opens new TCP connection                          | No                                                                                    |
| support both vertical and horizontal scaling.     | only support vertical scaling.                                                        |
