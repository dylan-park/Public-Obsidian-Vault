---
tags:
  - work
  - ccna
---
- ## Functions of Layer 4 (Transport Layer)
	- Provides transparent transfer of data between end hosts
	- Provides (or doesn't provide) various services to applications:
		- Reliable data transfer
		- Error recovery
		- Data sequencing
		- Flow control
	- Provides Layer 4 addressing (port numbers)
		- Identifies the Application Layer protocol
		- Provides session multiplexing
			- A session is an exchange of data between two or more communicating devices
			- Source Port:
				- Randomly selected by the client, helps identify the session
			- Destination Port:
				- Identifies the Application Layer Protocol
	- The following ranges have been designated by IANA (Internet Assigned Numbers Authority)
		- **Well-known** port numbers: 0-1023
		- **Registered** port numbers: 1024-49151
		- **Ephemeral/Private/Dynamic** port numbers: 49152-65535
- ## TCP (Transmission Control Protocol): ^ccna-tcp
	- TCP is connection-oriented
		- Before actually sending data to the destination host, the two hosts communicate to establish a connection. Once the connection is established, the data exchange begins
	- TCP provides reliable communication
		- The destination host must acknowledge that it received each TCP segment
		- If a segment isn't acknowledged, it is sent again
	- TCP provides sequencing
		- Sequence numbers in the TCP header allow destination hosts to put segments in the correct order even if they arrive out of order
	- TCP provides flow control
		- The destination host can tell the source host to increase/decrease the rate that data is sent
	- ![[Assets/Pasted image 20240312184023.png]]
	- ### TCP Three-Way Handshake (Establishing Connections): ^ccna-three-way-handshake
		- Client → Host: SYN flag
		- Client ← Host: SYN flag, ACK flag
		- Client → Host: ACK flag
	- ### TCP Four-Way Handshake (Terminating Connections):
		- Client → Host: FIN flag
		- Client ← Host: ACK flag
		- Client ← Host: FIN flag
		- Client → Host: ACK flag
	- ### TCP Sequencing/Acknowledgment
		- Hosts set a random initial sequence number
		- Forward acknowledgment is used to indicate the sequence number of the next segment the host expects to receive
	- ### TCP Flow Control
		- Acknowledging every single segment, no matter what size, is inefficient
		- The TCP header's **Window Size** field allows more data to be sent before an acknowledgment is required
		- A 'sliding window' can be used to dynamically adjust how large the window size is ^ccna-sliding-window
- ## UDP (User Datagram Protocol): ^ccna-udp
	- UDP is **not** connection-oriented
		- The sending host does not establish a connection with the destination host before sending data, the data is simply sent
	- UDP **does not** provide reliable communication
		- When UDP is used, acknowledgments are not sent for received segments
		- If a segment is lost, UDP has no mechanism to re-transmit it
		- Segments are sent 'best-effort'
	- UDP **does not** provide sequencing
		- There is no sequence number field in the UDP header
		- If segments arrive out of order, UDP has no mechanism to put them back in order
	- UDP **does not** provide flow control
		- UDP has no mechanism like TCP's window size to control the flow of data
	- ![[Assets/Pasted image 20240312185218.png]]
- ## Comparing TCP & UDP:
	- TCP provides more features than UDP, but at the cost of additional **overhead**
	- For applications that require reliable communications (like download a file), TCP is preferred
	- For applications like real-time voice and video, UDP is preferred
	- There are some applications that use UDP, but provide reliability etc. within the application itself
	- Some applications use both TCP & UDP, depending on the situation

| TCP                                    | UDP                             |
| -------------------------------------- | ------------------------------- |
| Connection-oriented                    | Connectionless                  |
| Reliable                               | Unreliable                      |
| Sequencing                             | No sequencing                   |
| Flow control                           | No flow control                 |
| Used for downloads, file sharing, etc. | Used for VoIP, live video, etc. |
- ## Important Port Numbers:

| TCP                                        | UDP                                           | TCP & UDP                  |
| ------------------------------------------ | --------------------------------------------- | -------------------------- |
| [[Day 43 - FTP & TFTP\|FTP]] data (20)     | [[Day 39 - DHCP\|DHCP]] server (67)           | [[Day 38 - DNS\|DNS]] (53) |
| [[Day 43 - FTP & TFTP\|FTP]] control (21)  | [[Day 39 - DHCP\|DHCP]] client (68)           |                            |
| [[Day 42 - SSH#^ccna-ssh\|SSH]] (22)       | [[Day 43 - FTP & TFTP#^ccna-tftp\|TFTP]] (69) |                            |
| [[Day 42 - SSH#^ccna-telnet\|Telnet]] (23) | [[Day 40 - SNMP\|SNMP]] agent (161)           |                            |
| SMTP (25)                                  | [[Day 40 - SNMP\|SNMP]] manager (162)         |                            |
| HTTP (80)                                  | [[Day 41 - Syslog\|Syslog]] (514)             |                            |
| POP3 (110)                                 |                                               |                            |
| HTTPS (443)                                |                                               |                            |
