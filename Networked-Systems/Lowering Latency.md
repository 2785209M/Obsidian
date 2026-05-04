# TCP Congestion Control
## Congestion Control Principles
### **Key Congestion Principles:**
#### Packet Loss is a congestion signal
Data flows from sender to receiver through a series of **IP routers**
Routers perfrom two functions:
- **Routing**: Receive packets, determine appropriate route to destination
- **Forwarding**: enqueue packets on outgoing link for delivery
	- Queues shrink if packets forwarded faster than they arrive
	- Queues grow if packets arrive faster than they can be forwarded
	- Queues have maximum size - > **packets are dropped if the queue is full**
	- Congestion control can use this packet loss as a congestion signal
#### Conservation of packets in flight
Acknowledgements are returned by the receiver
The receiver sends one packet per acknowledgement
- The total number of packets in transit is constant
- The system is in equilibrium; queues neither increase nor decrease in size
- **ACK Clocking** - each acknowledgement "clocks out" in next packet
- Automatically reduces sending rate if network is congested and delivers packet slowly
#### Additive Increase/multiplicative decrease
The sender follows the additive increase multiplicative decrease algorithm
- start slowly and increase gradually to find an equilibrium
	- Add a small amount to the sending speed each time
	- For a window-based algorithm $w_i =  w_{i-1} + \alpha$ for each RTT, where $\alpha = 1$ typically

Responds to congestion rapidly
- Multiplying sending window by some factor $\beta < 1$ each interval loss seen
- For a window-based algorithm $w_i = w_i * \beta$, where $\beta = \frac{1}{2}$ typically

Faster reduction than increase -> stability; **TCP Reno**
## Loss-based congestion control
### TCP Reno
#### Congestion Control in TCP Reno
TCP maintains a sliding window onto the available data that determines how much can be sent according to the AIMD algorithm, plus slow start and congestion avoidance, which gives approximately equal bandwidth to each flow sharing a link

**TCP Reno** is the foundational classic of TCP congestion control.
#### Sliding Window Protocols
Stop-and-wati protocols perform poorly.
- It takes time, $t_s$ to push a single segment onto the wire: $t_s = (packet size)/(link bandwidth)$
-  $t_{RTT}$ is the time it takes for a segment to travel to the receiver and for the ACK to return
- Link utilization, $U = t_s/t_{RTT}$
	- We want only a fraction of the time link to be spent sending packets - we want $U \approx 1.0$. This means that the transmission time effectively covers the entire round trip delay so the sender isn't left waiting.
	- If U > 1 the segments begin to fill up the router's queue, which leads to increased delay and eventually packet loss, which triggers TCP Reno's **fast retransmit** and **fast recovery** algorithms

**Sliding Window Protocols** improve on stop-and-wait by sending several packets before waiting for an acknowledgement
##### Sliding Window Protocols improve utilization
Sliding Window protocols improve link utilization using a **congestion window** - the number of packets to be sent before an ACK arrives
- **Note:** since in modern high-speed networks $t_s$ is often microseconds whilst $t_{RTT}$ is often milliseconds, Reno must maintain a very large congestion window to ensure the effective transmission time of the entire window stays in sync with the $t_{RTT}$.

**TCP Reno** is a sliding window protocol. It is built to optimise without having the information to know the correct window size. 
##### Choosing the Initial Window
Without information you have to start with a small window and increase gradually. 
$w_{init} = 1$ packet per round trip time -> This is safe. Start at the lowest possible rate and increase.
$w_{init} = 3$ packets per RTT -> Traditional approach
$w_{init} = 10$ packets per RTT -> Used in modern Reno implementations since RTT is so much larger than the sending time. $w_{init}$ is expected to gradually increase as network performance improves.
##### Finding the Path Capacity
The initial window allows you to send **something**. It is unlikely to be the optimal window size.
To choose the correct window size you can use TCP slow start to rapidly find the correct window size for the path.
Congestion avoidance adapts to the changes in channel capacity once the connection is running.
#### TCP Reno Slow Start
Start with $w_{init}$. For this example let's set it to one although it could be different. This is different to AIMD, as the increase is multiplicative. Double the congestion window each RTT. Do this until packets are lost. Once packets are lost, half the congestion window to its previous safe value, and **exit slow start**. Packets will now be sent at the optimal rate.

