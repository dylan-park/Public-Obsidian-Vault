---
tags:
  - work
  - ccna
---
- ## Classification
	- The purpose of QoS is to give certain kinds of network traffic priority over others during congestion
	- **Classification** organizes network traffic (packets) into traffic classes (categories)
	- Classification is fundamental to QoS, to give priority to certain types of traffic you have to identify which types of traffic to give priority to
	- Some examples of classifying traffic:
		- An ACL - Traffic which is permitted by the ACL will be given certain treatment, other traffic will not
		- **NBAR** (Network Based Application Recognition) - performs a *deep packet inspection*, looking beyond the Layer 3 and Layer 4 information up to Layer 7 to identify the specific kind of traffic
		- In the Layer 2 and Layer 3 headers there are specific fields used for this purpose
	- The **PCP** (Priority Code Point) field of the 802.1Q tag (in the Ethernet header) can be used to identify high/low priority traffic
		- Only when there is a dot1q tag
	- The **DSCP** (Differentiated Services Code Point) field of the IP header can also be used to identify high/low priority traffic
- ## PCP/CoS
	- ![[Pasted image 20240422164200.png]]
	- PCP is also known as Cos (Class of Service), its use is defined by IEEE 802.1p
	- 3 bits = 8 possible values (2^3 = 8)
	- 'Best Effort' delivery means there is no guarantee that data is delivered, or that it meets and QoS standard, This is regular traffic, not high priority
	- IP phones **mark** call signaling traffic (used to establish calls) as PCP3, they **mark** the actual voice traffic as PCP5
	- Because PCP is found in the dot1q header, it can only be used over the following connections:
		- Trunk links
		- Access link with a voice VLAN

**PCP Traffic Types**

| PCP Value | Traffic Types         |
| --------- | --------------------- |
| 0         | Best Effort (Default) |
| 1         | Background            |
| 2         | Excellent Effort      |
| 3         | Critical Applications |
| 4         | Video                 |
| 5         | Voice                 |
| 6         | Internetwork Control  |
| 7         | Network Control       |
- ## IP ToS Byte
	- ![[Pasted image 20240422165844.png]]
	- Old ToS Byte = 3 bits, 8 values (0-7)
	- Current ToS Byte = 6 bits, 64 values (0-63)
	- ### IP Precedence
		- IPP = 3 bits, 8 values (0-7)
		- Standard IPP markings are similar to PCP:
			- 6 and 7 are reserved for 'network control traffic' (i.e. OSPF messages between routers)
			- 5 = Voice
			- 4 = Video
			- 3 = Voice Signaling
			- 0 = Best Effort
		- With 6 and 7 reserved, 6 possible values remain
		- Although 6 values are sufficient for many networks, the QoS requirements of some networks demand more flexibility
	- ### DSCP
		- DSCP = 6 bits, 64 values (0-63)
		- RFC 2474 (1998) defines the DSCP field, and other 'DiffServ' RFCs elaborate on its use
		- With IPP updated to DSCP, new Standard Markings had to be decided upon
			- By having generally agreed upon standard markings for different kinds of traffic, QoS design & implementation is simplified, QoS works better between ISPs and enterprises, among other benefits
		- #### DF/EF
			- Default Forwarding (**DF**)
				- Best Effort Traffic
				- The DSCP marking for DF is 0
			- Expedited Forwarding (**EF**)
				- Low Loss/Latency/Jitter Traffic (Usually voice)
				- The DSCP marking for EF is 46
		- #### AF
			- **AF** (Assured Forwarding) defines four traffic classes, all packets in a class have the same priority
			- Within each class, there are three levels of *drop precedence*
				- Higher drop precedence = more likely to drop the packet during congestion
				- ![[Pasted image 20240422172933.png]]
	- ### CS
		- **CS** (Class Selector) defines eight DSCP values for backward compatibility with IPP
		- The three bits that were added for DSCP are set to 0, and the original IPP bits are used to make 8 values
		- ![[Pasted image 20240422173251.png]]
	- ### RFC 4954
		- RFC 4954 was developed with the help of Cisco to bring all of these vales together and standardize their use
		- The RFC offers many specific recommendations, like:
			- Voice Traffic: **EF**
			- Interactive Video: **AF4x**
			- Streaming Video: **AF3x**
			- High Priority Data: **AF2x**
			- Best Effort: **DF**
	- ### Trust Boundaries
		- The *trust boundary* of a network defines where devices trust/don't trust  the QoS markings of received messages
		- If the markings are trusted, the device will forward the message without changing the markings
		- If the markings aren't trusted, the device will change the markings according to the configured policy
		- If an IP phone is connected to the switch port, it is recommended to move the trust boundary to the IP phones
			- This is done via configuration on the switch port connected to the IP phone
			- If a user marks their PC's traffic with a high priority, the marking will be changed (not trusted)
- ## Queuing/Congestion Management
	- When a network device receives traffic at a faster rate than it can forward the traffic out of the appropriate interface, packets are placed in that interface's queue as they wait to be forwarded
	- When the queue becomes full, packets that don't fit in the queue are dropped (tail drop)
	- RED and WRED drop packets early to avoid trail drop
	- An essential part of QoS is the use of multiple queues
		- This is where classification plays a role
		- The device can match traffic based on various factors (for example the DSCP marking in the IP header) and then place it in the appropriate queue
	- However, the device is only able to forward one frame out of an interface at once, so a *scheduler* is used to device which queue traffic is forward from next
		- *Prioritization* allows the scheduler to give certain queues more priority than others
	- ![[Pasted image 20240422174151.png]]
	- A common scheduling method is *weighted round-robin*
		- **Round-Robin** = Packets are taken from each queue order, cyclically
		- **Weighted** = More data is taken from high priority queues each time the scheduler reaches that queue
	- **CBWFQ** (Class-Based Weighted Fair Queuing) is a popular method of scheduling, using a weighted round-robin scheduler while guaranteeing each queue a certain percentage of the interface's bandwidth during congestion
	- Round-robin scheduling is not ideal for voice/video traffic. Even if the voice/video traffic receives a guaranteed minimum amount of bandwidth, round-robin can add delay and jitter because even the high priority queues have to wait their turn in the scheduler
	- **LQQ** (Low Latency Queuing) designates one (or more) queues as strict priority queues
		- This means that if there is traffic in the queue, the scheduler will <u>always</u> take the next packet from that queue until it is empty
	- This is very effective for reducing the delay and jitter of voice/video traffic
	- However, it has the downside of potentially starving other queues if there is always traffic in the designated strict priority queue
		- *Policing* can control the amount of traffic allowed in the strict priority queue, so that it can't take all the link's bandwidth
- ## Shaping and Policing
	- Traffic **shaping** and **policing** are both used to control the rate of traffic
	- **Shaping** buffers traffic in a queue if the traffic rate goes over the configured rate
	- **Policing** drops traffic if the traffic rate goes over the configured rate
		- 'Burst' traffic over the configured rate is allowed for a short period of time
		- This accommodates data applications which typically are 'bursty' in nature, instead of a constant stream of data, they send data in bursts
		- The amount of burst traffic allowed is configurable
	- In both cases, classification can be used to allow for different rates for different kinds of traffic