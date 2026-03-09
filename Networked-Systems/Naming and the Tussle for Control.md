
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


## Methods of DNS Resolution
There are 5 main ways of making DNS queries:
- DNS over UDP
- DNS over TLS
- DNS over QUIC
- DNS over TCP
- DNS over HTTPS


# The Politics of Names
## DNS choise Resolver
when you connect to a network the host uses DHCP (dynamic host configuration protocol) to discover network settings and configuration to use. It is possible to configure the DNS resolver manually.

## Who controls Root Servers
The US controls most DNS root servers. Some argue that this is a data risk for other countries.