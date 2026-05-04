# 0-RTT Connection Establishment
It is possible for a client to re-establish connection with a host and immediately start sending and receiving data without having to wait for the TLS handshake to complete.
This can be accomplished by using a **Pre-shared key**. The pre-shared key will be shared between the client and the server in a previous TLS session and can be used again by the client when re-establishing the connection.

**Connection Re-establishment:** To re-establish connection with the server, the client sends a TLS Clienthello message combined with data (typically a HTTP GET request) that has been encrypted using the pre-shared key. When the server receives this request, it will decode the message using the pre-shared key. It will encode a reply into  the TLS Serverhello message and respond with the requested data immediately.

This leads to drastically reduced latency, less overhead in the server, and less waiting time for the client which can reduce battery expenditure in mobile devices.

However, it makes the connection more vulnerable to attack:
**Replay Attacks:** Hackers can intercept the clienthello message and steal the encoded message. If the hacker then sends this to the server later however many times they wants. They don't need to decode the message for this to work. This could be dangerous if the clienthello contained a request for a financial transaction, as the server could be made to process that transaction multiple times.
**No Forward Secrecy:** Typical TLS has forward secrecy as the client and server agree on a secret ephemeral key to use for communication. For 0-RTT connections, the pre-shared key is reused for the first request and reply in the connection - the "early data". This means that if a hacker recorded your early data and was later able to break into a server and steal the pre-shared key, they would be able to decode all of the early data they recorded with that one key. 

# TLS Metadata Leakage
Internet Protocol exposes the IP addresses of host and server
TCP Exposes the ports used and the connection Metadata
The below graph visualizes the exposed data - white is IP, green is TCP, Blue is hidden with TLS:
![[image-17.png|691]]

When TLS is used with HTTP, the Clienthello contains the **Server Name Indication (SNI) Extension**This identifies the requested site so the server knows what public key to use in the Serverhello
This is required to support hosting of multiple sites on one server, but the SNI must be unencrypted because it is sent before the session keys are negotiated. It can't be encrupted with the pre-shared key as this is done before the server is connected and the server is needed to make the pre-shared key.
This introduces a privacy concern in TLS.

# TLS Protocol Ossification
TLS is widely implemented but there are many poor quality implementations. Some TLS connections fail if the clienthello uses an unexpected version number. Some others fail if TLS 1.3 structure is used instead of 1.2 or earlier even though 1.3 is signalled.
When TLS 1.3 was first implemented they changed how the clienthello message was structured which caused ~8% of TLS connections to fail.

Later designs of TLS 1.3 changed to work around these bugs. The version number in the Clienthello now says 1.2, with an extension header that says "I'm actually 1.3".
When a 1.3 client contacts a 1.3 server the version is negotiated in extensions.
When a 1.3 client talks to a 1.2 server the extension is ignored and TLS 1.2 is negotiated.

**Protocol ossification is a significant concern and TLS is not the only protocol to include such workarounds.** 

## Avoiding Protocol Ossification
**GREASE - Generate Random Extensions And Sustain Extensibility**
- Send meaningless dummy extensions that are ignored
- Change the version number just to prove you can
- Do it even if you don't need to - use it or lose it

# Security
### **Distance can be inferred based on packet timing:** 
By seeing the RTT of the TLS handshake, hackers can infer your distance away from a server using the speed of light. If the time difference between the Clienthello and Serverhello is, for example, 100ms, they know that the one-way travel time is 50ms. From this the hacker can use d=vt to find your approximate distance from the server. In this example they would know that you are within a 10,000 radius of the server. This is not extremely useful as they can't triangulate your position with only one server. If they can see you communicating with multiple servers however, that can triangulate your position by overlapping the radii from each server. However, this is a very rough estimate, as routing inefficiencies, processing delays, and congestion can all slow down a TCP handshake and mess with the calculation. Timing analysis does do a good job of finding your maximum possible distance from a server.

### **Server Name Indication Field is unencrypted:** 
Since the Server Name Identification Field is unencrypted to let the server know the domain of the website it should pull up. This contains the domain name of the website being visited. They can combine this information with packet timing data (and inferred geolocation), to build a profile of your life. They can see when you wake up, go to work, what your hobbies are, and how much time you spend using specific services. They would know if you visited a cancer support website or a suicide prevention website. They can use this information to manipulate and control individuals.

