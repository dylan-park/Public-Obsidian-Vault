---
tags:
  - work
  - ccna
---
- ## Simple Network Management Protocol (SNMP)
	- SNMP is an industry-standard framework and protocol that was originally released in 1988
	- SNMPv1 was described as:
		- RFC 1065 - Structure and identification of management information for TCP/IP-based Internets
		- RFC 1066 - Management information base for network management of TCP/IP-based Internets
		- RFC 1067 - A simple network management protocol
	- SNMP can be used to monitor the status of devices, make configuration changes, etc.
	- There are two types of devices in SNMP:
		1. Managed Devices
			- There are the devices being managed using SNMP
			- For example, network devices like routers and switches
		2. Network Management Station (NMS)
			- The device/devices managing the managed devices
			- This is the SNMP 'server'
	- There are three main operations used in SNMP
		1. Managed devices can notify the NMS of events
			- Ex. Alert that interfaces have gone up or down
		2. The NMS can ask the managed devices for information about their current status
			- Ex. Check current CPU usage
		3. The NMS can tell the managed devices to change aspects of their configuration
			- Ex. Change the IP address of an interface
	- SNMP Agent = UDP port 161
	- SNMP Manager = UDP port 162
	- ### SNMP Components
		- #### NMS
			- The **SNMP Manager** is the software on the NMS that interacts with the managed devices
			- The **SNMP Application** provides an interface for the network admin to interact with
				- Displays alerts, statistics, charts, etc.
		- #### Managed Device
			- The **SNMP Agent** is the SNMP software running on the managed devices that interact with the SNMP Manager on the NMS
				- It sends notifications to/receives messages from the NMS
			- The **Management Information Base (MIB)** is the structure that contains the variables that are managed by SNMP
				- Each variable is identified with an Object ID (OID)
				- Example variables: Interface status, traffic throughput, CPU usage, temperature, etc.
	- ### SNMP OIDs
		- SNMP Object IDs are organized in a hierarchical structure
		- ![[Assets/Pasted image 20240411155804.png]]
- ## SNMP Versions
	- Many versions of SNMP have been proposed/developed, however only three major versions have achieved wide-spread use:
		- **SNMPv1**
			- The original version of SNMP
		- **SNMPv2c**
			- Allows the NMS to retrieve large amounts of information in a single request, so it is more efficient
			- 'c' refers to the 'community strings' used as passwords in SNMPv1, removed from SNMPv2, and then added back fir SNMPv2c
		- **SNMPv3**
			- A much more secure version of SNMP that supports strong **encryption** and **authentication**. Whenever possible, this version should be used
- ## SNMP Messages
	- ### Read
		- #### Get
			- A request sent from the manager to the agent to retrieve the value of a variable (OID), or multiple variables. The agent will send a *Response* message with the current value of each variable
		- #### GetNext
			- A request sent from the manager to the agent to discover the available variables in the MIB
		- #### GetBulk
			- A more efficient version of the **GetNext** message (introduced in SNMPv2)
	- ### Write
		- #### Set
			- A request from the manager to the agent to change the value of one or more variables. The agent will send a *Response* message with the new values
	- ### Notification
		- #### Trap
			- A notification sent from the agent to the manager. The manager does not send a Response message to acknowledge that it received the Trap, so their messages are 'unreliable'
		- #### Inform
			- A notification message that is acknowledged with a *Response* message

| Message Class | Description                                                                                                              | Messages                    |
| ------------- | ------------------------------------------------------------------------------------------------------------------------ | --------------------------- |
| Read          | Messages sent by the **NMS** to read information from the **managed devices**<br>(i.e. What's your current CPU usage %?) | *Get<br>GetNext<br>GetBulk* |
| Write         | Massages sent by the **NMS** to change information on the **managed devices**<br>(i.e. Change an IP address)             | *Set*                       |
| Notification  | Messages sent by the **managed devices** to alert the NMS of a particular event<br>(i.e. Interface going down)           | *Trap<br>Inform*            |
| Response      | Messages sent in response to a previous message/request                                                                  | *Response*                  |
- ## Configuration
	- `snmp-server contact *contact-info*` and `snmp-server location *location-info*` commands specify optional information
	- Set community strings:
		- `snmp-server community *string* ro` command configures a read only community string (password), no Set messages
		- `snmp-server community *string* rw` command configures a read/write community string (password), can use Set messages
		- Default Strings:
			- ro = public
			- rw = private
	- `snmp-server host *ip-address** version *version* *community-string*` command specifies the NMS, session, and community string for this client
	- Configure Trap types:
		- `snmp-server enable  traps snmp linkdown linkup` command sends traps when links go up or down
		- `snmp-server enable traps config` command sends traps when configurations are made