
# DNS
DNS stands for Domain Name System. It is a ditributed database that facilitates translation between domain names and IP addresses.

Everything between the dots in a domain name is called a Label. 
www.csperkins.org  -> www.csperkin are subdomains, whilst .org (the rightmost) is the top-level domain.

Top Level domains live within the DNS root zone and have, well-advertised, fixed IP addresses.

DNS is used for **Name Resolution**. Given a name, look up a particular record with relation to that name. A DNS Client asks its resolver to perform the lookup by calling getaddrinfo().

If the resolver has no information, it makes a recursive query. It starts by querying the root. If the root doesn't have an answer, it refers to the TLD nameservers. If they don't have an answer, it goes to the lower level nameservers. From there it should finally receive the answer.

Responses include a **time to live** that only lets the resolver cache the value for a set amount of time.

## IANA and ICANN
ICANN stands for the Internet Corporation for assigned names and numbers. ICANN was formed from a company called IANA which was run by just one guy called Jon Postel.

ICANN defines the set of legal top-level domains.

ICANN is a governance Model. It is Political. 

There are **Four** Types of Top-Level Domain:
- Country Code -> all 2 letter codes. 2 Letter codes are given to countries that are part of the UN, such as .uk. Beware if you see .su because that stands for soviet union, which doesn't exist anymore but is still an active domain, and is mostly used for Malware.
- Generic -> .com, .org, .net all have unrestricted use. There is a large set of these.
- Infrastructure Top-Level Domain -> such as .arpa. Mostly used for reverse DNS nowadays.
- Special Use -> .example, .invalid, .local, .localhost, .onion, .test

There are only **13 Servers** for DNS routing. But not really. There are many replicas. There are only 13 sets of IP addresses for them to use but there are about 2000 DNS servers.


# Methods of DNS Resolution
There are 5 main ways of making DNS queries:
• DNS over UDP
• DNS over TCP
• DNS over TLS
• DNS over HTTPS
• DNS over QUIC
The contents of the response are **identical** in all cases
They only change how the query is delivered and how the response is returned but the message remains the same
They do change the security gaurantees provided
They potentially give clients more flexibility to query different resolvers
## DNS over UDP - FOR FUTURE REFERENCE YOU DIDNT FINISH THIS
DNS queries aere typically made over port 53
Queries and responses are generally small enough to fit into a single packet.
TCP reliability isnt needed. If there is no answer just retransmit the query.
Congestion control isnt needed. You cant adjust the rate if you just send a single packet.
Base-64 encoded version of the data that would be sent in a DNS-over-UDP query
GET /dns-query?dns=AAABAAABAAAAAAAAA3d3dwdleGFtcGxlA2NvbQAAAQAB HTTP/1.1
Accept: application/dns-message

DNS over UDP is not secure. the packets are not encrypted or authenticated. Devices on the path between the client and the resolver can see DNS queries and responses and potentially forge responses.

![[image-29.png]]

**AAAA - Answer, Authority, and Additional information sections:**
List of domain names and record type, record data, and time-to-live
Answer - an answer to the previous query
Authority - Describes where the answer came from
Additional - Contains auxiliary information

![[image-30.png]]