1. Start sending slowly
2. Exponentially increase sending rate
3. Packet Loss Occurs -> Reset to last known good rate
4. Exit Slow Start
#### TCP Reno Congestion Avoidance
After the first packet is loss and the sending rate is reduced, the congestion window is now at approximately the right size for the path. the protocol switches to **congestion avoidance**.
The goal is now to adapt to changes in network capacity.
- Perhaps the capacity changes - for example radio signal strength on a mobile device
- Perhaps other traffic changes - for example competing flows stop or additional cross-traffic starts
Now we use additive increase, multiplicative decrease of the congestion window.

If a complete window of packets is sent without loss: increase the linear congestion window by $\alpha$ (usually 1) packet per RTT then send the next window of packets. Slow, linear additive increase $w_{i} = w_{i-1} +\alpha$.

If a packet is lost and detected via a triple duplicate acknowledgement: the window size will multiply by $\beta$ (usually half). Rapid reduction allows for congestion to clear quickly and avoid congestion collapse.

If a packet is lost via a timeout, $w_{i}$ is reset to $w_{init}$ and slow start begins again. timeout is considered as severe congestion by the protocol. Retransmission timeout (RTO) = Smoothed Round Trip Time (SRTT) + (4 * Round Trip Time Variance (RTTVAR)). SRTT is the expected arrival time, and 4xRTTVAR is the safety buffer. **RTO=SRTT+4×RTTVAR**

#### Congestion Window, Buffering, and Throughput
Bottleneck Buffer Size = Bandwidth * delay
The bottleneck queue is never empty
The bottleneck never become idle -> sending rate varies, but receiver sees continuous flow.
The congestion window sometimes follows a sawtooth pattern, but received rate is constant at approximately bottleneck bandwidth.

TCP Reno is effective at keeping the bottleneck link fully utilized.
- It trades some extra delay to maintain throughput
- Provided sufficient buffering in the network: Buffer size = bandwidth * delay
- Packets queued in Buffer -> delay

Limitations:
- Assumes packet Loss is due to congestion; non-congestive loss, e.g., due to wireless interference, impacts throughput.
- Congestion avoidance phase takes a long time to use increased capacity
### TCP Cubic
TCP Reno can perform poorly in fast long distance networks. With a 10Gbps bandwidth and a 100ms RTT window you could have the congestion window set to ~100,000 packets.
In congestion avoidance, one packet lost and detected via triple duplicate ACK halves the window and then increases by 1 per RTT. This would mean it would take 50,000 RTTs to recover the maximum sending rate (~1.4 hours with 100ms RTT).

TCP Cubic changes the congestion control algorithm to increase the congestion window faster than TCP Reno. Rather than using addition to increase the window size it uses a special algorithm to quickly reach the last stable window size, and then slows down.

If Packet Loss Occurs during congestion avoidance, TCP Cubic reduces the congestion window: $W_{i} = W_{i-1}*0.7$ (reduces less than TCP Reno).

After Packet Loss during Congestion Avoidance, TCP Cubic increases the congestion window as: $W_{cubic} = C(t-K)^3 + W_{max}$
- $W_{max}$ is the maximum window size reached before the loss
- t is the time taken since packet loss
- K is the time it will take to increase the window back to $W_{max}$ assuming no further packet loss
- C=0.4 is a constant that controls fairness to TCP Reno
Many additional details are included to ensure fairness with TCP Reno on slower, shorter RTT networks.
#### TCP Cubic vs Reno
TCP Cubic is default in most modern operating systems
- It is more complex than TCP Reno, but improves performance for networks with longer RTT and higher bandwidth
Both algorithms use packet loss as a congestion signal and eventually fill router buffers
## Delay-based Congestion Control
TCP Cubic and TCP Reno **both** aim to fill the network bandwidth. No matter how big the queue, Reno and Cubic **will** cause it to overflow if they have enough data to send. Having a long queue means packets have to wait longer, which increases the network latency. Router bufferbloat can also cause other packets, such as UDP packets, to be lost forever.
### TCP Vegas
Key insight: If sending faster than the network can deliver, packets will be queued
- TCP Reno and Cubic both wait until the queue overflows and packets are lost before slowing down
- **TCP Vegas** watches for the increase in delay as the queue starts to fill up, which allows it to slow down before the queue overflows.
	- Queues are smaller, lower latency
	- Packets are not lost so don't need retransmission
