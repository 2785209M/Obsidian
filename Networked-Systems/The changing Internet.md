## Networked Systems
A network system is a cooperating set of autonomous computing devices that exchange data to perform an  application goal.

What is the physical layer?
- The physical layer allows transmission of data via a channel. This can be baseband encoding or carrier modulation. This can be wired or wireless. This is what we learned about in Communication Systems last semester.

What is the data link layer? 
- The data link layer provides framing, addressing, and media access control. It structures the bitstream into meaningful frames of data. It detects and corrects transmission errors, and it arbitrates access to the channel.

## Media Access Control
Propogation delay aims to limit collisions during data transmission. If a collision occurs, the propogation delay is increased and the data is retransmitted. The sender listens before sending data. If the channel is active, the new sender will wait for it to be quiet. This is used by ethernet and WiFi.

## Network Layers and Internet Protocols

![[image.png]]

- IP addresses encode the location of the network interface
- If a host has multiple network interfaces then it has multiple IP addresses
- A host can support both ipv4 and ipv6 on each interface
- A host can have multiple IP addresses of each type assigned to each interface
- 
## Autonomos Systems and Inter-Domain Routing

![[image-1.png]]

In an AS Network diagram, the core is well connected with the edge networks being sparse. Each edge networks can use a default route to contact the core.

# The Transport Layer
Transport isolates applications in the network. It demultiplexes traffic, enhances network quality, and performs congestion control.

There are only two deployable transport protocols in the internet: TCP and UDP.

IP Networks provide a best-effort service. Packets can still be lost, delayed, and reordered.

### UDP
UDP is the simplest transport protocol. 
- Exposes raw IP service to applications
- Connectionless, best-effort delivery
- No congestion control
- Adds 16 bit port number to identify services
- Used by applications which prefer timeliness over reliability, such as video calls
- Application must be able to tolerate some data loss
- Application must be able to adapt to congestion in the application layer. e.g. buffering in videos.

### TCP
TCP is a reliable, ordered, byte stream delivery service running over IP.
- Lost packets are retransmitted. Ordering is preserved, message boundaries are not.
- Adapts sending rate to match network capacity (congestion control)
- Adds port number to identify services
- Used by applications which need reliability. Typically the default for most services.


## High layer Protocols
the goal of higher layer protocols is to support the needs of applications.
The OSI model defines three layers above transport: Session, Presentation, and Application.
Internect architecture makes no clear distinction between these layers.

- Manage layer connections
- Name and locate application-level resources
- Negotiate data formats, and perform format conversions if needed
- Present data in appropriate manner
- Implement application-level semantics

### Presentation Layer
Manages the presentation, representation, and conversion of data
- Media types and content negotiation
- Channel Encodings
- Internationalisation, languages, and character sets

### Application Layer
Protocol functions specific to application logic
- Deliver Email
- Retrieve a web page
- Stream Video

### Protocol Standards
The OSI model is a good way of thinking about network protocols
But it misses two key layers: Financial and Political
Successful network protocols have interoperability between different vendors

**Protocols are political. Code influences law.**

# Innovation in the network
TCP has been standardized. it is now very hard to change. 
The internet is becoming centralized around large network providers such as google, facebook, and amazon. This reduces network neutrality, competition, and innovation. It is very difficult for new startups to compete with these giants.