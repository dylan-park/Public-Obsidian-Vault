---
tags:
  - devops
  - ccna
---
- ## Layer 2 Discovery Protocols
	- Layer 2 discovery protocols such as CDP and LLDP share information with and discover information about neighboring (connected) devices
	- The shared information includes host name, IP address, device type, etc.
	- CDP is a Cisco proprietary protocol
	- LLDP is an industry standard protocol (IEEE 802.1AB)
	- Because they share information about the devices in the network, they can be considered a security risk and are often not used. It is up to the network engineer/admin to decide if they want to use them in the network or not
- ## CDP
	- CDP (Cisco Discovery Protocol) is a Cisco proprietary protocol
	- It is enabled on Cisco devices (routers, switches, firewalls, IP phones etc.) by default
	- CDP messages are periodically sent to multicast MAC address 0100.0CCC.CCCC
	- When a device receives a CDP message, it processes and discards the message. It does NOT forward it to other devices
	- By default, messages are sent once every 60 seconds by all interfaces in an up state
	- By default, the CDP holdtime is 180 seconds. If a message isn't received from a neighbor for 180 seconds, the neighbor table is removed from the CDP neighbor table
	- CDPv2 messages are sent by default
	- `show cdp` command shows CDP information
	- `show cdp traffic` command shows how many CDP packets the device has sent and received
	- `show cdp interface` command gives basic CDP information about each interface
	- `show cdp neighbors` command shows the device's current CDP neighbor table
		- Device ID - Device's name
		- Local Interface - Interface on the current device
		- Holdtime - Remaining holdtime on the device
		- Capability - Lists the device's capability codes, describing what the device can do (R = Router, S = Switch)
		- Platform - Platform code for the device
		- Port ID - The neighboring interface for this device
	- ### Configuration
		- CDP is globally enabled by default
		- CDP is also enabled on each interface by default
		- `cdp run` command enables CDP globally
		- `cdp enable` command enables CDP on an interface
		- `cdp timer *seconds*` command sets the CDP timer frequency
		- `cdp holdtime *seconds*` command sets the CDP holdtime frequency
		- `cdp advertose-v2` command enables CDPv2
- ## LLDP
	- LLDP (Link Layer Discovery Protocol) is an industry standard protocol (IEEE 802.1AB)
	- It is usually disabled on Cisco devices by default, so it must be manually enabled
	- A device can run CDP and LLDP at the same time, but you will usually just choose one
	- LLDP messages are periodically sent to multicast MAC address 0180.C200.000E
	- When a device receives a CDP message, it processes and discards the message. It does NOT forward it to other devices
	- By default, LLDP messages are sent once every 30 seconds
	- By default, the LLDP holdtimer is 120 seconds
	- LLDP has an additional timer called the 'reinitialization delay'. If LLDP is enabled (globally or on an interface), this timer will delay the actual initialization of LLDP, 2 seconds by default
	- `show lldp` command shows LLDP information
	- `show lldp traffic` command shows how many LLDP packets the device has sent and received
	- `show lldp interface` command gives basic LLDP information about each interface, including current TX and RX state
	- `show lldp neighbors` command shows the device's current LLDP neighbor table
		- Device ID - Device's name
		- Local Interface - Interface on the current device
		- Holdtime - Remaining holdtime on the device
		- Capability - Lists the device's capability codes, describing what the device can do (R = Router, B = Switch (Bridge))
		- Port ID - The neighboring interface for this device
	- ### Configuration
		- LLDP is usually globally disabled by default
		- LLDP is disabled on each interface by default
		- To enable LLDP, you need to enable it globally, and then enable it on each interface
		- `lldp run` command enables LLDP globally
		- `lldp transmit` command enables LLDP TX on a specific interface
		- `lldp lldp receive` command enables LLDP RX on a specific interface
		- `lldp timer *seconds*` command sets the LLDP timer frequency
		- `lldp holdtime *seconds*` command sets the LLDP holdtime frequency
		- `lldp reinit *seconds*` command sets the LLDP reinitialization delay