# Real-time traffic on the internet
Many transfers over the internet are **elastic**. This means that faster is good but the application can adapt to any chosen rate.
**Real-time applications are inelastic**. This means they have a maximum and minimum rate, above and below which they cannot be used.

## Quality of Service guarantees
**IP Packets** contain a DSCP field in the header which tells the network if the packet is real-time, which allows the network to prioritize real-time packets.
**Network Operators** can reserve capacity for real-time traffic, typically at a cost to the sender.

# Interactive Applications
## Constraints
Mouth-to-ear delay from sender to receiver should be <150ms to lip-sync audio
Each frame of speech is 20ms.

Speech data is highly loss-tolerant and can take 10-20% loss without a noticable drop in quality with loss concealment.
Video packet loss is harder to conceal. Lost packets result in jitter.

# Media Transport
**Audio and Video** data are compressed and sent inside secure **RTP** packets, and security is provided by **Datagram TLS**. RTP packets are sent **within** UDP packets.

## Media transmission path
1. Frames of media data are captured periodically
2. Codec compresses media frames
3. Compressed frames are fragmented into packets
	1. RTP protocol adds timing, sequencing, source identification, and payload identification in UDP
4. Packets are transmitted over the network
![[image-22.png]]

## Media reception path
1. UDP packets arrive
2. The **channel coder** repairs loss using forward error correction
	1. Some additional packets are sent along with the media to allow for some repair without retransmission
3. The **Playout buffer** is used to reconstruct order and smooth timing
4. Media is **decompressed**, **packet loss** is concealed, and **clock skew** is corrected
5. Recovered media is played out to user

## Timing Recovery
Variable queueing delays dirsupt timing, so the receiver has to buffer to smooth out the variation in received packets
![[image-23.png|458]]
![[image-24.png]]

Data needs to be **buffered** rather than playing immediately on arrival to smooth timing gaps.

## Application Level Framing
- RTP **payload formats** determine how compressed audio/visual data is formatted into RTP packets.
	- goal: each packet should be independently usable
	- If a packet arrives, it should be possible to decode all the data it contains
	- It is poor practice to make packets dependent on one another in case one of the packets gets lost

## FEC vs. Retransmittion
Forward Error Correction is often used because Retransmittion often takes too long. FEC packets are sent alongside the original data which contain **error correcting codes**. If some original packets are lost but the FEC packets arrive, the original data can be reconstructed.


# Signalling
- Media transfer flows peer-to-peer for low latency.
- The **signalling protocol** is needed to find the peer and establish the paths
- The control protocol needs to describe the communication session expected
	- Media transport connections are required
	- You need media formats and compression algorithms
	- You ned the IP addresses and the right ports to use
	- You need the Originator and purpose of the session
	- And you need options and parameters

# SDP Offer/Answer
**SDP** stands for **Session Description Protocol**
- Interactive Sessions Require Negotiation
	- An **offer** to communicate: list codecs, options and addressing details, identity of caller
	- The **answer** subsets codecs and options to those mutually acceptable, supplies addressing details, and confirms willingness to communicate
	- NAT traversal algorithms are used to find candidate addresses
- **SDP** is used as the negotiation format
	- SDP was not designed to express options and alternatives
	- It has insufficient structure in syntax and semantic overloading
	- It is complex but the complexity is not immediately visible. It is now too entrenched for alternatives to take off

## Control of Interactive Conferencing: SIP
**SIP** stands for **Session Initiation Protocol**. It is used for telephony and video conferencing. This protocol is what sets up the calls. Conferencing servers with public IP addresses handle calls for a domain. To initiate a connection,  SIP messages are sent from initiator to responder via servers. Data such as location, NAT bindings, and the parameters for the call are sent over this connection. NAT binding discovery and connection probing take place whilst the responding user is being alerted. Media then flows over a direct peer to peer path.
![[image-25.png]]

## Browser Based Conferencing: WebRTC
**WebRTC** is an alternative signalling protocol.
It is implemented in web browsers using the standard JavaScript API. Signalling messages are delivered via HTTP to the web server controlling the call. Offer/answer exchanges are performed using SDP.
**Media Transport** uses RTP with a peer-ro-peer data channel.
This is designed to integrate video conferencing into web browsers and applications.
![[image-26.png]]

# Streaming Video Applications
**RTP** should be used for streaming video, but most video services use HTTPS instead. RTP is unidirectional, very low latency, and loss tolerant, whilst HTTPS is high latency but ossified in the network.

## HTTP CDNs
Content Distribution Networks only work with HTTP-based content. They do not support RTP-based streaming.

## HTTP Adaptive Streaming (MPEG DASH)
This **Delivers video over HTTPS** to make use of content dirstribution networks.
- Each video is encoded in multiple **chunks**
	- Each Chunk encodes **~10 seconds** of video
	- Each chunk is independently decodable
	- Each chunk is made available in many different versions at different decoding rates
- A **manifest** file provides an index for the chunks that are available
- Clients fetch the manifest and download chunks in turn
	- Standard HTTPS downloads
	- Adaptive bitrate
	- Plays out chunks in turn as they load

## DASH rate adaption
- Each chunk is compressed multiple times at different encoding rates
- The receiver fetches the manifest containing the list of all the versions of each chunk
- The receiver fetches each chunk in turn
	- Measures the download rate compared to encoding rate
	- If download rate slower than encoding rate, switch to lower encoding rate for next chunk
	- If download rate faster than encoding rate, consider fetching higher rate for next chunk
- Chooses encoding rate to fetch based on TCP download rate

## DASH and TCP congestion control
- DASH  rate adaption and TCP congestion control work at different time scales
	- TCP adjusts congestion window once per RTT
	- The DASH receiver measures average download speed of TCP connection over ~10 seconds
		- Selects size of next chunk to download based on average TCP download speed
- Videos played  using DASH often have relatively poor quality for the first few seconds
	- The receiver picks a conservative doanload rate - relatively poor quality and small size - to start
	- It uses that to measure download speed, and then switches to a more appropriate rate

## DASH Latency
- Multi-second playout latency common due to:
	- Chunk duration
	- Playout buffering
	- Encoding delays for live streaming

- **Chunk duration is key** - you must download a complete chunk before starting playout
	- Download and decompresss chunk n
	- Start playing chunk n and while playing download chunk n+1
	- If no packets lost then latency equals chunk duration

# Sources of Latency
## TCP
If a packet is lost then TCP retransmits after a triple ACK which takes time and delays chunk download. Each packet loss and retransmission increase chunk download time and reduce the TCP congestion window.

## Compression
Each chunk of video is independently decodable. You can't compress based on the previous chunk because the previous chunk downloaded varies with network capacity.

Every chunk starts with an I-frame followed by P-frames
I-frames are large - often 20x the size of P frames
If chunks are smaller duration then there will be fewer P-frames compared to I-frames and the video compression rate will go down

There is a tradeoff between chunk size and compression overhead
Large chunks require less data but add latency
Small chunks reduce latency but need more data
Overheads become excessive for chunk duration <2 seconds