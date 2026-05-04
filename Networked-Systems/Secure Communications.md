[[Security]]
# DoS Attacks
A Denial of Service attack is a type of cyber attack that aims to exhaust a target system of its resources to block legitimate users from using the system.
DoS attacks cannot directly be used to steal data but they are often use as a smokescreen for other malicious activity, or as a ransom. A DoS attack could cripple a business if their primary servers are brought down.

## Types of DoS Attack
*There are three main ways to perform a DoS attack:*
**1. Direct Attack**
In a direct attack the hacker uses a single source device with a real IP address and can be easily traced.
**2. Spoofed Attack**
In a spoofed attack the attacker uses a fake IP address. Even if the IP is fake the real IP can be tracked down with the help of ISPs.
**3. Ditributed Attack**
In a distributed attack the hacker uses uses a network of corrupted private machines to attack a single target. The IPs of these machines may also be spoofed. This is the hardest kind of attack to trace.

*There are also three main types of DoS attack:*
### Fork() Bomb (Local Resource Exhaustion)
A fork bomb is a type of DoS attack that aims to overload the system's process table and consumes RAM and CPU. Once the process table is full the machine cannot start any new processes. On Unix-like systems this is done by a process using the fork() command to infinitely copy itself until the process table is full and the machine freezes.
### OS Mitigation
**Process Limits:** The OS can set process limits (ulimit() in Linux) to limit the amount of user processes that can run on a system. When the fork() command is called, the system will simply check if the limit has been reached, and if it has, the OS will deny the system call.

### Disk Space Exhaustion (Local Storage Exhaustion)
In a shared user environment a user can, accidentally or on purpose, write massive amounts of data to the disk until it is completely full. A hacker could also create millions of empty files to exhaust the system's inodes, which are the structures that hold information about files.
If this happens in a **shared** folder, no user will be able to run any applications that require disk writing. If this happens in the **root** folder, the entire operating system may crash or become unresponsive.

### OS Mitigation
**Disk Quotas:** The administrator can set a hard limit on how much data/inodes that an individual user is allowed to use on the system. Once a user hits their maximum number of writes, the OS will return a "Disk quota exceeded" error and prevent them from writing any more to the disk.
**Partitioning:** Core system directories like **root** can be saved to a separate partition to the one accessible by users. This means that even if a user manages to completely fill the user partition, the OS root partition will still be safe and functional.

### TCP SYN Flood (Remote Network Exhaustion)
Hackers can usurp the TCP connection handshake to overwhelm a server's Networking stack. Hackers can, usually spoofing trusted IPs, send millions of SYN requests to a server and never send the closing ACK. As they are never completed, these connections will stay in the server's memory until they time out. This leads to the server's connection backlog queue filling up and no legitimate users being able to connect.

### OS Mitigation
**Recycling old connections**: If the connection queue gets full the OS can close the old half of the half-complete connections to be reused for new connections.
**SYN Cookies:** When the server receives a SYN from a host, it can respond with an encoded message. It puts the initial connection information of the handshake inside the initial sequence number, combined with a **cryptographic hash** generated from a combination of the Client's IP and port, its own IP and port, and a *Secret Key* known only to itself. It then attaches this to the header of the SYN+ACK packet and sends it back to the host. It does not keep the connection open after this. **If the host is a hacker** they will never receive the SYN+ACK as it would have been sent to the innocent IP they spoofed. **If the host is legitimate** they will return an ACK to the server containing the cryptographic hash that it originally encoded. Once the server receives the ACK it will then decode this hash to make absolutely sure the host is legitimate (because the host could not have gotten the hash from anywhere but itself) it will reopen the connection and complete the TCP handshake.
**Firewalls:** Block specific IP addresses or IP address ranges known to be malicious

#  Monitoring internet traffic
There are many organizations that monitor internet traffic. This includes governments, intelligence agencies, and law enforcement.
There are good reasons to monitor some network traffic, but Edward Snowden showed that **all** traffic is being pervasively monitored.
Other notable monitors include businesses, network operators, and **criminals**.
![[image-6.png]]

