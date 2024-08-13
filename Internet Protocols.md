A **protocol** is a set of rules and standards for exchanging data. Most Internet protocols are specified by Internet documents known as **Request For Comments** or **RFCs**

*Packet* - A small unit of data that is transmitted over internet. **Latency** (ms) is time taken to reach its destination. **Bandwith** (bps) is the amount of data transfered per second

*IP Address*: A unique ID assigned to each device on a network. IPv4 is 32bit (192.0.0.1 ⇒ four 8bit no.s). IPv6 is 128bit, uses hex numbers and is faster, cheaper and secure. Machines can use both via dual addressing

*Domain Name:* A human-readable alias for IP address.

*Port:* unique no. that identifies an app or service running on a device.

*Socket:* combination of IP address and port number, representing a specific endpoint for communication.
##### Router
- A device that **directs** **packets** between different networks.
	1. **Manages traffic** among networks by forwarding data packets to their intended IP addresses
	2. Allows multiple devices to use the same **Internet connection.**
- uses an **internal routing table** — a list of paths to various destinations. The router reads a packet's **header** to determine where it is going, then consults the routing table to figure out the most efficient path to that destination. It then forwards the packet to the next network in the path.

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

### Mail related protocols

> application layer
##### Simple Mail Transfer Protocol
- used for “Mail Relaying” (sending and routing email between servers) and can be configured as an email gateway
- Features
    - client-server — sender's email client ⇒ recipient's mail server
    - connection oriented [TCP based]
    - text based — commands and responses are exchanged in ASCII
    - push protocol (sends out e-mails)
- **Extended SMTP** : allows sender authentication, direct multi-media file transfer, mail size reduction
##### Internet Message Access Protocol
- allows you to access your email from multiple devices and webmail clients (replaced POP3)
- Features
    - client/server protocol — client request its default server to access emails
    - doesn’t download emails to device
    - pop protocol — retrieve e-mails

### Secure Shell (SSH) protocol

a *TCP-based* appLayer protocol to securely send commands to a computer on network
Features
- SSH uses **public key cryptography** to authenticate and encrypt connections. In SSH connection, both sides have a **public/private key pair**, and each side authenticates the other using these keys.
- _SSH tunneling_ uses **port forwarding** to send packets from one machine to another. E.g. you can connect to an open server to send data to a closed server.
- Port 22 is the default port for SSH

Most common SSH use cases are:
- Remotely managing servers, infrastructure, and employee computers
- Securely transferring files
- Accessing services in the cloud without exposing a local machine's ports to the Internet

SSH vs. HTTPS — https only verifies the server identity, never blocked by firewalls, and doesn’t allow the client to access the server's command line

### File Transfer Protocols — all are built on **TCP**

**File Transfer Protocol** is foundational
- allows concurrent directory transfers 
- Lacks encryption and uses dual data channcels — risky
- limited encoding support (ASCII, 8bit, ECBD)

**FTP Secure** adds TLS encryption to FTP. It has wide support since most of internet infrastructure has built-in support for SSL

**Secure FTP** uses SSH encryption. It is the **recommended** choice.
- advanced file management features — create, delete, transfer etc.
- recipient of your files must authenticate with cryptographic keys

**Secure Copy Protocol** also uses SSH encryption but is simple.
- better suited for one-time file transfers ; way faster than SFTP.
- doesn’t have file mgmt features ; cannot resume file transfers.

**Managed File Transfer** support protocols like SFTP and FTPS. Used in banking industry, MFT provides additional encryption.