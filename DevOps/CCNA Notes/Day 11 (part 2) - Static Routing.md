---
tags:
  - devops
  - ccna
---
![[Assets/Pasted image 20240206202017.png]]
- End hosts like PC1 and PC4 can send packets directly to destinations in their connected network
	- PC1 is connected to 192.168.1.0/24, PC4 is connected to 192.168.4.0/24
- To send packets to destinations outside their local network, they must send the packets to their default gateway
- The default gateway configuration is also called a default route
	- It is a route to 0.0.0.0/0 = all netmask bits set to 0. Includes all addresses from 0.0.0.0 to 255.255.255.255
		- The default route is the least specific route possible, because it includes all IP addresses
- End hosts usually have no need for any more specific routes
	- They just need to know: to send packets outside my local network, I should send them to my default gateway
- Each router needs two routes: a route to 192.168.1.0/24 and a route to 192.168.4.0/24
	- This ensures two-way reachability
- `(config)# 'ip route *ip-address* *netmask* *next-hop*'` command sets a static route
	- `(config)# 'ip route *ip-address* *netmask* *exit-interface* *next-hop*'` command sets a static route, but specifies the specific interface the packets should be forwarded to
- A default route is often used to direct traffic to the Internet
	- More specific routes are used for destinations in an internal network
	- Traffic to destinations outside the internal network is sent to the Internet
- `(config)# 'ip route *0.0.0.0* *0.0.0.0* *next-hop*'` command sets the default gateway (default route, gateway of last resort)