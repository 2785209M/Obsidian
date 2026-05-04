## 1. The Evolution and Security of DNS

The Domain Name System (DNS) is arguably the most consistent theme in the recent papers, specifically focusing on its modernization and security.

- DNS queries were traditionally sent using UDP, but recent questions emphasize the shift toward using TCP as a transport.
- The transition to encrypted DNS is heavily tested, particularly the differences between DNS over TLS (DoT) and DNS over HTTPS (DoH).
- The deployment of DoH is a major recurring topic because of the controversy it causes among network operators and governments.
- Examiners frequently ask why DoH faces specific criticism regarding the evasion of legitimate access control mechanisms, whereas DoT does not.
- The architecture of the DNS root servers is another common focus, including how resolvers find them, their use of anycast IP addresses, and concerns about the geographical concentration of root server operators.
## 2. Modern Transport Protocols and Protocol Ossification

The recent papers show a strong departure from basic TCP mechanics, moving toward how modern protocols overcome the limitations of the current Internet architecture.

- The inability to deploy new transport protocols due to network middleboxes blocking non-TCP/UDP traffic is a recurring concept.
- Because of this protocol ossification, questions frequently focus on QUIC, examining why it is built on top of UDP and how to handle firewalls that block QUIC's UDP traffic.
- The encryption of transport headers in QUIC, contrasted with unencrypted TCP headers, is tested as a method to prevent middlebox interference.
- The concept of GREASE combined with pervasive encryption is explored as a modern solution to avoid protocol ossification.

## 3. Multimedia Performance vs. TCP Fairness

Real-world application performance, particularly the friction between streaming media and large file transfers, is a major focus in the 2020–2024 era.

- Questions repeatedly test the bursty nature of multimedia traffic and why interactive video conferencing applications prefer UDP over TCP.
- A classic scenario presented in multiple exams involves analyzing what happens to a UDP-based Voice-over-IP or video call when a large TCP file download (often using Cubic or Reno congestion control) starts on the same network path.
- The mitigation of these performance issues is explored through router buffer sizing to prevent bufferbloat.
- Explicit Congestion Notification (ECN) and its integration with RTP for interactive video applications is also tested as a modern congestion signal.

## 4. Advanced Routing and Private Infrastructure

While older papers focused on basic distance-vector or link-state routing, recent papers focus on modern, large-scale infrastructure.

- A prominent recent theme is the rise of private global networks (operated by companies like Google, Amazon, and CDNs) and why these intradomain paths often achieve lower latency than public BGP interdomain paths.
- Content Distribution Networks (CDNs) are tested on their routing methodologies, specifically comparing DNS-based routing to anycast routing.
- BGP security remains relevant, with questions focusing on the Gao-Rexford filtering rules to enforce policy.
- Prefix hijacking attacks in BGP and the use of the Resource Public Key Infrastructure (RPKI) to prevent them are specific security topics you should review.

## 5. Connection Establishment and Latency Reduction

Optimizing the speed at which connections are made is a highly specific but repeated theme in the 2020–2025 papers.

- Exams ask how clients iterate through DNS results to establish TCP connections in parallel rather than sequentially.
- The TLS 1.3 0-RTT (Zero Round Trip Time) feature is tested, specifically regarding how it works, its performance benefits, and its inherent security risks.
- Theoretical modifications to the TCP three-way handshake to overlap with data transmission are discussed, alongside why they are problematic in practice.
- HTTP server push features are evaluated regarding their actual net benefit to performance metrics.

---