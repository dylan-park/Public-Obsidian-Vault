---
tags:
  - devops
  - ccna
---
- ## The Purpose of DNS
	- DNS is used to resolve human-readable names (google.com) to IP addresses
	- Machines such as PCs don't use names, they use addresses (i.e. IPv4/IPv6)
	- Names are much easier for us to use and remember than IP addresses
	- The DNS server(s) your device uses can be manually configured or learned via [[Day 39 - DHCP|DHCP]]
	- DNS 'A' records are used to map names to IPv4 addresses
	- DNS 'AAAA' records are used to map names to IPv6 addresses
	- Standard DNS queries/responses typically use **UDP**. TCP is used for DNS messages greater than 512 bytes
		- In either case port 53 is used
- ## DNS Cache
	- Devices will save the DNS server's responses to a local DNS cache, this means they don't have to query the server every single time they want to access a particular destination
	- `ipconfig /displaydns` command on Windows displays the DNS cache
- ## Host File
	- Most devices have a host file in addition to a DNS cache, it is basically a list of names and IP addresses
		- On Windows, it is located at C:\\Windows\\System32\\drivers\\etc
- ## DNS in Cisco IOS
	- For hosts in a network to use DNS, you don't need to configure DNS on the routers, they will simply forward the DNS messages like any other packets
	- However, a Cisco router can be configured as a DNS server, although it's rare
		- If an internal DNS server is used, usually it's a Windows or Linux server
	- A Cisco router can also be configured as a DNS client
	- ### DNS Configuration
		- `ip dns server` command configures a router to act as a DNS server
		- `ip host *name* *ip-address*` command configures a list of hostname/IP address mappings
		- `ip name-server *ip-address*` command configures an external DNS server that the router will query if the requested record isn't in its host table
		- `ip domain lookup` command enables the router to perform DNS queries
			- Enabled by default
		- `show hosts` command shows cached and configured hosts