```
james@james-laptop:~$ dig +trace csperkins.org

; <<>> DiG 9.18.39-0ubuntu0.24.04.3-Ubuntu <<>> +trace csperkins.org
;; global options: +cmd
.			341926	IN	NS	e.root-servers.net.
.			341926	IN	NS	c.root-servers.net.
.			341926	IN	NS	h.root-servers.net.
.			341926	IN	NS	g.root-servers.net.
.			341926	IN	NS	l.root-servers.net.
.			341926	IN	NS	k.root-servers.net.
.			341926	IN	NS	i.root-servers.net.
.			341926	IN	NS	j.root-servers.net.
.			341926	IN	NS	a.root-servers.net.
.			341926	IN	NS	d.root-servers.net.
.			341926	IN	NS	b.root-servers.net.
.			341926	IN	NS	f.root-servers.net.
.			341926	IN	NS	m.root-servers.net.
;; Received 811 bytes from 127.0.0.53#53(127.0.0.53) in 25 ms

org.			172800	IN	NS	a0.org.afilias-nst.info.
org.			172800	IN	NS	a2.org.afilias-nst.info.
org.			172800	IN	NS	b0.org.afilias-nst.org.
org.			172800	IN	NS	b2.org.afilias-nst.org.
org.			172800	IN	NS	c0.org.afilias-nst.info.
org.			172800	IN	NS	d0.org.afilias-nst.org.
org.			86400	IN	DS	26974 8 2 4FEDE294C53F438A158C41D39489CD78A86BEB0D8A0AEAFF14745C0D 16E1DE32
org.			86400	IN	RRSIG	DS 8 1 86400 20260515170000 20260502160000 54393 . I0tOHkPp/De7GuEquTP4yzmMtGS1ULvZVuyXbZX2pwfw+7isEBNo3vIJ Y1ZcdkJSGADYBs7RYiEjpBuk+YOQbiCKHWXVv9gPNIiieha3XyuuIGVm aquqo1Jmc3V24CX7C57+5pd5Ueh77+MRTPfJiM1yAvV9g9iiA6TSrLHO fxXkIo1b8InoiC7W3LyLInCptBe37hFjhb+tGHm9H5Hk3656uQDLohAU LAQqPdc0XaTGqryg8aa1Ol6RlyFmAJhRryKG0yT6Unqrx+g2se4kq2VL 8Dvq/pcEv/QgXZNKKQXC2GL03X7nxEN5zWDOoReJLDUN38y0vYWotNAj VELo/Q==
;; Received 779 bytes from 192.5.5.241#53(f.root-servers.net) in 21 ms

;; UDP setup with 2001:500:40::1#53(2001:500:40::1) for csperkins.org failed: network unreachable.
;; no servers could be reached
;; UDP setup with 2001:500:40::1#53(2001:500:40::1) for csperkins.org failed: network unreachable.
;; no servers could be reached
;; UDP setup with 2001:500:40::1#53(2001:500:40::1) for csperkins.org failed: network unreachable.
csperkins.org.		3600	IN	NS	ns2.mythic-beasts.com.
csperkins.org.		3600	IN	NS	ns1.mythic-beasts.com.
csperkins.org.		3600	IN	DS	14422 10 2 5055950BC6D4F7309EC1FF46F71E0153293022ACFAA495513C17DA2F 237E84D6
csperkins.org.		3600	IN	DS	45433 10 2 E3C75AADE492D95315252E6D3233F46A373E34591CE8481BBAEF0DF6 1863D287
csperkins.org.		3600	IN	RRSIG	DS 8 2 3600 20260523001236 20260501231236 39684 org. vo5FeFcqQ7saCk71o3k7p3PZzAZDax2EEaJT3FwIkAk2GVHKmKUQFkR3 f2EhVgSThi24SKo9LONYqZ2joV91QdCnLk26B/MmE+gee3VaFvOKeMVx vW6HMwy5G9Ia8HVEdAxTD26cTtlnc8Mwe1g9kKtC9PVOYVtFA3oTZat1 BkI=
;; Received 354 bytes from 199.19.53.1#53(c0.org.afilias-nst.info) in 307 ms

csperkins.org.		3600	IN	A	93.93.131.127
csperkins.org.		3600	IN	RRSIG	A 10 2 3600 20260531142828 20260501133054 13820 csperkins.org. YNQRK5dPRe/r62VcOzATTpOQqzZIaNJuBemiq8U4MJ9f2LWXYjYAZxRt jpnL5b8/UvIa68t97F6mXsnixqjEfQWolRHDKYIUDf8hR8bk4GXKRKyg aIQV9yYdLDjj+YG9FAV48iYY1lUimHSZ1k9N4Nb7uTt7E2kLA+Lqtqbj aFtViWqOEfvX0QBOolY9z7+O+IbToLoteNV2jlmikDLaBezA3v1z9TrD HjsWMvg7tt3T+6sM92pd9erYAhxfHZUlybElHn9odwJguavo1+FQQWqo SSaZ8HTFxjUeEsqYhQFUPz5MK79uXDvM1GmZb7il/QyJ7sJngVxG3V7e 5Y+/NA==
;; Received 387 bytes from 45.33.127.156#53(ns1.mythic-beasts.com) in 138 ms

james@james-laptop:~$ 
```

## DNS over TLS
DNS over TLS solves the insecurity problem. 
1. The DNS client opens a connection to the resolver on port 853
2. The DNS client and the resolver negotiate a TLS 1.3 session on the TCP connection
3. The DNS client sends a query and receives a response over the TLS connection

