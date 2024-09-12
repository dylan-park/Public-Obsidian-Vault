---
tags:
  - devops
  - ccna
---
![[Assets/Pasted image 20240207222314.png]]
- Each interface on a switch has a MAC address

- PC1 > PC4
	- ARP Request: PC1 > SW1 (Learns PC1 MAC) > R1. ARP Reply: R1 > SW1 (Learns R1 MAC) > PC1
	- PC1 encapsulates R1's mac address into Ethernet header
	- R1 removes header
	- R1 uses routing table to determine next hop (R2)
	- ARP Request R1 > R2. ARP Reply R2 > R1
	- R1 encapsulates R2's mac header into Ethernet header
	- R2 removes header
	- R2 uses routing table to determine next hop (R4)
	- ARP Request R2 > R4. ARP Reply R4 > R2
	- R2 encapsulates R4's mac header into Ethernet header
	- R2 uses routing table to determine next hop (Gi0/2)
	- ARP Request R4 > SW4 (Learns R4 MAC) > PC4. ARP Reply PC4 > SW4 (Learns PC4 MAC) > R2
	- R4 encapsulates PC4's mac header into Ethernet header