- This only affects congestion avoidance. Slow starts are unchanged.
#### Congestion Control in TCP Vegas
- **BaseRTT** -> the base RTT is the time between sending a packet and getting its acknowledgement when the network is not congested.
- **ExpectedRate** -> Calculated by dividing the congestion window by the baseRTT. $ExpectedRate = \frac{W}{baseRTT}$. If the network can support this sending rate the complete window should be delivered within the RTT.
- **ActualRate** -> Bytes sent divided by the actual RTT for each packet. Measures how fast packets are actually received based on acknowledgements returned. Actualrate <= ExpectedRate since packets can't be delivered faster than they are sent.

TCP Vegas takes the actual rate and the expected rate, compares them, and adjusts the congestion window accordingly.
- If ExpectedRate - ActualRate < R1, then additive increase to window
	- If data is arriving at close to the expected rate, then you can likely send faster
- If ExpectedRate - ActualRate > R2 then additive decrease to window
	- If data is arriving slower than it is being sent, slow down
#### Limitations of TCP Vegas
Delay-based congestion control is a good idea in principle as it reduces latency and reduces packet loss, but loss and delay-based congestion don't cooperate.
TCP Reno and Cubic aggressively increase queue sizes. This increases the RTT and reduces the ActualRate as measured by TCP Vegas. This forces TCp Vegas to slow down. This cycle repeats until the TCP Vegas sending rate drops to **zero**.
**TCP Vegas is not used as it cannot be deployed alongside TCP Reno or TCP Cubic**
### TCP BBR
TCP BBR is an attempt at making a congestion control algorithm for TCP that can be deployed alongside TCP Reno and Cubic.
It is a proposal from Google to measure the RTT and bandwidth of the bottleneck link, and directly sets the bottleneck window.
It is used by Youtube in some cases, but is highly experimental.
## Explicit congestion notification
TCP has inferred congestion through measurement.
- TCP Reno and Cubic lose packets when they cause router queue overflow
	- Problematic because it increases delay
	- Problematic because of non-congestive loss on wireless links
- TCP Vegas -> increase delay as queues start to fill
	- Conceptually a very good idea, but difficult to deploy when competing with TCP Reno and Cubic
**Why not have the network tell TCP that congestion is occuring?**
- An Explicit Congestion Notification field in the IP header
- Tell TCP to slow down if it's overloading the network

ECN extensions also exist in QUIC and RTP.
### ECN and IP
The sender sets the ECN bits on transmission
- 00 = doesn't support ECN, 01 = ECN capable
If congestion occurs, the **congested router** (<- Important) sets the ECN bits to 11
- This indicates that a queue of packets for some link is getting full but has not yet overflowed - The signal of congestion set by the network
- If the queue does overflow, packets are still dropped
This requires cooperation between the Network and EndPoints
### ECN and TCP
If the IP segment arrives with the ECN congestion experienced (ECN-CE) bit set to 1:
- the ECN Echo field in the TCP packet header allows the receiver to report this to the sender. 
- The next packet the sender receives will have the ECE bit set to 1, and will be notified that congestion occurred.
- The sender will then reduce the congestion window **as if a packet has been lost** and set the CWR (Congestion Window Reduced) bit to 1 in the TCP header to indicate that it has done so.
## Light Speed?
Two factors impact latency for data transfer:
- The time packets spend in queues within the network
	- Impact of TCP congestion control algorithm
	- ECN to signal congestion before queues overflow
- The time packets spend propagating down the links between routers
	- Speed at which light propagates through an optical fibre
	- Speed at which electrical signals propagate down a wire
	- Speed at which radio signals propagate through air
### Reducing propagation delay
Physically shorter links have lower propagation delay:
- This offers a significant reduction in latency by laying new cables that follow great circle routes between popular destinations
- For example the ocean floor cable that links Japan to Europe via the North pole

Signals propagate at the speed of light in a **transmission medium**
- The speed of light in an optical fibre is ~200,000km/s
- The speed of light in vacuum is 299,792km/s
Starlink is deploying thousands of low earth orbit satellites.
- With careful routing, satellite path can folllow close to the most direct great cicle route and a chieve 47% reduction in latency since signals sent through vacuum rather than optical fibre.

But - satellites disrupt astronomy and have short lifetimes, and cause pollution on re-entry
