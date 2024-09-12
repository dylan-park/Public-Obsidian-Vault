---
tags:
  - work
  - ccna
---
- ## EUI-64
	- EUI = Extended Unique Identifier
	- (Modified) EUI-64 is a method of converting a MAC address (48 bits) into a 64-bit interface identifier
		- This interface identifier can then become the 'host portion' of a /64 IPv6 address
	- ### How to convert the MAC address:
		1. Divide the MAC address in half
			- 1234 5678 90AB → 1234 56 **|** 78 90AB
		2. Insert FFFE in the middle
			- 1234 56**FF FE**78 90AB
		3. Invert the 7th bit
			- 1**2**34 56FF FE78 90AB → 2 = 00**1**0 → 0000 → 1**0**34 56FF FE78 90AB
	- `ipv6 address *network-prefix*/64 eui-64` command tells the router to use this prefix plus the EUI-64 interface identifier to generate an IPv6 address
- ## IPv6 Address Types
	- ### Global Unicast Address
		- Global unicast IPv6 addresses are public addresses which can be used over the internet
		- Must register to use them. Because they are public addresses, it is expected that they are globally unique
		- Originally defined as the 2000::/3 block (2000:\: to 3FFF:FFFF:FFFF:FFFF:FFFF:FFFF:FFFF:FFFF)
		- Now defined as all addresses which aren't reserved for other purposes
		- ![[Pasted image 20240319185123.png]]
	- ### Unique Local Address
		- Unique local IPv6 addresses are *private* addresses which cannot be used over the internet
		- You do not need to register to use them. They can be used freely within internal networks, and don't need to be globally unique, and they can't be routed over the internet
			- The global ID should be unique, so that addresses don't overlap when companies merge
		- Uses address block FC00::/7(FC00:\: to FDFF:FFFF:FFFF:FFFF:FFFF:FFFF:FFFF:FFFF)
			- However, a later update requires the 8th bit to be set to 1, so the first two digits must be FD
		- ![[Pasted image 20240321171734.png]]
	- ### Link Local Address
		- Link-local IPv6 addresses are automatically generated on IPv6-enabled interfaces
		- `ipv6 enable` command on an interface enables IPv6 on an interface
		- Uses address block FE80::/10 (FE80:\: to FEBF:FFFF:FFFF:FFFF:FFFF:FFFF:FFFF:FFFF)
			- However, the standard states that the 54 bits after FE80/10 should be all 0, so you won't see link local addresses beginning with FE9, FEA, or FEB, **Only FE8**
		- The interface ID is generated using EUI-64 rules
		- *Link-local* means that these addresses are used for communication within a single link (subnet)
			- Routers **will not** route packets with a link-local destination IPv6 address
		- Common uses of link-local addresses:
			- routing protocol peerings (OSPFv3 uses link-local addresses for neighbor adjacencies)
			- next-hop addresses for static routes
			- *Neighbor Discovery Protocol* (NDP, IPv6's replacement for ARP) uses link-local addresses to function
	- ### Multicast Address
		- **Unicast** addresses are one-to-one
			- One source to one destination
		- **Broadcast** addresses are one-to-all
			- One source to all destinations (within the subnet)
		- **Multicast** addresses are one-to-many
			- One source to multiple destinations (that have joined the specific *multicast group*)
		- IPv6 uses range FF00::/8 for multicast (FF00:\: to FFFF:FFFF:FFFF:FFFF:FFFF:FFFF:FFFF:FFFF)
		- **IPv6 doesn't use broadcast** (there is no 'broadcast address' in IPv6)
		- #### Multicast Address Scopes:
			- IPv6 defines multiple multicast 'scopes' which indicate how far the packet should be forwarded
			- IPv6 multicast scopes:
				- **Interface-local** (FF01): The packet doesn't leave the local device. Can be used to send traffic to a service within the local device
				- **Link-local** (FF02): The packet remains in the local subnet. Routers will not route the packet between subnets
				- **Site-local** (FF05): The packet can be forwarded by routers. Should be limited to a single physical location (not forwarded over a WAN)
				- **Organization-local** (FF08): Wider in scope than site-local (an entire company/organization)
				- **Global** (FF0E): No boundaries. Possible to be routed over the internet
	- ### Anycast Address
		- Anycast is a new feature op IPv6
		- Anycast is 'one-to-one-of-many'
		- ![[Pasted image 20240321173759.png]]
		- Multiple routers are configured with the same IPv6 address
			- They use a routing protocol to advertise the address
			- When hosts send packets to that destination address, routers will forward it to the nearest router configured with that IP address (based on routing metric)
		- There is no specific address range for anycast addresses
			- With the `ipv6 address *address*/*netmask anycast*` command, you can use a regular unicast address (global unicast, unique local) and specify it as an anycast address
	- ### Other IPv6 Addresses
		- :: = Unspecified IPv6 address
			- Can be used when a device doesn't yet know its IPv6 address
			- IPv6 default routes are configured to ::/0
			- IPv4 equivalent = 0.0.0.0
		- ::1 = Loopback address
			- Used to test the protocol stack on the local device
			- Messages sent to this address are processed within the local device, but not sent to other devices
			- IPv4 equivalent = 172.0.0.0/8


## Common Multicast Address Examples 

| Purpose                                       | IPv6 Address | IPv4 Address |
| --------------------------------------------- | ------------ | ------------ |
| All nodes/hosts<br>(functions like broadcast) | FF02::1      | 224.0.0.1    |
| All routers                                   | FF02::2      | 224.0.0.2    |
| All OSPF routers                              | FF02::5      | 224.0.0.5    |
| All OSPF DRs/BDRs                             | FF02::6      | 224.0.0.6    |
| All RIP routers                               | FF02::9      | 224.0.0.9    |
| All EIGRP routers                             | FF02::A      | 224.0.0.10   |