### **TCP Reset Attack:**
If control data is visible to hackers they can jump into the connection with a spoofed IP and accurate Sequence Number and trick the server into closing the connection. The hacker can do this by spoofing the client's IP and sending a RESET packet to the server with the Sequence Number that the server is expecting next from the client, making the server think that the client has left and closing the connection, causing whatever operation was happening between the client and server to fail. This cannot happen in QUIC because the header is encrypted.

### **TCP SACK Attack:** 
A **selective acknowledgement** is a message from the receiver to the sender when data is missing. The **Cumulative acknowledgement** tells them "I have received everything up to packet x", and the selective acknowledgement may say "I have also received packets y through z" so the sender only has to retransmit packets x through y. Since a hacker can see the sequence numbers they could send a false spoofed SACK telling the sender the receiver received everything perfectly. The receiver would then tell the sender they are missing packets, but since the sender would already have stopped sending this would break and corrupt the transmission. Historically hackers have also used SACK attacks to send engineered sack blocks that would take so much processing power they crash the server.

### **Explicit Congestion Notification:** 
**ECN** stands for Explicit Congestion Notification. It is used to notify the sender when the receiver's network is getting congested but not yet sufficiently overloaded that packets need to be discarded, to tell it to slow down transmission to avoid packet loss. When they perform the TCP handshake the sender sets the **ECN-Capable Transport (ECT) bit** to true to indicate that they support ECN. When the packet gets sent through the network, if the receiver's router is experiencing congestion it will change this bit to **ECT-CE (Congestion Experienced)** before it forwards it to the receiver. The receiver will see that the bit has been set to CE and immediately set the **ECN Echo (ECE) bit** in the segment it generates to send back to the sender. When the sender receives the segment with the ECE bit set, it reduces its **TCP congestion window** to slow down transmission and avoid packets being lost the same way it would if a packet was lost.

**Why is ECN useful for interactive video conferencing Applications**
ECN feedback allows for the sender to react to congestion early and avoid UDP packet loss. This improves the quality of audio and video, as UDP is unreliable and doesn't retransmit lost packets.
This leads to the router queues being less full, which reduces queueing latency in the Network. This is beneficial for interactive video conferencing since latency makes an interactive call difficult.

### Preventing Ossification
QUIC prevents protocol ossification by using encryption to block devices in the network from viewing control information. Preventing network devices from seeing control information means they can't build rules around the data. They must just let the data through because they can't understand it. This also prevents network devices from seeing state information, which means they can't build rules around that either. This means that if engineers want to change how QUIC handles connection states or control in the future they can do so without breaking anything because the network devices never understood it in the first place.

**TLS version negotiation:** This was a downfall of TLS v1.3, When TLS v1.3 was first released many middleboxes rejected TCP connection requests with the new header. Approximately 8% of new TCP connections failed. The fix for this was to make the header of TLS v1.3 packets still say v1.2 and then add a header extension that says "I'm really v1.3". This happened because network devices built rules around what they learned to expect. They ossified in the old system and now the internet can't be upgraded without breaking.

# QUIC
**Downsides of TLS v1.3 over TCP**:
Slow to connect due to sequential TCP and TLS handshakes
Leaks some metadata
Highly ossified and hard to upgrade

**Goals of QUIC:**
QUIC aims to replace TLS over TCP with a single secure transfer protocol
Reduce latency by overlapping TLS and transport handshake
avoid metadata leakage via pervasive encryption
Avoid ossification via systematic application of GREASE and encryption

## QUIC overview
**QUIC** replaces TCP and TLS as part of HTTP
- HTTP -> stream multiplexing
- TLS -> security
- TCP -> reliability, ordering, congestion control
- Runs on UDP for ease of deployment
![[image-19.png]]

![[image-20.png]]


## QUIC Packets and data streams
QUIC packets are made up of one or more Frames and sent within UDP datagrams.
These datagrams are then sent over UDP/IP data streams.

Data streams in QUIC have 64 bit long IDs, 2 bits of which are for the header. one bit to specify the source (client/server) and one bit to specify the directionality (unidirectional/bidirectional).
This means that the QUIC protocol can theoretically support $2^{62}$ (~4 quadrillion) simultaneous data streams for in each direction for a single connection.
### QUIC Packet Headers
QUIC Packets can be long header packets or short header packets:

**Long header packets** are used to establish QUIC connections. There are four types of long header packet: 
- **Initial:** indicates connection, starts TLS handshake
- **0-RTT:** idempotent data sent with initial handshake when resuming a session
- **Handshake:** Completes connection establishment
- **Retry:** used to force address validation
Several Initial, 0-RTT, and handshake packets can be included in one UDP datagram, one after the other, followed by a short header packet