DNS over TLS messages are formatted in the exact same way as DNS over UDP, and contain the exact same information. The only difference is that they are sent over TLS and not UDP.
This is slower and has higher overhead than DNS over UDP, due to the need to negotiate TCP and TLS, but it is more secure. You can reduce the overhead by keeping the connection open.
## DNS over QUIC
DNS over QUIC was only recently standardized in 2024
It has the same principle as DNS over TLS: The client opens a QUIc connection to the resolver
	Negotiates the TLS security as part of the connection setup
Sends the query and receives the response over that connection
	Queries and response contain exactly the same data as DNS over UDP, just sent via QUIC
## DNS over HTTPS
DoH allows a client to send queries to a resolver using HTTPs.
You can use either GET or POST methods in HTTPS.
The HTTP response contains a content type: application/dns-message and contains the exact same data that would be sent in a UDP-based DNS response.
```
GET /dns-query?dns=AAABAAABAAAAAAAAA3d3dwdleGFtcGxlA2NvbQAAAQAB HTTP/1.1
Accept: application/dns-message

POST /dns-query HTTP/1.1
Accept: application/dns-message
Content-type = application/dns-message
Content-length = 33

<33 bytes of UDP query, exactly as if sent in a UDP packet>
```

# Implications of the choice of DNS resolver

## How is the DNS resolver chosen?
when you connect to a network the host uses **DHCP (dynamic host configuration protocol)** to discover network settings and configuration to use.
It is possible to configure the DNS resolver manually.
If a host has multiple network interfaces it may use a different DNS resolver for each. For example if somebody is connected to 5G and company ethernet, the ethernet connection may have access to names of internal services that aren't accessible to the public.
## Implications of DNS over HTTPS
### Can networks restrict the choice of DNS resolver?
Firewalls can block access to DNS over UDP and DNS over TLS resolvers by blocking access to UDP port 53 and TLS port 853 for all destination ip addresses apart from allowed DNS resolvers.

**Difficulty of blocking DNS over HTTPS**
Networks can't distinguish DoH traffic from other HTTPS traffic.
The only way they would be able to tell is from the destination IP address of the packets.
If a web server handles a mix of web and DNS traffic over HTTPS, it is not possible to block one without blocking the other.
Many ISPs and governments are concerned that DoH prevents the use of DNS as a control point

# Intellectual property and the DNS
Intellectual property is managed on a national basis. For example a company might own a trademark in the UK that is owned by a different company in the Republic of Ireland.
Which of these companies owns trademark.ie and trademark.co.uk are a straightforward question.
Which of those companies owns trademark.com is harder to decide (especially since .com is operated by a US based organization). 

**A ccTLD  clearly operates under a legal regime of a particular country. Use of a gTLD might lead to legal complications**

ICANN allows interested parties to apply for new gTLDs (provided they pay ~$200,000)
What happens if two parties want the same gTLD?
	ICANN has a process to decide who gets the gTLD:
	1. Negotiation - applicants are encouraged to sort out the contention among themselves
	2. Community priority evaluation - If an applicant has a formal community-based proposal and represents a specific identified group of people or organizations they may be given priority over commercial applications
	3. Auction - If all previous steps fail then the gTLD will be given to the highest bidder

# What Domains should exist?
**Should a particular gTLD be allowed to exist?**
For example, should .xxx be allowed to exist to host adult content? It is well known that criminals use these websites. 
If a particular country/group finds a site objectionable should it be taken down? For example, if a website is hosting content that is illegal in country X in country Y, but is accessible from country X, should the website be taken down?

**Should a particular subdomain be allowed to exist?**
A ccTLD can enforce local conventions and rules.

# Who controls the DNS root servers?
The DNS root servers are mostly managed by US-based organizations.
Is this a risk for other countries?
Should the root servers be controlled by a broader mix of countries?
	If so, who gets to decide? ICANN? The UN?
	Is there a benefit from controlling a DNS root server?
	Is there a benefit from controlling a gTLD server? Who gets to host .com for example?
# Should there be a single DNS root?
Should all TLDs be accessible from everywhere?
Should there be a single global DNS?
Should the same name always resolve to the same site?
	With global distribution networks how can you tell?
Should different countries be allowed to filter DNS?
	If so how should such restrictions be implemented?
	It is difficult to distinguish modifications to DNS responses made to conform to government-mandated filtering requirements from those made by malware, phishing attacks, etc. - is this a feature or a bug?