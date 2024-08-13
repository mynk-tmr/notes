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
