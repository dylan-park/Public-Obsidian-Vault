---
tags:
  - work
  - ccna
---
- Redundancy is an essential part of network design
- Modern networks are expected to run 24/7/365. Even a short downtime can be disastrous for a business
- If one network component fails, you must ensure that other components will take over with little or no downtime
- As much as possible, you must implement redundancy at every possible point in the network
- Broadcast Storm - An infinite loop of broadcast frames in a network
- MAC Address Flapping - When frames with the same source MAC address repeatedly arrive on different interfaces, the switch is continuously updating the interface in its MAC address table
- ## Classic Spanning Tree Protocol:
	- IEEE802.1D
	- Switches from ALL vendors run STP by default
	- STP prevents Layer 2 loops by placing redundant ports in a blocking state, essentially disabling the interface
	- These interfaces act as backups that can enter a forwarding state if an active (=currently forwarding) interface fails
		- Interfaces in a forwarding state behave normally, they send and receive all normal traffic
		- Interfaces in a blocking state only send and receive STP messages (called BDPUs = Bridge Protocol Data Units)
			- SPT still uses the term 'bridge'. However, when we use the term 'bridge' we really mean 'switch'. Bridges are not used in modern networks
	- By selecting which ports are forwarding and which ports are blocking, STP creates a single path to/from each point in the network. This prevents Layer 2 loops
	- There is a set process that STP uses to determine which ports should be forwarding and which should be blocking
		- STP-enabled switches send/receive Hello BPDUs out of all interfaces, the default timer is 2 seconds
		- If a switch receives a Hello BPDU on an interface, it knows that interface is connected to another switch
			- Switches use one field in the STP BPDU, the bridge ID field, to elect a root bridge for the network
			- The switch with the lowest Bridge ID becomes the root bridge
				- ALL ports on the root bridge are put in a forwarding state, and other switches in the topology must have a path to reach the root bridge
				- When a switch is powered on, it assumes it is the root bridge
				- It will only give up its position if it receives a superior BPDU (lower bridge ID)
				- Once the topology has converged and all switches agree on the root bridge, only the root bridge sends BPDUs
				- Other switches in the network will forward these BPDUs, but will not generate their own original BPDUs
			- Each remaining switch will select ONE of its interfaces to be its root port. The interface with the lowest root cost will be the root port. Root ports are also in a forwarding state
				- ### Root port selection:
					1. Lowest root cost
					2. Lowest **neighbor** bridge ID
					3. Lowest **neighbor** port ID
						- STP Port ID = port priority (default 128) + port number
			- Each remaining collision domain will select ONE interface to be a designated port (forwarding state). The other port in the collision domain will be non-designated (blocking)
				- Designated port selection:
					1. Interface on switch with the lowest root cost
					2. Interface on switch with the lowest bridge ID

### Spanning Tree Path Cost

| Speed    | STP Cost |
| -------- | -------- |
| 10 Mbps  | 100      |
| 100 Mbps | 19       |
| 1Gbps    | 4        |
| 10Gbps   | 2        |

- Cisco switches use a version of STP called PVST (Per-VLAN Spanning Tree)
	- PVST runs a separate STP 'instance' in each VLAN, so in each VLAN different interfaces can be forwarding/blocking