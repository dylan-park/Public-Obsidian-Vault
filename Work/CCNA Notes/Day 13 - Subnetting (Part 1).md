---
tags:
  - work
  - ccna
---
- **IANA (Internet Assigned Numbers Authority)** - assigns IPv4 addresses/networks to companies based on their size
	- For example, a very large company might receive a class A or class B network, while a small company might receive a class C network
	- This system lead to many wasted IP addresses
	![[Pasted image 20240209145738.png]]
- The IETF (Internet Engineering Task Force) introduced **CIDR** in 1993 to replace t he 'classful' addressing system
- With CIDR, the class/netmask requirements were removed, allowing for larger networks to be split into smaller ones, allowing greater efficiency.
	- These smaller networks are called subnetworks, or **subnets**

## Class C Subnetting

| Prefix Length | Netmask         | Number of usable addresses     | Possible Subnets (2^x) |
| ------------- | --------------- | ------------------------------ | ---------------------- |
| /25           | 255.255.255.128 | 126 (2^7-2)                    | 2                      |
| /26           | 255.255.255.192 | 62 (2^6-2)                     | 4                      |
| /27           | 255.255.255.224 | 30 (2^5-2)                     | 8                      |
| /28           | 255.255.255.240 | 14 (2^4-2)                     | 16                     |
| /29           | 255.255.255.248 | 6 (2^3-2)                      | 32                     |
| /30           | 255.255.255.252 | 2 (2^2-2)                      | 64                     |
| **/31**       | 255.255.255.254 | 2 (2^1) (No broadcast/network) | 128                    |
| **/32**       | 255.255.255.255 | 1 (No broadcast/network)       | 256                    |
