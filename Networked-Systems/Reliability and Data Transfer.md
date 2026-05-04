The internet is unreliable so transport protocols are used to improve the quality of service

**The End-to-End argument:** Only put functionality that is absolutely necessary in the network, and leave the rest to End Systems
# Timeliness vs Reliability
**Repairing lost packets takes time.**
There is a fundamental trade-off: reliable connections cannot guarantee timeliness (TCP), and timely connections cannot guarantee reliability (UDP).
# UDP
## Sending Datagrams
Data is sent over UDP using **Datagrams**
The sendto() function calls a single datagram
Alternatively you can connect() to an address and then just call send()
## Using Datagrams
recv() is used to read a single datagram but doesn't provide the source address
Most code uses recvfrom() instead which fills in the source address of the received datagram
## UDP Framing and Reliability
# TCP
## TCP Acknowledgements
### Loss Detection
TCP uses acknowledgements to detect lost segments. If data is sent but no acknowledgement returns, it means that either the receiver or the network has failed. TCP creates a **Timeout** as an indication of packet loss and retransmits the lost segments.
If some data arrived but some data was lost TCP will send a triple acknowledgement - **Four consecutive acknowledgements for the same sequence number** - as an indication of packet loss. The lost segments are retransmitted.

**Why a triple acknoledgement?:** Packets being delayed causing one or two duplicate ACKs is relatively common, but being delayed enough to cause three is rare. This balances speed of loss detection with the likelihood of retransmitting a packet that was only delayed. 3 repeats gives enough time for consecutive packets to be reordered, which is common, but support of further reordering in not necessary.

## Head of line blocking
A TCP receiver will always wait for data to be retransmitted. It will block recv() until the missing data arrives, and it will always deliver data to the application in order.

![[image-21.png]]
### Week
# QUIC service model
Whilst TCP delivers an ordered reliable byte stream, QUIC delivers *multiple* ordered reliable byte streams.
## QUIC packets and sequence numbers
Every packet in QUIC has a **packet number**.
- There are **two** packet number spaces.
	- Packets sent during the **initial** handshake
	- Packets sent to carry **data**
- Within each space the packet number starts at **0** and increments by **1** for each packet sent.
- Every QUIC **Packet** contains one or more **frames**
	- the **STREAM** carries the data
	- Every stream has a **stream identifier** which carries the length of the data and its offset from the start of the stream.

- **QUIC** doesn't preserve message boundaries in streams.
	- If **data** written to a stream is too **big** for a packet, it will be split across multiple packets.
	- if **data** written to a stream is too **small** for a packet the data may be delayed to fill a following packet
	- **data** from more than one **stream** might be sent in a **single** packet
	- if **more** **than** one stream has data to send the QUIC sender chooses how to prioritize and deliver frames from each stream.
## QUIC Acknowledgements
- A **QUIC** receiver acknowledges **packets** it receives. 
	- Unlike **TCP** it **does not** acknowledge sequence numbers within each stream.
	- The **sender** must remember what data from each stream was in each packet to know what parts of the stream have been received.

**Acknowledgements** are sent as **reverse path frames** and can be **delayed**. The delay is measured using the **ACK delay** field.
## QUIC re-transmissions and loss recovery
QUIC does **NOT** retransmit lost packets. Packets each contain a unique packet number, and much like in TCP, after three missed ACKs are considered lost.
QUIC retransmits this lost data in **new packets**.

## Head of line blocking in QUIC
QUIC delivers **several ordered reliable** byte streams within a single connection. It suffers head of line blocking much less than TCP, as lost packets can only affect the stream they are in.
Order is not preserved **between** streams in a QUIC connection. One stream might be blocked waiting for re-transmission, while other streams continue delivering data. Careful use of streams can limit head-of-line blocking.

