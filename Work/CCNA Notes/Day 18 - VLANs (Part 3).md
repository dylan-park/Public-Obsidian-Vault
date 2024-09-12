---
tags:
  - work
  - ccna
---
- There are 2 methods of configuring the native VLAN on a router:
	- `encapsulation dot1q *vlan-id* native` command on the router subinterface
	- Configure the IP address for the native VLAN on the router's physical interface
![[Pasted image 20240215184105.png]]
- **Layer 3 (Multilayer) Switch** - capable of both switching AND routing
	- It is 'Layer 3 aware'
	- You can assign IP addresses to its interfaces, like a router
	- You can create virtual interfaces for each VLAN, and assign IP addresses to those interfaces
	- You can configure routes on it, just like a router
	- It can be used for inter-VLAN routing
- **SVIs (Switch Virtual Interfaces)** - Virtual interfaces you can assign IP addresses to in a multilayer switch ^ccna-svi
	- Configure each PC to use the SVI (NOT the router) as their gateway address
	- To send traffic  to different subnets/VLANs, the PCs will send traffic to the switch, and the switch will route the traffic
- `ip routing` command enables Layer 3 routing on the switch
- `no swithchport` command configures the interface as a routed port (Layer 3 port)