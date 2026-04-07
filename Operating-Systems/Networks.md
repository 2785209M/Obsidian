A **Distributed System** is a collection of loosely coupled nodes interconnected by a communications network. Nodes are variously called processors, computers, machines, hosts. A **Site** is the location of a **Node**.

# Motivations for distributed systems
- Resource sharing
	- Processing information in a distributed database
	- sharing files
	- Remotely using specialized hardware devices such as GPUs
- Computation speed-up
	- distribute dub-computations to run concurrently between sites
	- Move jobs from high loaded to low loaded sites
- Reliability
	- detect and recover from site failure remotely

# Local Area Network (LAN)
**LAN** is designed to cover a small geographical area
- consists of multiple computers (Laptop, phone, PC), peripherals (Printer, storage), and routers
- Ethernet and WiFi are common ways to construct LANs
	- Standard IEEE Ethernet provides speeds from 10Mbps to 10Gbps
	- IEEE standard WiFi provides speeds from 11Mbps to 400Mbps
	- Both standards are constantly evolving

# Wide Area Network (WAN)
- Wide area networks cover a wide geographical area and link geographically separated sites
	- Telephone lines, data lines, optical cable, microwave links, radio waves, satellite, etc.
- Implemented via routers to direct traffic from one network to another
- Internet WAN enables hosts world wide to communicate
- Speeds vary
	- Many backbone providers have speeds of 40 to 100Gbps
	- Local Internet Service providers may be slower
	- WAN links are constantly being upgraded
- WANs and LANs interconnect
	- Cell phones use radio waves to cell towers
	- Towers connect to other towers and hubs


# What does this have to do with the OS
The OS manages resources and hardware. This includes:
- The networking software stack
- The networking hardware devices
Network software and hardware are executed in the kernel in privileged mode.
Many independent OS applications want to use the networking stack so the OS must manage access.
![[image-29.png]]


# The OSI Layer Model
![[image-30.png]]

# TCP/IP
In the typical TCP/IP communication stack presentation and session are not present as distinct layers.
![[image-31.png]]


# The Linux Networking Stack
- System calls are used to communicate between the Transport and Application layer
- The transport layer is protocol-agnostic in regards of the application
- The network protocols talk through sockets
- The device driver is also showing a device agnostic interface to the link layer while the driver itself is device-specific
- "Everything is **NOT** a file"
![[image-32.png]]


# Sockets
## What is a socket?
- A socket is an endpoint for communication
- The socket() syscall returns a file descriptor (a small integer) that represents such an endpoint
- Each endpoint must be bound to Internet Address information and then two endpoints must be connected to enable information flow between two endpoints
```
#include <sys/socket.h>
int socket(int domain, int type, int protocol);
```
- The **domain** argument specifies a communication domain. It selects the protocol family to be used for communication. We will restrict ourselves to AF_INET, the IPv4 internet protocols
- The **Type** arguments specify the communication semantics. We will restrict ourselves to:
	- SOCK_STREAM - sequenced, reliable, two-way connection-based byte streams
	- SOCK_DGRAM - supports datagrams, which are connectionless, unreliable messages of a fixed maximum length
- The **Protocol** argument specifies a particular protocol to be used with a socket. Normally, only a single protocol type exists to support a specific protocol type within a given protocol family, in which case this argument is specified as 0.
## Using Sockets (stream)

| Server        | Client        | Description                                    |
| ------------- | ------------- | ---------------------------------------------- |
| socket()      |               | Server creates a socket descriptor             |
| setsockopt()  |               | Configure any protocol options                 |
| bind()        |               | Associate server socket with local port number |
| listen()      |               | Enable connection requests from clients        |
|               | socket()      | Client creates a socket descriptor             |
|               | connect()     | Client requests connection to server           |
| recv()/send() | recv()/send() | Exchange Data                                  |
| close()       | close()       | Either process can close the connection        |

## Using Sockets (Datagram)

| Server       | Client     | Description                                    |
| ------------ | ---------- | ---------------------------------------------- |
| socket()     |            | Server creates a socket descriptor             |
| setsockopt() |            | Configure any protocol options                 |
| bind()       |            | Associate server socket with local port number |
|              | socket()   | Client creates a socket descriptor             |
|              | bind()     | Associate client socket with local port number |
| recvfrom()   | sendto()   | Exchange Data                                  |
| sendto()     | recvfrom() | Exchange Data                                  |
| close()      | close()    | Each process closes when finished              |
**Client Process:**
- Create a socket
- Bind it to a local port
- Talk to the server
**Server Process**:
- Create a Socket
- Bind it to a Local Port
- **Forever:**
	- Receive Datagram
	- Send Datagram

## Socket Buffers
- Remove complexity of adding/removing headers from packets between protocols
- Links control with memory
- A pointer + field length
- Allows direct data manipulation
![[image-34.png]]


# Common POSIX Socket API Functions
![[image-35.png]]

# Network and host order
- When sending data, different processors use different in-memory representations
	- Big Endian and Little Endian