# Protecting privacy
Mechanisms that protect monitoring from hackers also prevent monitoring from law enforcement.
Methods that permit law enforcement monitoring risk being usurped by hackers.

# Protecting message integrity
Mechanisms to protect messages being interfered with maliciously also prevent benign interference
Web browsers deploy DNS-over-https to ensure the integrity of DNS responses. This protects against phishiing attacks that modify DNS replies and stops ISPs from modifying DNS responses to prevent access to domains hosting illegal material.
**There is no technical way to distinguish beneficial integrity violations from malicious attacks.**

# Preventing protocol ossification
Network operators deploy **middleboxes** to monitor and control traffic. This includes NATs, firewalls, monitors, etc. Middleboxes must understand the protocols being used. They can't control traffic they don't understand.

The increased use of middleboxes is causing **protocol ossification**. Protocols can't be changes because they wouldn't work with the current middleboxes. The more of a protocol that is encrypted the easier it is to change, but the harder it is for middleboxes to provide useful services.

# Security and Policy
## Encryption Key Escrow
There are strong reasons to protect privacy, prevent modification of data in transit, and prevent protocol ossification. But doing so can complicate the job of those performing legitimate network monitoring. There have been numerous proposals for key escrow and other forms of exceptional access.
**Key Escrow** is a security system where cryptographic keys needed to decrypt data are held by a trusted third party such as a corporation or government agency.
## End-system Content Monitoring
If data is encrypted during transit to prevent malicious attacks, should law enforcement be allowed to monitor endpoints? Does the answer change if the endpoints are personal devices or data centers? How do expectations for privacy, law enforcement access, and abuse protection differ between one-to-one conversations, group conversations, social networks, etc?

# Goals of Secure Communication
- Avoid eavesdropping
- Avoid Tampering
- Avoid Spoofing
![[image-8.png]]


## Providing Confidentiality
- Data traversing the network can be read by any device on the path
	- Configure a switch or router to snoop on data as it's forwarded between links
	- Can eavesdrop on packets as they traverse a link
- The network operator can **always** do this. If their network has been compromised others could be too.

Data can **always** be read. **Encryption** is used to make it useless if intercepted.
There are two basic approaches:
- Symmetric cryptography
	- Advanced encryption standard
- Public key cryptography
	- The Diffie-Hellman algorithm
	- The Rivest-Shamir-Adleman (RSA) algorithm
	- **Elliptic curve-based algorithms**
- Complex mathematics

# Symmetric Cryptography
- Symmetric encryption converts plain text into cipher-text
	- A secret key controls the encryption and decryption process
		- The same key is used to encrypt as is used to decrypt
		- Provided key is secret and only known to sender and receiver, so the conversation is secure
		- **Problem:** How to securely distribute the key
	- Very fast - suitable for bulk encryption
- Encryption and decryption algorithms are public
	https://nvlpubs.nist.gov/nistpubs/FIPS/NIST.FIPS.197.pd
	- US Advanced Encryption Standard (AES) is widely used – based on Rijndael algorithm, developed by Vincent Rijmen and Joan Daemen 
![[image-9.png]]
# Public key cryptography
- Public key encryption also converts plain text into cypher-text
	- The algorithms are public:  Diffie–Hellman, Rivest–Shamir–Adleman,
	Ephemeral Elliptic Curve Diffie-Hellman, …
	- Public key encryption uses **two** related keys:
		- The **public key** for a user is widely distributed
		- **The corresponding private key must be kept secret**
		- If one key is used to encrypt, the other is required to decrypt
	- This solves the **key distribution** problem
		- Look up the public key of the receiver in a directory
		- Sender uses the public key to encrypt the message -> can only be decrypted by the private key
		- If receiver is trusted to keep private key secret, only it can decrypt the message
