---
tags:
  - work
  - ccna
---
- ## Industry Standards:
	- ### Spanning Tree Protocol (802.1D)
		- The original STP
		- All VLANs share one STP instance, therefore, cannot load balance
	- ### Rapid Spanning Tree Protocol (802.1w)
		- Much faster at converging/adapting to network changes than 802.1D
		- All VLANs share one STP instance
	- ### Multiple Spanning Tree Protocol (802.1s)
		- Uses modified RSTP mechanics
		- Can group multiple VLANs into different instances to perform load balancing
			- i.e. VLANs 1-5 in instance 1, VLANs 6-10 in instance 2
- ## Cisco Versions:
	- ### Per-VLAN Spanning Tree Plus (PVST+)
		- Cisco's upgrade to 802.1D
		- Each VLAN has its own STP instance
		- Can load balance by blocking different ports in each VLAN
	- ### Rapid Per-VLAN Spanning Tree Plus (Rapid PVST+)
		- Cisco's upgrade to 802.1w
		- Each VLAN has its own STP instance
		- Can load balance by blocking different ports in each VLAN

- ## RSTP - Rapid Spanning Tree Protocol:
	- *RSTP is not a timer-based spanning tree algorithm like 802.1D. RSTP offers an improvement over the 30 seconds or more that 802.1D takes to move a link to forwarding. Bridge-bridge handshakes allows ports to move directly to forwarding.*
	- RSTP serves the same purpose as STP, blocking specific ports to prevent Layer 2 loops
	- RSTP elects a root bridge, root ports, and designated ports with the same rules as STP
	- Port Roles:
		- ### Root Port:
			- Unchanged from STP
			- The port that is closest to the root bridge becomes the root port for the switch
			- The root bridge is the only switch that doesn't have a root port
		- ### Designated Port:
			- Unchanged from STP
			- The port on a segment (collision domain) that sends the best BPDU is that segment's designated port (only one per segment)
		- Non-Designated Port is split into two separate roles:
			- ### Alternate Port:
				- A discarding port that receives a superior BPDU from another switch
				- The same as blocking ports in STP
				- Functions as a backup to the root port
				- If the root port fails, the which can immediately move its best alternate port to forwarding
			- ### Backup Port:
				- A discarding port that receives a superior BPDU from <u>another interface on the same switch</u>
				- This only happens when two interfaces are connected to the same collision domain (via a hub)
				- Function as a backup for a designated port
	- Rapid STP is compatible with Classic STP. The interface(s) on the Rapid STP-enabled switch will operate in Classic STP mode (timers, blocking → listening → learning → forwarding process, etc.)
	- In classic STP, only the root bridge originated BPDUs, and other switches just forwarded the BPDUs they received
	- In rapid STP, ALL switches originate and send their own BPDUs from their designated ports (every hello time, 2 seconds)
	- Switches 'age' the BPDU information much more quickly
		- In classic STP, a switch waits 10 hello intervals (20 seconds)
		- In rapid STP, a switch considers a neighbor lost if it misses 3 BPDUs (6 seconds), it will then flush all MAC addresses learned on that interface
	- RSTP distinguishes between three different 'link types':
		- ### Edge:
			- A port that is connected to an end host
			- Moves directly to forwarding, without negotiation
			- They function like a classic STP port with PortFast enabled
			- To configure an edge port, configure the port with portfast
		- ### Point-to-point:
			- A direct connection between two switches
			- They function in full-duplex
			- You don't need to configure the interface as point-to-point, it should be auto-detected
		- ### Shared:
			- A connection to a hub
			- Must operate in half-duplex mode to avoid collisions
			- You don't need to configure the interface as shared, it should be auto-detected
	
| Speed    | STP Cost | RSTP Cost |
| -------- | -------- | --------- |
| **10 Mbps**  | 100      | 2,000,000 |
| **100 Mbps** | 19       | 200,000   |
| **1 Gbps**   | 4        | 20,000    |
| **10 Gbps**  | 2        | 2000      |
| **100 Gbps** | x        | 200       |
| **1 Tbps**   | x        | 20        |
### Rapid Spanning Tree Port States

| STP Port State | Send/Receive BPDUs | Frame Forwarding (Regular Traffic) | MAC Address Learning | Stable/Transitional |
| -------------- | ------------------ | ---------------------------------- | -------------------- | ------------------- |
| **Discarding**     | NO/YES             | NO                                 | NO                   | Stable              |
| **Learning**       | YES/YES            | NO                                 | YES                  | Transitional        |
| **Forwarding**     | YES/YES            | YES                                | YES                  | Stable              |
- If a port is administratively disabled = discarding state
- If a port is enabled but blocking traffic to prevent Layer 2 loops = discarding state