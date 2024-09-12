---
tags:
  - work
  - ccna
---
- **Interfaces** = Ports
- **RJ** = Registered Jack (As in RJ-45)
- **Ethernet** - A collection of network protocols/standards.
- Speed is measured in bits per second, not bytes per second
	- 8 bits = 1 byte
	- 1 kilobit (Kb) = 1,000 bits
	- 1 megabit (Mb) = 1,000,000 bits
	- 1 gigabit (Gb) = 1,000,000,000 bits
	- 1 terabit (Tb) = 1,000,000,000,000 bits
- **IEEE** = Institute of Electrical and Electronics Engineers
- IEEE 802.3 standard, 1983

| Speed    | Common Name      | IEEE Standard | Informal Name | Maximum Length |
| -------- | ---------------- | ------------- | ------------- | -------------- |
| 10 Mbps  | Ethernet         | 802.3i        | 10BASE-T      | 100 m          |
| 100 Mbps | Fast Ethernet    | 802.3u        | 100BASE-T     | 100 m          |
| 1 Gbps   | Gigabit Ethernet | 802.3ab       | 1000BASE-T    | 100 m          |
| 10 Gbps  | 10 Gig Ethernet  | 802.3an       | 10GBASE-T     | 100 m          |
- **UTP Cables** = Unshielded Twisted Pair Cables
	- Twist protects against EMI (Electromagnetic Interference)
	- 10BASE-T & 100BASE-T = 2 pairs (4 wires)
	- 1000BASE-T & 10GBASE-T = 4 pairs (8 wires)
	- Full-Duplex - Separate wires are used for transmitting and receiving, to avoid collisions.
	![[Assets/Pasted image 20240125161144.png]]
	![[Assets/Pasted image 20240125161835.png]]
		- Each pair in 1000BASE-T and 10GBASE-T are bidirectional, and can be used to transmit and receive.
	- Routers/PCs transmit data on pins 1 and 2, and receive data on 3 and 6
	- Switches transmit data on pins 3 and 6, and receive data on 1 and 2
	- Straight-Through cable - Pin pairs are matched on both sides of the cable
	- Crossover cable - Pin pairs are reversed on the other side of the cable
	![[Assets/Pasted image 20240125161556.png]]
- Auto MDI-X - Allows devices to automatically detect which pins their partner is using to transmit, and adjusts what pins they then use to receive
![[Assets/Pasted image 20240125162011.png]]![[Assets/Pasted image 20240125162250.png]]
- ### Multimode Fiber Cable -
	- Core diameter is wider than single-mode fiber
	- Allows multiple angle (modes) of light waves to enter the core.
	- Allows longer cables than UTP, but shorter cables than single-mode fiber
	- Cheaper than single-mode fiber (due to cheaper LED-based SFP transmitters)
- ### Single-Mode Fiber Cable - 
	- Core diameter is narrower than multimode fiber
	- Light enters at a single angle (mode) from a laser-based transmitter
	- Allows longer cables than both UTP and multimode fiber
	- More expensive than multimode fiber (due to more expensive laser-based SFP transmitters)

| Informal Name | IEEE Standard | Speed  | Cable Type               | Maximum Length          |
| ------------- | ------------- | ------ | ------------------------ | ----------------------- |
| 1000BASE-LX   | 802.3z        | 1Gbps  | Multimode or Single-Mode | 550 m (MM)<br>5 km (SM) |
| 10GBASE-SR    | 802.3ae       | 10Gbps | Multimode                | 400 m                   |
| 10GBASE-LR    | 802.3ae       | 10Gbps | Single-Mode              | 10 km                   |
| 10GBASE-ER    | 802.3ae       | 10Gbps | Single-Mode              | 30 km                   |
- ## UTP vs Fiber-Optic Cabling:
	- ### UTP
		- Lower cost than fiber-optic
		- Shorter max distance than fiber-optic (~100m)
		- Can be vulnerable to EMI
		- RJ-45 ports used with UTP are cheaper than SFP ports
		- Emit a faint signal outside the cable, which can be copied (=security risk)
	- ### Fiber-Optic
		- Higher cost than UTP
		- Longer max distance than UTP
		- No vulnerability to EMI
		- SFP ports are more expensive than RJ-45 ports (single-mode is more expensive than multimode)
		- Does not emit and signal outside the cable (=no security risk)