---
tags:
  - work
  - ccna
---
- ## OSPF (Open Shortest Path First) ^ccna-ospf
	- Uses the **Shortest Path First** algorithm of Dutch computer scientist Edger Dijkstra (aka **Dijkstra's algorithm**)
	- ### Three Versions
		- **OSPF v1** (1989) - OLD, not in use anymore
		- **OSPFv2** (1998) - Used for IPv4
		- **OSPFv3** (2008) - Used for IPv6 (Can also be used for IPv4, but usually v2 is used)
	- Routers store information about the network in LSAs (Link State Advertisements), which are organized in a structure called the LSDB (Link State Database)
	- Routers will flood LSAs until all routers in the OSPF *area* develop the same map of the network (LSDB)
	- ### OSPF Steps:
		1. **Become neighbors** with other routers connected to the same segment
		2. **Exchange LSAs** with neighbor routers
		3. **Calculate the best routes** to each destination, and insert them into the routing table
	- ### OSPF uses **areas** to divide up the network
		-  **Area** - A set of routers and links that share the same **LSDB**
		- **Backbone Area (Area 0)** - An area that all other areas must connect to
		- **Internal Routers** - Routers with all interfaces in the same area
		- **Area Border Routers (ABRs)** - Routers with interfaces in multiple areas
			- ABRs maintain a separate LSDB for each area they are connected to. It is recommended that you connect an ABR to a maximum of 2 areas. Connecting an ABR to 3+ areas can overburden the router
		- **Autonomous System Boundary Router (ASBR)** - An OSPF router that connects the OSPF network to an external network
		- **Backbone Router** - Routers connected to the backbone area (area 0)
		- **Intra-area Route** - A route to a destination inside the same OSPF area
		- **Interarea Route** - A route to a destination in a different OSPF area
		  
		- Small networks can be single-area without any negative effects on performance
		- In larger networks, a single-area design can have negative effects:
			- The SPF algorithm takes more time to calculate routes
			- The SPF algorithm requires exponentially more processing power on the routers
			- The larger LSDB takes up more memory on the routers
			- Any small change in the network causes every router to flood LSAs and run the SPF algorithm again
		- OSPF areas should be contiguous
			- Each individual area should be connected, not divided up
		- All OSPF areas must have at least one **ABR** connected to the backbone area
		- OSPF interfaces in the same subnet must be in the same area
	- ### OSPF Configuration:
		- `router ospf *process-id*` command enters OSPF config mode
			- A router can run multiple OSPF processes, the process id identifies which one you are configuring
			- Generally you will only use one process id
			- The OSPF process ID is **locally significant**, routers with different process IDs can become OSPF neighbors
		- `network *ip-address* *wildcard-netmask* area *area-number*` command tells OSPF to:
			- Look for any interfaces with an IP address contained in the range specified in the **network** command
			- Activate OSPF on the interface in the specified **area**
			- The router will then try to become OSPF neighbors with other OSPF-activated neighbor routers
		- `passive-interface *interface*` command tells the router to stop sending OSPF 'hello' messages out of the specified interface
			- However, the router will continue to send LSAs informing its neighbors about the subnet configured on the interface
			- You should always use this command on interfaces which don't have any OSPF neighbors
		- `ip route 0.0.0.0 0.0.0.0 *default-route-next-hop-ip*` command sets a default route into OSPF
		- `default-information originate` command advertises a default route into OSPF
		- `show ip protocols` command shows routing protocol info
		- `router-id *router-id*` command manually sets router id
			- `clear ip ospf process` command must be run after manually setting router id in order for it to take effect
		- #### Router ID order of priority:
			1. Manual Configuration
			2. Highest IP address on a loopback interface
			3. Highest IP address on a physical interface
		- `maximum-paths *number*` command sets maximum paths for OSPF
		- `distance *distance*` command sets distance for OSPF