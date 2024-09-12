---
tags:
  - work
  - ccna
---
- Networking models categorize and provide a structure for networking protocols and standards
	- **Protocol** - A set of rules defining how network devices and software should work
- ## OSI - 'Open Systems Interconnection' model
	- A conceptual model that categorizes and standardized the different functions in a network
	- Created by the 'International Organization for Standardization' (ISO)
	- Network engineers don't usually work with the top 3 layers, Application developers work with the top layers to connect their applications over networks
	- **A**ll **P**eople **S**eem **T**o **N**eed **D**ata **P**rocessing

| Level |              |
| ----- | ------------ |
| 7     | Application  |
| 6     | Presentation |
| 5     | Session      |
| 4     | Transport    |
| 3     | Network      |
| 2     | Data Link    |
| 1     | Physical     |
- ### Application
	- This layer is closest to the end user
	- Interacts with software applications, like your web browser
	- HTTP/HTTPS are Layer 7 protocols
	- Functions:
		- Identifying communication partners
		- Synchronizing communication
- ### Presentation
	- Data in the application layer is in 'application format' and needs to be translated to a different format to be sent over the network
	- The presentation layer translates between application and network formats
	- Encryption of data as it is sent, and decryption of data as it is received, are examples of functions.
	- Also translates between different application-layer formats
- ### Session
	- Controls dialogues (sessions) between communicating hosts
	- Establishes, manages, and terminates connections between the local application and the remote application
- ### Transport
	- Segments and reassembles data for communication between end hosts
	- Breaks large pieces of data in to smaller <u>segments</u> which can be more easily sent over the network and are less likely to cause transmission problems if errors occur.
	- Provides host-to-host communication
	- (Adds a 'Layer 4 Header', at this point, all the data is called a segment)
- ### Network
	- Provides connectivity between end hosts on different networks (ie. outside the LAN)
	- Provides logical addressing (IP Addresses)
	- Provides path selection between source and destination
	- Routers operate at Layer 3
	- (Adds a 'Layer 3 Header', at this point, all the data is called a packet)
- ### Data Link
	- Provides note-to-node connectivity and data transfer
		- PC to switch, switch to router, router to router
	- Detects and corrects Physical Layer errors
	- Uses Layer 2 addressing, separate from Layer 3 addressing
	- Switches operate at Layer 2
	-  (Adds a 'Layer 2 Trailer' and 'Layer 2 Header', at this point, all the data is called a frame)
- ### Physical
	- Defines physical characteristics of the medium used to transfer data between devices
	- For example, voltage levels, maximum transmission, distances, physical connectors, cable specifications, etc.
	- Digital bits are converted into electrical (for wired connections) or radio (for wireless connections) signals
	- All the information in Day 2's video (cables, pin layouts, etc.) is related to the Physical Layer

- **Protocol Data Units (PDUs)** - Data, Segments, Packets, Frames

- ## TCP/IP Suite - Conceptual model and set of communications protocols used on the internet and other networks
	- Developed by the United States Department of Defense through DARPA (Defense Advanced Research Projects Agency)
	- Similar structure to the OSI Model, but with fewer layers
	- This is the model actually in use in modern networks

| Level |             |
| ----- | ----------- |
| 4     | Application |
| 3     | Transport   |
| 2     | Internet    |
| 1     | Link        |

| OCI     | TCP/IP Equivalent |
| ------- | ----------------- |
| 7, 6, 5 | 4                 |
| 4       | 3                 |
| 3       | 2                 |
| 2, 1    | 1                 |
 ![[Pasted image 20240128013322.png]]
