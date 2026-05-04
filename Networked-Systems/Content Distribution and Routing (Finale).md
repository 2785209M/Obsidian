# Content Distribution Networks (CDNs)
content distribution refers to the action of spreading data around multiple web caches in different locations, such as edge networks and data centers. This spread out approach reduces the load on the main servers, reduces latency, and reduces the chances of a successful DOS attack. 

CDNs are massive and extremely expensive.

## Latency
One of the main benefits of using a CDN is decreasing latency. This is achieved by caching location from distant servers in servers more close by for faster lookup.

This method requires global distribution in the CDN.

## Locating CDNs
Nearby CDNs are located using DNS.
**Every Resource** hosted by the CDN is given a unique DNS name.
DNS entries in CDN all contain address records which link directly to the CDN edge server.

The closest CDN node can be located using anycast routing. 
Each resource hosted by the CDN has a unique filename but common hostname.
*Anycast Routing is a method of networking where a common IP address is shared between multiple globally dispersed servers. Border Gateway control is used to find the nearest/best performing server.* 

# Inter-Domain Routing
The internet is a network of networks. Every network is an **Autonomous System**.
**Inter-Domain routing** is the process of finding the best path from the source network to the destination network.

## Routing Policy
Inter-domain routing goes between competing companies. To prevent these companies from interfering with each other, routing must use certain policies.

There are policy restrictions on who can determine network topology.

There are policy restrictions on which route traffic should follow between a particular source and destination

Policy restrictions might prioritize non-shortest path routes.

## Autonomous Systems
An autonomous system is a network that is operated independently.

Each AS has an AS number and one or more IP address prefixes.

Each node in an AS diagram is an autonomous system. Each edge in the diagram is a link between autonomous systems.

## Routing at the edge
Devices on the edge networks tend to contain simple routing tables.
Everything other than IPs on the local network is routed on a default path to the internet.

## Routing at the core
Unlike edge networks, core networks need full routing tables. Each network has multiple paths and information is needed to choose the most appropriate.

As the number of interconnections each AS has increases, companies have started using **Internet Exchange Points**. 

# Border Gateway Control
External BGPs are used to exchange routing information between ASes. They exchange information about AS network topology. They run over TCP connections between two routers. They are used to calculate appropriate inter domain routes to between ASes.

eBGP routers advertise lists of IP address prefixes and associated AS-level paths.

Internal BGPs propagate information from external BGPs within Autonomous Systems.

## Routing Policy in BGP
Autonomous systems can choose what information they broadcast to their neighbors. It is typical that they will broadcast certain information to their customers but not to their peers.
These systems are often designed to maximize profit.

## BGP Routing Decision Process
Each AS exchanges routing information with its neighbors
Each AS applies policy to choose what routes to use
Each AS applies policies to choose what routes to advertise to neighbors

eBGPs exchange routes between neighbors.


# Internet routing is insecure.
Any AS participating in BGP can announce any address prefix, whether or not they own it. This can result in traffic being misdirected. This is called **BGP Hijacking**.

## Resource Public Key Infrastructure (RPKI)
RPKI is an attempt to secure internet routing. It allows an AS to make a signed **Route Origin Authorisation (ROA)**, which is a digital signature, announces via BGP, proving the AS is authorized to announce an IP address prefix.


# Intra-Domain Routing

## Intra-Domain unicast routing
BGP gives information on the path to reach other networks.
There is a single trust domain with no policy restrictions on who can determine network topology and no policy restrictions on which links can be used.
It uses desire efficient routing, which means shortest path best path.

There are two approaches: using link state or using distance vector.

## Distance vector Routing
Nodes contain a vector which shows the distance to every other node in the network.
Packets will be forwarded on the shortest path to the destination.
This is not widely used in practice because it is slow and bad on failure in networks containing loops.

## Link State Routing
Nodes contain Link State Information.
This information is flooded through the network on startup.
Each node then directly calculates shortest path to every other node

Flooding link state data through all nodes ensures all nodes know the entire network topology. This means every node can calculate the shortest path and the shortest path is assumed to be optimal.

# Discuss how the effects of a total DNS Failure would manifest themselves and how quickly they would become visible
DNS data is widely stored in Caches in locations like CDNS. DNS names have a specific Time To Live (TTL) in these caches, and much DNS traffic is routed through them, so not all DNS lookups would immediately fail. You would, immediately after the failure, not be able to lookup new names that were not stored in the cache, but would still be able to lookup names that were in the cache. As the TTL of each DNS name is used up in the CDNs you would experience an increase in the DNS failure rate over time, until eventually everything stops working.

# Discuss why DNS used to use UDP but switched to using different transport protocols
UDP was a good choice on a smaller internet because it was fast, required little overhead, and each DNS request and response could fit into a single UDP packet. TCP doesn't offer a benefit in reliability compared to just resending a request if no response is received, and there is not enough network congestion for congestion control to work.
DNS is moving to alternative transport protocols because:
With the deployment of DNS security and authenticated DNS responses, DNS request are now too large to fit within a single UDP packet.
It is desireable in a larger internet to encrypt and authenticate DNS requests and responses, which is easier to do using a transport protocol that supports these features (such as TCP, TLS, QUIC).