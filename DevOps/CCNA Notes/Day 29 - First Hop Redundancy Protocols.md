---
tags:
  - devops
  - ccna
---
- A first hop redundancy protocol (FHRP) is designed to protect the default gateway used on a subnetwork by allowing two or more routers to provide backup for that address
	- In the event of a failure of an active router, the backup router will take over the address, usually within a few seconds
	- FHRPs are 'non-preemptive', the current active router will not automatically give up its role, even if the former active router returns
		- You can change this setting to make the original active router become active again after returning
		- ### FHRP Steps:
			1. A **virtual IP** is configured on the two routers, and a **virtual MAC** is generated for the virtual IP (each FHRP used a different format for the virtual MAC)
			2. An **active** and **standby** router are elected
			3. End hosts in the network are configured to use the virtual IP as their default gateway
			4. The active router replies to ARP requests using the virtual MAC address, so traffic destined for other networks will be sent to it
			5. If the active router fails, the standby becomes the next active router
				- The new active router will send **[[Day 51 - Dynamic ARP Inspection#^ccna-gratuitous-arp|gratuitous ARP]]** messages so that switches will update their MAC address tables
				- It now functions as the default gateway
			6. If the old active router comes back online, by default it won't take back its role as the active router, it will become the standby router
				- You can configure 'preemption', so that the old active router does take back its old role
- ## HSRP (Hot Standby Router Protocol)
	- Cisco proprietary
	- An **active** and **standby** router are elected
		- Determined in this order:
			1. Highest priority (default 100)
			2. Highest IP address
	- There are two versions: **version 1** and **version 2**
		- Version 2 adds IPv6 support and increases the number of groups that can be configured
	- Multicast IPv4 address:
		- **v1**: 224.0.0.2
		- **v2**: 224.0.0.102
	- Virtual MAC address:
		- **v1**: 0000.0c07.ac**XX** (**XX** = HSRP group number)
		- **v2**: 0000.0c9f.f**XXX** (**XXX** = HSRP group number)
	- In a situation with multiple subnets/VLANs, you can configure a different active router in each subnet/VLAN to load balance
	- ### Configuration:
		- `standby *group-number* ip *virtual-ip-address*` command sets the virtual IP on the selected interface
		- `standby *group-number* priority *priority*` command sets the priority on the selected interface, to determine which router will become the active router
		- `standby *group-number* preempt*` command causes the router to take the role of active router, even if another router already has the role
			- Must have a higher priority or IP address
			- Only necessary on the router you want to become active
		- `standby version *version*` command will change HSRP version
			- versions must match between routers
- ## VRRP (Virtual Router Redundancy Protocol)
	- Open standard
	- A **master** and **backup** router are elected
	- Multicast IPv4 address: 224.0.0.18
	- Virtual MAC address: 0000.5e00.01**XX** (**XX** = VRRP group number)
	- In a situation with multiple subnets/VLANs, you can configure a different master router in each subnet/VLAN to load balance
- ## GLBP (Gateway Load Balancing Protocol)
	- Cisco proprietary
	- Load balances among multiple routers <u>within a single subnet</u>
	- An **AVG (Active Virtual Gateway)** is elected
	- Up to four **AVFs (Active Virtual Forwarders)** are assigned by the AVG (the AVG itself can also be an AVF)
	- Each AVF acts as the default gateway for a portion of the hosts in the subnet
	- Multicast IPv4 address: 224.0.0.102
	- Virtual MAC address: 0007.b400.**XXYY** (**XX** = GLBP group number, **YY** = AVF number)