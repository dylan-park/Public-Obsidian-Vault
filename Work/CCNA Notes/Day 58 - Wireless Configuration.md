---
tags:
  - work
  - ccna
---
- ## Switch Configuration
	1. [[Day 16 - VLANs (Part 1)#^ccna-configure-vlan|Create VLANs]] for the different WLANs and Management
		- For example: Management, Internal, and Guest VLANs
	2. Configure the switch ports that are connected to the APs (and any ports you wish to use for management) as [[Day 16 - VLANs (Part 1)#^ccna-configure-vlan|access ports]] on the management VLAN, and enable [[Day 21 - Spanning Tree Protocol (Part 2)#^ccna-portfast|Portfast]]
	3. Configure the switch ports connected to the WLC as a [[Day 23 - EtherChannel#^ccna-etherchannel-mode|static LAG]]
		- `channel-group 1 mode on`
	4. Configure the port channel as a trunk
		- `interface port-channel 1`
		- `switchport mode trunk`
		- `switchport trunk allowed vlan *vlan-ids*`
	5. Configure an [[Day 18 - VLANs (Part 3)#^ccna-svi|SVI]] for each VLAN
		- Give each VLAN an IP address
	6. Configure a [[Day 39 - DHCP#^ccna-dhcp-server-config|DHCP Pool]] for each VLAN
		- configure a default-router for all, and enable option 43 on the management VLAN (with the IP to the WLC)
	7. `ntp master` command makes the device an NTP server
- ## WLC Initial Setup
	- Upon first boot, the WLC will initialize a wizard to facilitate setup
		- Terminate Auto Install
		- Configure a:
			- System Name
			- Username
			- Password
		- Enable LAG
		- Configure Management Interface:
			- IP address
			- Netmask
			- Default Gateway
			- VLAN
			- DHCP Server
		- Configure a:
			- Virtual Gateway IP
			- Multicast IP
			- Mobility/RF Group Name
		- Configure a:
			- SSID
			- Disable DHCP Bridging
			- Allow Static IPs
			- Do not configure a RADIUS Server
		- Enter Country Code
		- Enable all 802.11 and Auto-RF
		- Configure an NTP server
		- Save Configuration
- ## WLC Ports/Interfaces
	- WLC **ports** are the physical ports that cables connect to
	- WLC **interfaces** are the logical interfaces within the WLC (i.e. SVIs on a switch)
	- WLC **Ports**:
		- **Service Port**: A dedicated management port. Used for out-of-band management. Must connect to a switch access port because it only supports one VLAN. This port can be used to connect to the device while it is booting, perform system recovery, etc.
		- **Distribution System Port**: These are the standard network ports that connect to the 'distribution system' (wired network) and are used for data traffic. These ports usually connect to switch trunk ports, and if multiple distribution ports are used they can form a LAG
		- **Console Port**: This is a standard console port, either RJ45 or USB
		- **Redundancy Port**: This port is used to connect to another WLC to form a high availability (HA) pair
	- WLC **Interfaces**:
		- **Management Interface**: Used for management traffic such as Telnet, [[Day 42 - SSH#^ccna-ssh|SSH]], HTTP, HTTPS, RADIUS authentication, NTP, Syslog, etc. CAPWAP tunnels are also formed to/from the WLC's management interface
		- **Redundancy Management interface**: When two WLCs are connected by their redundancy ports, one WLC is 'active' and the other is 'standby'. This interface can be used to connect to and manage the 'standby' WLC
		- **Virtual Interface**: This interface is used when communicating with wireless clients to relay DHCP requests, perform client web authentication, etc.
		- **Service Port Interface**: If the service port is used, this interface is bound to it and used for out-of-band management
		- **Dynamic Interface**: These are the interfaces used to map a WLAN to a VLAN. For example, traffic from the 'Internal' WLAN will be sent to the wired network from the WLC's 'Internal' dynamic interface
- ## WLC Configuration
	- ### Interface
		- Configure an Interface for each VLAN (Internal, Guest)
	- ### WLANs
		- Configure the Internal WLAN to the Internal Interface
		- Configure security to WPA+WPA2
		- Select PSK Authentication Method
		- Use ASCII for the PSK format and configure password
		- Configure QoS if necessary
			- Settings:
				- Platinum (Voice)
				- Gold (Video)
				- Silver (Best Effort) (Default)
				- Bronze (Background)
		- Create a new Guest WLAN
			- Enable
			- Change to Guest Interface
			- Configure security as above