- **Problem:** very slow to encrypt and decrypt
![[image-10.png]]
#  Hybrid Cryptography
Modern communications use a combination of both public-key and private-key cryptography for security and speed.
Public key cryptography is used to transmit the private key to the receiver, and from there symmetric cryptography is used.
This ensures confidentiality of communication with good performance.

# Authentication
Encryption provides confidentiality, but you also need to verify the identity of the sender and make sure data has not been modified in transit.
To do this you use a **digital signature** to identify yourself as sender and a **Cryptographic hash**

# Cryptographic Hash Functions
A cryptographic hash takes any input and generates an illegible output code. No two unique inputs can generate the same output.
It is infeasible to find the input given only the output.
The only way to decrypt the output is to use the same cryptographic hash algorithm that was used to encrypt it.
![[image-11.png]]
# Digital Signatures
## Signature generation
The sender attaches a digital signature to their message. This signature is generated by: 
- Generating a cryptographic has of the message
- Encrypting this hash with the sender's private key
- attaching this to the end of the message
- The message and digital signature are sent to the receiver using hybrid encryption
![[image-13.png]]
## Signature verification
The receiver verifies the digital signature:
- The receiver decrypts the message
- The receiver calculates the cryptographic hash of the message
- The receiver decrypts the digital signature using the sender's public key
- The receiver compares the two messages
If they match then the message is authentic and unmodified, provided the sender has kept their private key secret.
![[image-14.png]]

# Trust and public key infrastructure
**How do you know what public key corresponds to a particular receiver?**
- The receiver gave you their key in person
- The receiver sent you their key, authenticated by someone you trust
- Someone gave you the receiver's key
A **Public key infrastructure** can authenticate keys.
- The PKI can verify the identity of the sender, then attach its own digital signature to the sender's public key and forward it to the receiver
- If the receiver trusts the PKI, they can verify the sender by confirming the signature of the PKI
![[image-15.png]]


# Transport Layer Security
TCP is not secure. Neither TCP headers nor data are encrypted or authenticated.
TLS can be used to encrypt and authenticate data carried **within** a TCP connection.

## Goals of TLS
TLS only provides a secure channel between two communicating peers. It depends on an already reliable, in-order data stream between two peers.
The secure channel should provide the following:
- **Authentication:** Can be done through asymmetric cryptography,  **digital signature algorithm**, or  **symmetric pre-shared key**
- **Confidentiality:** Data sent between the channels can only be seen at the endpoints. It does not hide the length of the data it transmits although the endpoints can obscure this.
- **Integrity:** Data sent over a channel cannot be modified without detection

## Overview
TLS consists of two primary components:
- A **handshake protocol:** This authenticates endpoints and agrees on encryption keys with the communicating parties. 
- A **record protocol**: This protects the traffic between communicating peers. It divides traffic into a series of records which are individually protected using traffic keys.

## Handshake protocol
- TCP connection is established
	- syn -> syn+ack -> ack
- TLS handshake immediately follows
	- TLS clienthello sent with ack
	- TLS serverhello sent in response
	- TLS finished message concludes and carries initial secure data record
- Adds one **Round Trip Time** to connection establishment
![[image-16.png]]
### TLS ClientHello (Server -> Client)
The **ClientHello** message:
- Indicates the TLS version to be used
	- Signals TLS v1.2 in the main ClientHello message, but with an extension saying "I'm really TLS v1.3" because a lot of middleboxes break if the TLS version changes (*Protocol Ossification*)
- Provides the cryptographic algorithms the client supports
- Provides the name of the server to which the client is connecting
	- If connecting to a web hosting server, it's likely that more than one site is hosted on the same server, so need to specify which is intended
- **Does not contain any data**

### TLS ServerHello (Client -> Server)
- The **Finished** message:
- Provides the client's public key
- Optionally provides the certificate needed to authenticate the client to the server
- May contain data from client to server

