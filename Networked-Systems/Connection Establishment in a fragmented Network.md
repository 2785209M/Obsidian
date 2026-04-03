# TCP
TCP resides in the transport Layer
Provides a reliable, ordered, byte stream service over the best-effort IP Network
Provides Congestion control to adapt to network capacity
Most widely used internet transport protocol

**TCP is delivered within IP**
- TCP segments are carried as data in IP packets
- IP packets are carried as data in link layer frames
- Link layer frames are delivered over the physical layer

![[image-2.png]]


## TCP Connections
Typical TCP connections are client-server
- The server listens for connections on a well-known port
- The client connects to the server
- The client sends requests and the server responds

However, TCP can also be used in a peer-to-peer manner
- The two peers connect simultaneously, then send and receive data
- This requires both peers to have known and accessible IP addresses, and to be able to bind() to known ports

![[image-3.png|225]]


## TCP client-server connection establishment
- The server calls accept() and waits for a signal
- The client calls connect() which triggers the three-way handshake

A handshake is required before TCP communication can begin.

- **client - server**
	- Synchronize bit set in TCP header
	- Client Initial sequence number chosen at random
- **Server - client**
	- SYN
	- ACK for client's initial sequence number
	- Server'c initial sequence number chosen at random
- **Client - server**
	- ACK for server's initial seqence number

Once the handshake is complete, a connection is established

**send()/recv()** are used to send and receive data and acknowledgements

# Impact of transport layer security
Implementing transport layer security slows down TCP as it requires more Round Trip Time to complete the TLS handshake.

![[image-4.png]]


# Impact of IPv6 dual stack deployments
Using the IPv4 and IPv6 protocols simultaneously means there are two internets.
Some hosts only connect via v4 and others only via v6, and some have both.
IPv6 is not a subset of IPv4 but it does overlap in some places.

# Connecting to a host with more than one IP address
- Perform parallel look-ups for IPv4 and IPv6 and start with whichever one completes first
- Call connect for that address. If it does not work call connect to the next address in the list, alternating between IPv4 and IPv6
- Use the first connect() that succeeds

## Factors that affect performance
- Bandwidth
- RTT
## How to improve performance
- Overlap connect calls for hosts with multiple addresses
- Reduce number of TCP connections made
- Reduce number of requests per connection
- Overlap TCP and TLS handshakes -> QUIC transport control
- Reduce RTT


# Peer-to-peer connections
In concept, the internet is a peer-to-peer network where any device can, theoretically, talk to any other
You should be able to run a TCP server on any device
You should be able to run a TCP or UDP based peer-to-peer application

In reality, peer-to-peer connection is difficult due to **network address translation**

## Network Address Translation
IPv4 address space has been exhausted
IPv6 is the long-term solution but the translation from one to the other is taking many years
**Network address translation** is a work-around for the shortage of IPv4 addresses. It allows several devices to **share a single IP address.**

## Connecting a single host
Internet service providers all own an IP address prefix. They assign a customer a single address for a single host. There is **no address translation**

## Connecting multiple hosts
**What is supposed to happen:**
The customer acquires a router. The ISP assigns a set of IP addresses from the ISP's prefix for the customer to use. From there every device that connects to the customer's router is assigned an IP address from that range. **No address translation**.

**What actually happens:**
The customer acquires a NAT (Network Address Translation) router. Every device that is connected to their network is assigned a private IP. The NAT performs translation on every packet traversing it. This allows every private IP address to be represented as a single public IP address in the internet.

**The NAT hides a private network behind a single public IP address**

## Problems due to NAT
- Client-server applications with the client behind NAT work without changes
- Client-server applications with the server behind NAT fail without explicit port-forwarding
- Peer-to-peer applications fail without a NAT traversal algorithm
- This encourages the centralization of services to data centers in the ISP network or internet

## Why use NAT?
It allows translation between IPv4 and IPv6 addresses. This is useful in the transition from IPv4 to IPv6. 
It also allows you to avoid having to renumber a network when changing to a new ISP.

## NAT over TCP connections
Outgoing connections create a "state" in NAT, which  allows the router to keep track of the original host device that initiated the connection so that replies can be translated back to the correct host.
Incoming connections cannot create states, which complicates running a server behind NAt and peer-to-peer connections.

## NAT for UDP connections
Outgoing connections in UDP also create states in NAT. The difference is, NAT can't detect the end of a UDP flow, so there need to be periodic checks to see if the flow is still active.
State still cannot be created for incoming connections.
UPD NAT is less strict for incoming connections than TCP. Often simpler for peer-to-peer connections.

## Peer-to-peer NAT Traversal
Peer-to-peer connection is possible over NAT if both of the NAT routers involved think that they are opening a client-server connection, and that the incoming response is coming from a server.
Peers systematically **probe connectivity**, in order to try to establish a connection using every possible combination of addresses. This generates a lot of overhead.
![[image-5.png]]

## Binding Discovery
Packets sent from a host on a private network to a public network will have their IP address and port translated.
The server can see these translated labels, and sends a response to the host informing it what its translated labels are.
This is known as **NAT Binding Discovery**
The **STUN** protocol (Session Traversal Utilities for NAT) is a standard binding discovery mechanism

The host will attempt to find every possible **candidate** IP address on which it might be reachable. It will try all Pv4 and IPv6 addresses of each interface, for each of which and server reflexive addresses will be discovered using STUN, and it will try any related addresses e.g. TURN proxy and VPN.

## Exchange Candidates
In a **Peer-to-peer** connection each host will exchange their candidate addresses. They then make TCP connections and exchange data over those connections.

## Probing for connectivity
Each host will systematically try connect() from each of their candidate addresses to every candidate address of their peers
Connection addresses sent from a host that passes through a NAT will open a binding that allows a response even if the connection request fails
A later incoming connection request that reaches the port on the NAT previous opened by previous outgoing request will pass the NAT allowing the connection to succeed
The host then echange data over the connection path to confirm
The **ICE algorithm** describes how to do this

## TLDR
Binding discovery and systematic probing are slow and require a lot of overhead
They are effective for UDP traffic as connections aren't required
Less effective for TCP connections as they require exact matches for packets before they allow connections to be established