**Short header packets:** There is only one short header packet defined in QUIC. This is the **1-RTT** packet and it is used for all packets after the initial TLS handshake. The short header is followed by QUIC frames in the enclosed UDP packet. It omits information that the long header has as it can be inferred from context.
### QUIC Frames
QUIC packets contain an encrypted sequence of **Frames**
- **CRYPTO** frames are used to carry TLS messages
- **STREAM and ACK** frames send data and acknowledgements
- **PATH_CHALLENGE and PATH_RESPONSE** support migration between two network interfaces
- other frames control the progress of a QUIC connection

Compared to TCP, QUIC headers are limited in scope. Functionality is provided by frames instead, which is more flexible.
**TCP send sequence numbers and acknowledgements in the header; QUIC sends this informationin STREAM and ACK frames**

## Using encryption to protect privacy and prevent ossification
QUIC encrypts transport headers to protect privacy and avoid metadata leakage
### Protecting Privacy
Encryption of transport headers avoids metadata leakage that could expose sensitive data about an individual to a third party.

# QUIC
## QUIC connection establishment and data transfer
A **QUIC** connection has two phases: handshake and data transfer
**Handshake** uses long headers and established the connection
**Data transfer** uses short headers and sends data and acknowledgements
It essentially overlaps TCP and TLS to make a 1-RTT handshake + connection establishment
## QUIC Connection Establishment
- **QUIC** Estanlishes connections and performs the TLS Handshake in **ONE ROUND TRIP**
- Initial Client - server: Initial QUIC CRYPTO containing TLS clienthello
- Server - client: Sends two packets. One CRYPTO that contains the serverhello and one handshake packet that contains connection information, both send in a single UDP datagram
- Final Client - Server:  initial ACK that acknowledges the server's initial crypto packet, handshake that contains the CRYPTO which says TLS Finished, and a QUIC 1-RTT short header packet that contains initial data from client to server
### Initial Packet
The initial packets contain both the TLS "hellos" and synchronize the client and server's state **similar to** the TCP ACK and SYN+ACK and may carry and optional token that can be used to prevent spoofing.
### handshake
The handshake packet is used to complete the TLS exchange ("Finished").
### 0-RTT connection re-establishment
QUIC supports 0-RTT connection re-establishment.
The Initial packet can contain both the clienthello and SessionTicket ("A pre-agreed upon secret key") which can be used to immediately re-establish a QUIC connection
A **0-RTT** short header packet can be sent in the same UDP datagram containing a request from the client
The Server would respond with its own initial CRYPTO (serverhello), handshake (SYN+ACK) and **1-RTT** short header packet in the same UDP datagram

## Data Transfer, Streams, and Reliability
After the handshake has finished, QUIC switches to sending only short-header packets.
QUIC **NEVER** retransmits packets. It only retransmits frames from lost packets in new packets with new packet numbers.
QUIC performs **Congestion Control** the same as TCP

QUIC acknowledges received packets in ACK frames within packets - not within the header like TCP

The encrypted QUIC frames are stored within the payload section of a packet
Data is stored in STREAM frames within packets. Each one contains a stream ID, offset, data length, and data.
Data for each stream is delivered reliably and in order.
Order is not preserved between streams.

## QUIC over UDP
### Why run QUIC over UDP rather than IP?
To ease deployment in user-space applications. It is easier to build a user space application with UDP than IP as they have a standard API and don't require privileged kernel access.
IP implementations would have to run inside the kernel which would require root privilege and be difficult to update.
It is also to work around ossification as firewalls don't block TCP or UDP.

## Ossification
**If a field protocol is visible to the Network someone will build a middlebox that requires its presence**
Once a protocol has been widely deployed it is very hard to change.

### Avoiding Ossification in QUIC
**GREASE:**  Generate Random Extensions And Sustain Extensibility
QUIC encrypts as much as possible in its packets which makes it unreadable by the network and impossible to ossify around
Every field is either encrypted or has a value that is unpredictable
QUIC authenticates ALL data so if a middlebox changes a header it will know

## Why is QUIC desirable?
It reduces latency of secure connection establishment
It reduces the likelihood of ossification
It is easy to deploy and update
It supports multiple streams within a single connection

## Why is QUIC problematic
Libraries to support it are new and often buggy
CPU usage is high compared to TLS over TCP
This will improve over time but it will take some time before QUIC is as performant as TLS over TCP