---
tags:
  - work
  - ccna
---
- ## OSPF (Open Shortest Path First)
	- ### OSPF Cost:
		- OSPF's metric is called **cost**
		- It is automatically calculated based on the bandwidth (speed) of the interface
		- It is calculated by dividing a **reference bandwidth** value by the interface's bandwidth
			- The default reference bandwidth is 100 Mbps
			- All values less than 1 will be converted to 1
				- Therefore, FastEthernet, Gigabit Ethernet, 10Gig Ethernet, etc. are all equal and all have a cost of 1 by default
				- Loopback interfaces have a cost of 1
					- **Reference**: 100 Mbps / **Interface**: 10 Mbps = cost of **10**
					- **Reference**: 100 Mbps / **Interface**: 100 Mbps = cost of **1**
					- **Reference**: 100 Mbps / **Interface**: 1000 Mbps = cost of **1**
					- **Reference**: 100 Mbps / **Interface**: 10000 Mbps = cost of **1**
			- You can change the reference bandwidth with the `auto-cost reference-bandwidth *megabits-per-second*` command
				- You should configure a reference bandwidth greater than the fastest links in your network (to allow for future upgrades)
				- You should configure the same reference bandwidth on all OSPF routers in the network
		- The OSPF cost to a destination is the total cost of the 'outgoing/exit interfaces'
		- `ip ospf cost *cost*` command sets the specific cost of an interface
		- One more option to change the OSPF cost of an interface is to change the bandwidth of the interface with the bandwidth command
			- Although the bandwidth matches the interface speed by default, changing the interface bandwidth <u>doesn't actually change the speed at which the interface operates</u>
			- The bandwidth is just a value that is used to calculate OSPF cost, EIGRP metric, etc.
			- `bandwidth *kbps*` command sets the interface's bandwidth used for calculations
			- Because the bandwidth value is used in other calculations, it is not recommended to change this value to alter the interface's OSPF cost
	- ### OSPF Neighbors:
		- Making sure that routers successfully become OSPF neighbors is the main task in configuring and troubleshooting OSPF
		- Once routers become neighbors, they automatically do the work of sharing network information, calculating routes, etc.
		- When OSPF is activated on an interface, the router starts sending OSPF **hello** messages out of the interface at regular intervals (determined by the **hello timer**). These are used to introduce the router to potential OSPF neighbors
			- The default hello timer is 10 seconds on an Ethernet Connection
		- Hello messages are multicast to 224.0.0.5 (multicast address for all OSPF routers)
		- OSPF messages are encapsulated in an IP header, with a value of 89 in the Protocol field
		- #### Neighbor States:
			- ##### Down State:
				- OSPF is activated on both interfaces of a connection
				- An OSPF hello message is sent
				- The sending interface doesn't know about any OSPF neighbors yet, so the current neighbor state is **Down**
			- ##### Init State:
				- When the interface on the other end of the connection receives the hello packet, it will add an entry for the sending router to its OSPF neighbor table
				- In the receiving router's table, the relationship with the sending router is now in the Init state
					- The sending router has no idea about the receiving router, so the sending router has no entries in its neighbor table
				- **Init** state = Hello packet received, but own router ID is not in the Hello packet
			- ##### 2-way State:
				- The receiving router will send a Hello packet containing the RIP of both routers
				- The sending router will insert the receiving router into its OSPF neighbor table in the 2-way state
				- The sending router will send another Hello message, this time containing the receiving router's RID
				- Now both routers are in the **2-way** state
				- The 2-way state means the router has received a Hello packet with its own RID in it
				- If both routers reach the 2-way state, it means that all the conditions have been met for them to become OSPF neighbors. They are now ready to share LSAs to build a common LSDB
				- In some network types, a DR (Designated Router) and BDR (Backup Designated Router) will be elected at this point
			- ##### Exstart State:
				- The two routers will now prepare to exchange information about their LSDB
				- Before that, they have to choose which one will start the exchange
					- The router with the higher RID will become the **Master** and initiate the exchange. The router with the lower RID will become the **Slave**
					- To decide the Master and Slave, they exchange DBD (Database Description) packets
				- All of this is done in the **Exstart** state
			- ##### Exchange State:
				- In the **Exchange** state, the routers exchange DBDs which contain a list of the LSAs in their LSDB
				- These DBDs do not include detailed information about the LSAs, just basic information
				- The routers compare the information in the DBD they received to the information in their own LSDB to determine which LSAs they must receive from their neighbor
			- ##### Loading State:
				- In the **Loading** state, routers send LSR (Link State Request) messages to request that their neighbors send them any LSAs they don't have
				- LSAs are sent in LSU (Link State Update) messages
				- The routers send LSAck messages to acknowledge that they received the LSAs
			- ##### Full State:
				- In the **Full** state, the routers have a full OSPF adjacency and identical LSDBs
				- They continue to send and listen for Hello packets (every 10 seconds by default) to maintain the neighbor adjacency
				- Every time a Hello packet is received, the 'Dead' timer (40 seconds by default) is reset
				- If the Dead timer counts down to 0 and no Hello message is received, the neighbor is removed
				- The routers will continue to share LSAs as the network changes to make sure each router has a complete and accurate map of the network (LSDB)
			- ![[Pasted image 20240305204548.png]]
		- `show ip ospf neighbor` command will show a router's neighbor table
	- `ip ospf *process-id area *area*` command activates OSPF directly on an interface
	- `passive-interface default` command configures ALL interfaces as OSPF passive by default on a router
		- Then, use `no passive-interface *int-id*` command to specifically set an interface as active

## OSPF Message Types

| Type | Name                               | Purpose                                                                                  |
| ---- | ---------------------------------- | ---------------------------------------------------------------------------------------- |
| 1    | Hello                              | Neighbor discovery and maintenance.                                                      |
| 2    | Database Description (DBD)         | Summary of the LSDB of the router. Used to check if the LSDB of each router is the same. |
| 3    | Link-State Request (LSR)           | Requests specific LSAs from the neighbor.                                                |
| 4    | Link-State Update (LSU)            | Sends specific LSAs to the neighbor.                                                     |
| 5    | Link-State Acknowledgement (LSAck) | Used to acknowledge that the router received a message.                                  |
