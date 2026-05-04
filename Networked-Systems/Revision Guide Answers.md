### 1. The Evolution and Security of DNS

**Why DNS is moving to TCP:** Originally, UDP was chosen for DNS because it is connectionless and avoids the overhead of a three-way handshake, making small queries very fast. However, modern DNS resolvers increasingly use TCP because DNS responses have grown larger (due to extensions like DNSSEC), and UDP cannot securely handle the encryption needed for modern privacy standards.

**The DoH vs. DoT Controversy:**

- **DNS over TLS (DoT):** Runs over a dedicated port (Port 853). This allows network operators and governments to easily identify DNS traffic and block it if it violates policy.
- **DNS over HTTPS (DoH):** Blends DNS queries into standard encrypted web traffic over Port 443. This is highly controversial because it prevents network operators from distinguishing between regular web browsing and DNS lookups, allowing users to evade legitimate access control mechanisms (like enterprise firewalls or parental controls).

**Root Servers:** Root servers sit at the top of the DNS hierarchy. Resolvers find them using a pre-configured list of "root hints." They use **Anycast IP addresses**, meaning multiple physical servers globally share the exact same IP address, allowing the network to route the user's query to the topologically closest server. A recurring exam concern is that, for historical reasons, most of the 13 root server operators are US-based organizations, raising geopolitical and jurisdictional concerns regarding a globally distributed system.

### 2. Modern Transport Protocols and Protocol Ossification

**Protocol Ossification:** In the original Internet design, end systems chose their transport protocols. Today, the deployment of new transport protocols is severely hindered by middleboxes (like NATs and firewalls) that are programmed to only recognize and forward TCP and UDP traffic. Any packet with an unknown protocol header is simply dropped.

**The QUIC Workaround:**

- To bypass these restrictive firewalls, QUIC was built to run on top of UDP, which middleboxes already allow.
- Unlike TCP, where sequence numbers and control bits (SYN, FIN) are sent in the clear, QUIC encrypts its transport header information. This prevents middleboxes from reading, tampering with, or making routing decisions based on the transport headers, ensuring the protocol can be upgraded in the future without middleboxes breaking it.
- **GREASE:** This is a technique combined with pervasive encryption to avoid future ossification. It involves intentionally sending unrecognized or dummy extension values during protocol handshakes (like TLS) to force middleboxes to tolerate unknown values rather than breaking when new features are eventually introduced.
### 3. Multimedia Performance vs. TCP Fairness

**UDP for Video vs. TCP Interference:** Video traffic is inherently bursty, oscillating between upper and lower bounds depending on the compression of moving frames versus static backgrounds. These applications use UDP because they require real-time delivery; TCP's requirement to acknowledge packets and retransmit lost ones causes unacceptable latency and jitter for live conversations.
**The "TCP Download" Scenario:** When you run a Voice-over-IP or video call (UDP) over an otherwise idle link, packets arrive on time. However, if you start a large file download using TCP (especially with an aggressive congestion control algorithm like Cubic), TCP will continually increase its transmission rate until it fills the network path's capacity.
- As TCP probes for bandwidth, it fills the memory buffers in the router.
- Your UDP video packets get stuck waiting at the back of this massive queue, causing a massive spike in latency and jitter (a phenomenon known as bufferbloat).
- Reducing the amount of memory (buffer space) in the router forces TCP to drop a packet sooner, which signals TCP to slow down before a massive queue forms, thereby saving the real-time performance of the video call.
### 4. Advanced Routing and Private Infrastructure

**BGP vs. Private Networks:** BGP (Border Gateway Protocol) is used for inter-domain routing between Autonomous Systems (AS) on the public Internet. However, large tech companies (Amazon, Google, Netflix) measure significantly lower latency across their private fiber networks compared to public BGP paths between the exact same cities.
- This occurs because BGP does not route based on the fastest or shortest geographical path; it routes based on AS-path length and business policies (such as the Gao-Rexford rules, which prioritize customer routes over peer/transit routes to save money).
- Private networks, completely under the control of one company, can route purely for optimal performance and lowest latency, bypassing public peering chokepoints.

**BGP Security:** BGP relies on trust, making it vulnerable to **prefix hijacking**, where a malicious or misconfigured network advertises a route to IP addresses it does not own. The **Resource Public Key Infrastructure (RPKI)** helps prevent this by using cryptographic certificates to mathematically prove that an Autonomous System is authorized to announce a specific IP prefix.
### 5. Connection Establishment and Latency Reduction

**TLS 1.3 0-RTT:** 0-RTT (Zero Round-Trip Time) connection re-establishment allows a client to send encrypted application data in its very first communication with a server it has previously connected with. This drastically improves performance by skipping the standard handshake. However, the inherent risk is **replay attacks**; an attacker could intercept the initial 0-RTT packet and resend it to the server multiple times, potentially causing the server to execute an action (like a database change) more than once.

**HTTP Server Push:** This HTTP feature allows a web server to proactively send data (like CSS or JavaScript files) to the client before the client even realizes it needs to request them.

- **Benefit:** It reduces the time it takes to render a full webpage by eliminating the round-trip delay of the client discovering and requesting those sub-resources.
- **Risk:** It can actually hurt performance and waste bandwidth if the server pushes resources that the client already has stored in its local browser cache.