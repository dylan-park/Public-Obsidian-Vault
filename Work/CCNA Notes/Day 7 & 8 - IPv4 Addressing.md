---
tags:
  - work
  - ccna
---
## Day 7

- IP addresses are 32 bits (4 bytes) in length
	- each 8 bit group is called an octet
- IP addresses are written in dotted decimal
- the /24 at the end of an IP address dictates that the first 24 bits represent the network portion of the address, and the remaining 8 represent the host.
  
- ### IPv4 Addresses Classes

| Class | First octet | First octet numeric range | Prefix Length | Number of networks | Addresses per network |
| ----- | ----------- | ------------------------- | ------------- | ------------------ | --------------------- |
| A     | 0xxxxxxx    | 0-127                     | /8            | 128 (2^7)          | 16,777,216 (2^24)     |
| B     | 10xxxxxx    | 128-191                   | /16           | 16,384 (2^14)      | 65,536 (2^16)         |
| C     | 110xxxxx    | 192-223                   | /24           | 2,097,152 (2^11)   | 256 (2^8)             |
| D     | 1110xxxx    | 224-239                   |               |                    |                       |
| E     | 1111xxxx    | 240-255                   |               |                    |                       |
	- D: Multicast addresses
	- E: Reserved (experimental)
- 127.0.0.0–127.255.255.255 = Loopback Addresses
	- Used to test the 'network stack' (OSI, TCP/IP) on the local device

| Class | Shash Notation | Netmask       |
| ----- | -------------- | ------------- |
| A     | /8             | 255.0.0.0     |
| B     | /16            | 255.255.0.0   |
| C     | /24            | 255.255.255.0 |

- Host portion of the address is all 0's = Network Address
	- The network address cannot be assigned to a host
- Host portion of the address is all 1's = Broadcast Address
	- The broadcast address cannot be assigned to a host
	- is used to send a packet to every host on the network

## Day 8

- ### Maximum Hosts per Network
	- 2 less than max addresses on the network: (2^n)-2
	- #### Class C: 192.168.1.0/24 → 192.168.1.255/24
		- Host portion = 8 bits = 2^8 = 256
		- Maximum hosts per network = (2^8)-2=254
	- #### Class B: 172.16.0.0/16 → 172.16.255.255/16
		- Host portion = 16 bits = 2^16 = 65,536
		- Maximum hosts per network = (2^16)-2=65,534
	- #### Class A: 10.0.0.0/8 → 10.255.255.255/8
		- Host portion = 24 bits = 2^24 = 16,777,216
		- Maximum hosts per network = (2^24)-2=16,777,214
- ### First/Last Usable Address
	- #### Class C: 192.168.1.0/24 → 192.168.1.255/24
		- Add 1 to network address = 192.168.1.1 = first usable address
		- Subtract 1 from broadcast address = 192.168.1.254 = last usable address
	- #### Class B: 172.16.0.0/16 → 172.16.255.255/16
		- Add 1 to network address = 172.16.0.1 = first usable address
		- Subtract 1 from broadcast address = 172.16.255.254 = last usable address
	- #### Class A: 10.0.0.0/8 → 10.255.255.255/8
		- Add 1 to network address = 10.0.0.1 = first usable address
		- Subtract 1 from broadcast address = 10.255.255.254 = last usable address
- `show ip interface brief` command lists all interfaces, their IPs (if assigned, otherwise unassigned), the method by which each interface's ip was set, the 'Layer 1' status of the interface, and the protocol ('Layer 2' status).
	- Administratively down status = Interface has been disabled with the 'shutdown' command
		- This is the default status of Cisco router interfaces
- `interface *interface-id*` command enters interface config mode
	- `ip address *address* *subnet-mask*` command sets the ip address for the interface
	- `no shutdown` command enables the interface
	- `description *text*` command sets the interface's description
- `show interfaces description` command shows interfaces w/ descriptions