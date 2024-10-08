---
tags:
  - devops
  - ccna
---
- **Trunk Port** - Carries traffic from multiple VLANs over a single interface
- Switches will tag all frames that they send over a trunk link, allowing the receiving switch to know which VLAN the frame belongs to
	- ### Trunk ports = 'tagged' ports
	- ### Access ports = 'untagged' ports
- ## Two main trunking protocols:
	- ### ISL (Inter-Switch Link)
		- Old Cisco proprietary protocol created before the industry standard IEEE 802.1Q
	- ### IEEE 802.1Q
		- Also called dot1q
		- Industry standard protocol created by the IEEE (Institute of Electrical and Electronics Engineers)
		- Tag is inserted between the Source and Type/Length parts of the Ethernet header
		- 4 bytes (32 bits) in length
		- Consists of 2 main fields:
			- #### Tag Protocol Identifier (TPID)
				- 16 bits (2 bytes) long
				- Always set to a value of 0x8100. This indicates that the frame is 802.1Q-tagged
			- #### Tag control Information (TCI)
				- ##### PCP (Priority Code Point)
					- 3 bits in length
					- Used for Class of Service (CoS), which prioritizes important traffic in congested networks
				- ##### DEI (Drop Eligible Indicator)
					- 1 bit in length
					- Used to indicate frames that can be dropped if the network is congested
				- ##### VID (VLAN ID)
					- 12 bits in length
					- Identifies the VLAN the frame belongs to
					- 4096 total VLANs (2^12), range of 0-4095
						- VLANs 0 and 4095 are reserved and cannot be used
						- Therefore the actual range of VLANs is 1-4094
- The range of VLANs (1-4094) is divided into two sections:
	- Normal VLANs (1-1005)
	- Extended VLANs (1006-4094)
		- Some older devices cannot use the extended VLAN range, but most modern switches should
- 802.1Q has a feature called **native VLAN**
	- The native VLAN is VLAN 1 by default on all trunk ports, however this can be manually configured on each trunk port
	- The switch does not add an 802.1Q tag to frames in the native VLAN
	- When a switch receives an untagged frame on a  trunk port, it assumes the frame belongs to the native VLAN
		- It's very important that the native VLAN matches
	- For security purposes, it is best to change the native VLAN to an unused VLAN
- ## Configure Trunk port:
	- Select interface: 'interface (range) *interfaces*'
	- `switchport trunk encapsulation *type*` command sets encapsulation type of port (dot1q). Only needed if switch supports dot1q and isl
	- `switchport mode trunk` command ports as trunk ports
	- `switchport trunk allowed vlan *vlan-ids*` command allows vlans onto a trunk
- `show interfaces trunk` command shows trunk info
- `switchport trunk native vlan *vlan-id*` command changes the native vlan
- ## Router on a Stick (ROAS)
	- Used to route between multiple VLANs using a single interface on the router and switch
	- The switch interface is configured as a regular trunk
	- The router interface is configured using subinterfaces:
		- `interface *interface*.*subinterface*` command enters subinterface configuration (ex. g0/0.10). Subinterface tends to match the VLAN interface to make it easier to understand
		- `encapsulation dot1q *vlan-id*` command sets subinterface encapsulation 
		- `ip address *ip-address* *subnet*` command will set an IP for the subinterface
	- The router will behave as if frames arriving with a certain VLAN tag have arrived on the subinterface configured with that VLAN tag
	- The router will tag frames sent onto of each subinterface with the VLAN tag configured on the subinterface