## TLS v1.3 cryptographic algorithms
- TLS uses Ephemeral Elliptic Curve Diffie-Hellman (ECDHE) key exchange
	- Client sends its public key in **ServerHello**
	- Server sends its public key in **ClientHello**
		- The public keys are **Ephemeral**. This means new private keys are generated for each connection and new public keys are derived from them.
	- The client and server both combine the two public keys with their own private key to derive the key used for the symmetric cryptography
	- The server provides a digital signature to allow the client to verify its identity in the **ServerHello**, and the client can optionally provide this in **Finished**
- **The Client proposes symmetric encryption algorithms in ClientHello, then the Server picks from them and replies in ServerHello**
	- Either AES or ChaCha20 symmetric encryption
## TLS v1.3 Record Protocol
TLS v1.3 splits data into records, each containing $\le2^14$ bytes
- Each record is encrypted and authenticated. Has a sewuence number
- Can renegotiate encryption keys between records
- TCP does not preserve record boundaries. TLS adds a framing that does. reading from a TLS connection will block until a complete record is received
**Client and Server exchange records - send and receive data - then close connection**
## Limitations of TLS
TLS is secure but has limitations:
- It must operate within a TCP/IP connection
	- IP addresses and TCP port numbers are not protected. This exposes information about who is communicating and what app is being used
- The TLS handshake **does not encrypt the server name**:
	- This exposes the name of the server to which connection is being made in plain text
	- And encrypted SNI connection is in development
- Relies on PKI to validate public keys
	- Potential conerns about trustworthiness of the PKI
	- Who runs the PKI and certificate authorities and are they trustworthy?


# End to End Security
For communication to be secure, it must be **End to End**.
The two end points are the sender and the final recipient.
- Who is the final recipient with a **data center** or **CDN**?
	- Is it the load balancer at the entrance to the data center or the server within the data center that processes the request
	- If the request is directed to a content distribution network, is the end the local cache that serves the request? If so, how are the encryption keys shared?
- If the data is moving between two users, is it encrypted between the two users or between each user and the data center?
	- i.e. can the data center see user-to-user data flows?
	- This is normal if you run TLS from user-to-data center then from data center-to-user

Is there in-network processing? How much data is revealed to the in-network server?
- e.g. video conference with privacy protection vs without
- Does the central server decrypt the speech data, mix into one stream and send to the receiver, or does it forward all active streams in encrypted form?
- Trades off security vs bandwidth
	- For audio the bandwidth is small enough that this doesn't matter
	- For video conferencing the combined bandwidth may be significant

## The Robustness Principle (Postel's Law)
Balance interoperability with security - don't be too liberal in what you accept. A clear specification of how and when you will fail might be more appropriate.

"At every layer of the protocols, there is a general rule whose application can lead to enourmous benefits in robustness and interoperability:
"Be liberal in what you accept, and conservative in what you send"
Software should be written to deal with every conceivable error, no matter how unlikely. Sooner or later a packet will come in with that particular combination of errors and attributes, and unless the software is prepared, chaos can ensue. In general, it is best to assume that the network is filled with malevolent entities that will send in packets designed to have the worst possible effect. This assumption will lead to suitable protective design, although the most serious problems in the internet have been caused by un-envisaged mechanisms triggered by low-probability events; mere human malice would never have taken so devious a course!"

"Postel lived on a network with his friends. We live on a network with our enemies. Postel was wrong for today's internet." - Poul-Henning Kamp

- Protocol implementations **Must** have:
	- Robustness to Software defects
	- Robustness to attacks
	- Robustness to the unexpected

# Validating Input Data
- Networked applications work with data supplied by un-trusted third parties.
- Data from the network may not comform to the protocol specification.
- Due to ignorance, bugs, malice, or to disrupt services.
- **You must carefully validate all data before use**

# Writing Secure Code
The network is hostile. Any networked application is security critical.
- You must carefully specify behavior with both correct and incorrect inputs
- Must carefully validate inputs and handle errors
- Must take additional care if using type and memory-unsafe languages, such as C and C++, as these have additional failure modes
**The best encryption doesn't help if the endpoints can be compromised**