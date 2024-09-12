---
tags:
  - devops
  - ccna
---
### Spanning Tree Port States

| STP Port State | Stable/Transitional |
| -------------- | ------------------- |
| Blocking       | Stable              |
| Listening      | Transitional        |
| Learning       | Transitional        |
| Forwarding     | Stable              |
- Non-designated ports remain stable in a **Blocking** state
	- Are effectively disabled to prevent loops
	- Do net send/receive regular network traffic
	- Receive STP BPDUs
	- Do NOT forward STOP BPDUs
	- Do NOT learn MAC addresses
- **Listening** and **Learning** are transitional states which are passed through when an interface is activated, or when a **Blocking** port must transition to a Forwarding state due to a change in the network topology
	- After the Blocking state, interfaces with the Designated or Root role enter the Listening State
		- Only **Designated** or **Root** ports enter the Listening state
		- The listening state is 15 seconds long by default. This is determined by the **Forward delay** timer
		- ONLY forwards/receives STP BPDUs
		- Does NOT send/receive regular traffic
		- Does NOT learn MAC addresses from regular traffic that arrives on the interface
	- After the Listening state, interfaces with the Designated or Root role enter the Learning State
		-  The Learning state is 15 seconds long by default. This is determined by the **Forward delay** timer
		-  ONLY forwards/receives STP BPDUs
		- Does NOT send/receive regular traffic
		- Learns MAC addresses from regular traffic that arrives on the interface
- Root/Designated ports remain stable in a **Forwarding** state
	- A port in the Forwarding state operate as normal
	- Sends/Receives BPDUs
	- Sends/Receives regular traffic
	- Learns MAC addresses

| STP Port State | Send/Receive BPDUs | Frame forwarding (regular traffic) | MAC Address learning | Stable/Transitional |
| -------------- | ------------------ | ---------------------------------- | -------------------- | ------------------- |
| **Blocking**   | NO/YES             | NO                                 | NO                   | Stable              |
| **Listening**  | YES/YES            | NO                                 | NO                   | Transitional        |
| **Learning**   | YES/YES            | NO                                 | YES                  | Transitional        |
| **Forwarding** | YES/YES            | YES                                | YES                  | Stable              |
| **Disabled**   | NO/NO              | NO                                 | NO                   | Stable              |
### Spanning Tree Timers

| SPT Timer         | Purpose                                                                                                          | Duration            |
| ----------------- | ---------------------------------------------------------------------------------------------------------------- | ------------------- |
| **Hello**         | How often the root bridge sends hello BPDUs                                                                      | 2 sec               |
| **Forward delay** | How long the switch will stay in the Listening and Learning states (each state is 15 seconds = total 30 seconds) | 15 sec              |
| **Max Age**       | How long an interface will wait <u>after ceasing to receive Hello BPDUs</u> to change the STP topology           | 20 sec (10 * hello) |
- If another BPDU is received before the max age timer counts down to 0, the time will reset to 20 seconds and no changes will occur
- If another BPDU is not received, the max age timer counts down to 0 and the switch will reevaluate its STP choices, including root bridge, and local root, designated, and non-designated ports
- If a non-designated port is selected to become a designated or root port, it will transition from the blocking state to the listening state (15 seconds), learning state (15 seconds), and then finally the forwarding state. So, it can take a total of **50 seconds** for a blocking interface to transition to forwarding
- These timers and transitional states are to make sure that loops aren't accidentally created by an interface moving to forwarding state too soon


- **Portfast** - Puts a switch interface into forwarding mode immediately, skipping the listening and learning states ^ccna-portfast
	- Must ONLY be enabled on ports connected to end hosts
	- If enabled on a port connected to a switch it could cause a Layer 2 loop
	- `spanning-tree portfast` command in interface config sets portfast on the specific interface
		- Can only be done if the interface is not a trunk port
	- `spamming-tree portfast default` command will enable portfast on all access ports
- **BPDU Guard** - If an interface with BPDU Guard enabled receives a BPDU from another switch, the interface will be shut down to prevent a loop from forming
	- `spanning-tree bpduguard enable` command in interface config enables this feature
	- `spanning-tree bpdguard default` command enables BPDU Guard on all Portfast-enabled interfaces
- **Root Guard** - If you enable root guard on an interface, even if it receives a superior BPDU (lower bridge ID) on that interface, the switch will not accept the new switch as the root bridge. The interface will be disabled
- **Loop Guard** - If you enable loop guard on an interface, even if the interface stops receiving BPDUs, it will not start forwarding. The interface will be disabled
  
  
- `spanning-tree mode *mode*` command changes STP mode
- `spanning-tree vlan *vlan-id* root primary` command sets the root (primary) bridge
	- Does so by setting this switch's priority to 4098 less than the other switch's priority
- `spanning-tree vlan *vlan-id* root secondary` command sets the secondary bridge, in case of failure
- STP Load-Balancing - Distributing the traffic evenly across the devices, ensuring that ports that are blocked in one STP group (VLAN) are used by another STP group (VLAN) for forwarding
- `spanning-tree vlan *vlan-id* *setting*` command sets STP port settings
	- cost - Changes an interface's per VLAN spanning tree path cost
	- port-priority - Changes an interface's spanning tree port priority