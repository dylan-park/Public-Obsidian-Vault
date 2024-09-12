---
tags:
  - devops
  - ccna
---
- **Network Route** - A route to a network/subnet (mask length < /32)
- **Host Route** - A route to a specific host (/32 mask)
- ## Dynamic Routing:
	- Router can use dynamic routing protocols to advertise information about the routes they know to other routers
	- They form 'adjacencies'/ 'neighbor relationships' / 'neighborships with adjacent routers to exchange this information'
	- If multiple routes to a destination are learned, the router determines which route is superior and adds it to the routing table. It uses the 'metric' of the route to decide which is superior (lower metric = superior)
	- Dynamic routing protocols can be divided into two main categories:
		- ### IGP (Interior Gateway Protocol)
			- Used to share routes within a single autonomous system (AS), which is a single organization (i.e. a company)
			- Algorithm Types:
				- #### Distance Vector
					- Were invented before Link State protocols
					- Early examples are RIPv1 and Cisco's proprietary protocol IGRP (which was updated to EIGRP)
					- Distance vector protocols operate by sending the following to their directly connected neighbors
						- Their known destination networks
						- Their metric to reach their known destination networks
					- This method of sharing route information is often called 'routing by rumor'
					- This is because the router doesn't know about the network beyond its neighbors. It only knows the information that its neighbors tell it
					- Called 'distance vector' because the routers only learn the 'distance' (metric) and 'vector' (direction, the next-hop router) of each route
					- Examples:
						- **RIP** (Routing Information Protocol)
						- **EIGRP** (Enhanced Interior Gateway Routing Protocol)
				- #### Link State
					- When using a link state routing protocol, every router creates a 'connectivity map' of the network
					- To allow this, each router advertises information about its interfaces (connected networks) to its neighbors. These advertisements are passed along to other routers, until all routers in the network develop the same map of the network
					- Each router independently uses this map to calculate the best routes to each destination
					- Link state protocols use more resources (CPU) on the router, because more information is shared. However, link state protocols tend to be faster in reacting to changes in the network than distance vector protocols
					- Examples:
						- **OSPF** (Open Shortest Path First)
						- **IS-IS** (Intermediate System to Intermediate System) ^ccna-is-is
		- ### EGP (Exterior Gateway Protocol)
			- Used to share routes *between* different autonomous systems
			- Algorithm Types:
				- #### Path Vector
					- **BGP** (Border Gateway Protocol)
	- ### Dynamic Routing Protocol Metrics:
		- A router's route table contains the best route to each destination network it knows about
		- If a router using a dynamic routing protocol learns two different routes to the same destination, it uses the metric value of the routes to determine which is best. A lower metric = better
		- Each routing protocol uses a different metric to determine which route is best
		- If a router learns two (or more) routes via the **same routing protocol** to the **same destination** (same network address, same subnet mask) with the **same metric**, both will be added to the routing table. Traffic will be load-balanced over both routes
			- **ECMP** (Equal Cost Multi-Path)
				- This can be done with static routes as well
		- In most cases, a company will only use a single IGP - usually OSPF or EIGRP
		- However, in some rare cases they might use two. For example, if two companies connect their networks to share information, two different routing protocols might be in use
		- Metric is used to compare routes learned via the same routing protocol
		- Different routing protocols use totally different metrics, so they cannot be compared
		- The **administrative distance (AD)** is used to determine which routing protocol is preferred
			- The lower AD is preferred, and indicates that the routing protocol is considered more 'trustworthy' (more likely to select good routes)
			- You can change the AD of a routing protocol
			- You can also change the AD of a static route (during `ip route` command)
				- By changing the AD of a static route, you can make it less preferred than routes learned by a dynamic routing protocol to the same destination
				- Make sure the AD is higher than the routing protocol's AD
				- Called a 'floating static route'
				- The route will be inactive unless the route learned by the dynamic routing protocol is removed

### Dynamic Routing Protocol Metrics

| IGP   | Metric                                         | Explanation                                                                                                                                                                    |
| ----- | ---------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **RIP**   | Hop count                                      | Each router in the path counts as one 'hop'. The total metric is the total number of hops to the destination. **Links of all speeds are equal.**                               |
| **EIGRP** | Metric based on bandwidth & delay (by default) | Complex formula that can take into account many values. By default, the bandwidth of the **slowest link in the route** and the total delay of all links in the route are used. |
| **OSPF**  | Cost                                           | The cost of e ach link is calculated based on **bandwidth**. The total metric is the total cost of each link in the route.                                                     |
| **IS-IS** | Cost                                           | The total metric is the total cost of each link in the route. The cost of each link is **not** automatically calculated by default. All links have a cost of 10 by default.    |
### Administrative Distance

| Route protocol/type | AD  |
| ------------------- | --- |
| Directly connected  | 0   |
| Static              | 1   |
| External BGP (eBGP) | 20  |
| EIGRP               | 90  |
| IGRP                | 100 |
| OSPF                | 110 |
| IS-IS               | 115 |
| RIP                 | 120 |
| EIGRP (external)    | 170 |
| Internal BGP (iBGP) | 200 |
| Unusable route      | 255 |


