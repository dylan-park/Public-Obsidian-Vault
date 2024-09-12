---
tags:
  - devops
  - work
---
- ## Syslog Overview
	- Syslog is an industry standard protocol for message logging
	- On network devices, Syslog can be used to log events such as:
		- Changes in interface status (up/down)
		- Changes in OSPF neighbor status (up/down)
		- System restarts
		- Etc.
	- The messages can be displayed in the CLI, saved in the device's RAM, or sent to an external Syslog server
	- Logs are essential when troubleshooting issues, examining the cause of incidents, etc.
	- Syslog and [[Day 40 - SNMP|SNMP]] are both used for monitoring and troubleshooting of devices, they are complementary, but their functionalities are different
	- ### Syslog Message Format
		- #### *seq:time stamp: %facility-severity-MNEMONIC:description*
			- **seq**: A sequence number indicating the order/sequence of messages
			- **time stamp**: A timestamp indicating the time the message was generated
			- **facility**: A value that indicates which process on the device generated the message
			- **severity**: A number that indicates the severity of logged event
			- **MNEMONIC**: A short code for the message, indicating what happened
			- **description**: Detailed information about the event being reported
- ## Syslog Severity Levels

| Level | Keyword         | Description                                         |
| ----- | --------------- | --------------------------------------------------- |
| 0     | **Emergency**   | System is unusable                                  |
| 1     | **Alert**       | Action must be taken immediately                    |
| 2     | **Critical**    | Critical conditions                                 |
| 3     | **Error**       | Error conditions                                    |
| 4     | **Warning**     | Warning conditions                                  |
| 5     | **Notice**      | Normal but significant condition (**Notification**) |
| 6     | **Information** | Informational messages                              |
| 7     | **Debugging**   | Debug-level messages                                |
**E**very **A**wesome **C**isco **E**ngineer **W**ill **N**eed **I**ce cream **D**aily
- ## Syslog Logging Locations
	- **Console Line**: Syslog messages will be displayed in the CLI when connected to the device via the console port. By default, all messages (level 0 - level 7) are displayed
	- **VTY lines**: Syslog messages will be displayed in the CLI when connected to the device via *[[Day 42 - SSH#^ccna-telnet|Telnet]]/[[Day 42 - SSH#^ccna-ssh|SSH]]*. Disabled by default
	- **Buffer**: Syslog messages will be saved to RAM. By default, all messages (level 0 - level 7) are displayed
		- You can view the messages with the `show logging` command
	- **External Server**: You can configure the device to send Syslog messages to an external server
		- Syslog servers will listen for messages on **UDP port 514**
- ## Syslog Configuration
	- `logging console *level*` command enables console line logging for the set severity level and higher
	- `logging monitor *level*` command enables VTY line logging for the set severity level and higher
		- Even if `logging monitor` is enabled, by default Syslog messages will not be displayed when connected via Telnet of SSH
		- `terminal monitor` command enables log output to Telnet and SSH
			- This command must be used **every time you connect to the device via Telnet or SSH**
	- `logging buffered *size-bytes* *level*` command enables logging to the buffer for the set severity level and higher
	- `logging *server-ip*` command enables remote logging to the specified server
		- `logging trap *level*` command sets the logging level for the external server at the set severity level and higher
	- By default, logging messages displayed in the CLI while you are in the middle of typing a command will result in the log message interrupting your input, continuing it after the message
		- `logging synchronous` command will cause a new line to be printed if your typing is interrupted by a message
	- `service timestamps log *datetime|uptime*` command enables timestamps in the logs
	- `service sequence-numbers` command enables sequence numbers in the logs
- ## Syslog vs SNMP
	- ### Syslog
		- Used for message logging
		- Events that occur within the system are categorized based on facility/severity and logged
		- Used for system management, analysis, and troubleshooting
		- Messages are sent from the devices to the server, the server **can't** actively pull information from the devices (like SNMP **Get**) or modify variables (like SNMP **Set**)
	- ### [[Day 40 - SNMP|SNMP]]
		- Used to retrieve and organize information about the SNMP managed devices
		- IP addresses, current interface status, temperature, CPU usage, etc.
		- SNMP servers can use **Get** to query the clients and **Set** to modify variables on the clients