---
tags:
  - devops
  - ccna
---
- **DTP (Dynamic Trunking Protocol)** - A Cisco proprietary protocol that allows Cisco switches to dynamically determine their interface status (access or trunk) without manual configuration
	- DTP is enabled by default on all Cisco switch interfaces
	- For security purposes, manual configuration is recommended. DTP should be disabled on all switchports
	- `switchport mode dynamic *auto/desirable*` command turns on DTP
		- A switchport in ***dynamic auto*** mode will NOT actively try to form a trunk with other Cisco switches, however it will form a trunk if the switch connected to it is actively trying to form a trunk. It will form a trunk with a switchport in the following modes:
			- switchport mode trunk
			- switchport mode dynamic desirable
		- A switchport in ***dynamic desirable*** mode will actively try to form a trunk with other Cisco switches, It will form a trunk if connected to another switchport in the following modes: 
			- switchport mode trunk
			- switchport mode dynamic desirable
			- switchport mode dynamic auto
	- DTP will not form a trunk with a router, PC, etc. The switchport will be in access mode
	- On older switches, ***switchport mode dynamic desirable*** is the default administrative mode
	- On newer switches, ***switchport mode dynamic auto*** is the default administrative mode
	- You can disable DTP negotiation on an interface with the command: `switchport nonegotiate`
	- Configuring an access port with ***switchport mode access*** also disables DTP negotiation on an interface
	- Switches that support both **802.1Q** and **ISL** trunk encapsulations can use DTP to negotiate the encapsulation they will use
		- This negotiation is enabled by default, as the default trunk encapsulation made is: ***switchport trunk encapsulation negotiate***
		- ISL is favored over 802.1Q, so if both switches support ISL it will be selected
		- DTP frames are sent in VLAN1 when using ISL, or in the native VLAN when using 902.1Q

| Administrative Mode   | Trunk | Dynamic Desirable | Access | Dynamic Auto |
| --------------------- | ----- | ----------------- | ------ | ------------ |
| **Trunk**             | Trunk | Trunk             | X      | Trunk        |
| **Dynamic Desirable** | Trunk | Trunk             | Access | Trunk        |
| **Access**            | X     | Access            | Access | Access       |
| **Dynamic Auto**      | Trunk | Trunk             | Access | Access       |

- **VTP (VLAN Trunking Protocol)** - Allows you to configure VLANs on a central VTP server switch, and other switches (VTP clients) will synchronize their VLAN database to the server
	- Designed for large networks with many VLANs, so that you don't have to configure each VLAN on every switch
	- It is rarely used, and it is recommended that you do not use it
	- There are three VTP versions 1, 2, and 3
	- There are three VTP modes: server, client, and transparent
	- `vtp mode *mode*` command sets VTP mode
	- Cisco switches operate in VTP server mode by default
		- ### Server: 
			- Can add/modify/delete VLANs
			- Store the VLAN database in non-volatile RAM (NVRAM)
			- Will increase the revision number every time a VLAN is added/modified/deleted
			- Will advertise the latest version of the VLAN database on trunk interfaces, and the VTP clients will synchronize their VLAN database to it
			- VTP servers also function as VTP clients
				- Therefore a VTP server will synchronize to another VTP server with a higher revision number
		- ### Client:
			- Cannot add/modify/delete VLANs
			- Do not store the VLAN database in NVRAM (in VTPv3 they do)
			- Will synchronize their VLAN database to the server with the highest revision number in their VTP domain
			- Will advertise their VLAN database, and forward VTP advertisements to other clients over their trunk ports
		- ### Transparent:
			- Does not participate in the VTP domain (does not sync its VLAN database)
			- Maintains its own VLAN database in NVRAM. It can add/modify/delete VLANs, but they won't be advertised to other switches
			- Will forward VTP advertisements that are in the same domain as it
	- `show vtp status` command lists VTP info for the current switch
	- VTPv1/v2 do not support the extended VLAN range (1006-4094). Only VTPv3 supports them
	- If a switch with no VTP domain (domain NULL) receives a VTP advertisement with a VTP domain name, it will automatically join that VTP domain
	- One danger of VTP: If you connect an old switch with a higher revision number to your network (and the VTP domain name matches), all switches in the domain will sync their VLAN database to that switch
	- Changing the VTP domain to an unused domain will reset the revision number to 0