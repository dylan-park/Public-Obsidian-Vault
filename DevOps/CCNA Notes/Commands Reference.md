---
tags:
  - devops
  - ccna
---
## Configuration Modes

| Prefix           | Mode                    |
| ---------------- | ----------------------- |
| >                | User EXEC               |
| #                | Privileged EXEC         |
| (config)#        | Global Configuration    |
| (config-if)#     | Interface Configuration |
| (config-router)# | Router Configuration    |

## User EXEC
- Router/Switch> `enable`
	- Enters Privileged EXEC mode
- Router/Switch> `exit`/`logout`
	- Exits User EXEC mode
- Router/Switch> `ping`
	- Send echo messages
- Router/Switch> `traceroute`
	- Trace route to a destination

## Privileged EXEC
- Router/Switch# `configure terminal`
	- Enters Global Configuration mode
- Router/Switch# `show running-config`
	- 
- Router/Switch# `write`/`write memory`/`copy running-config startup-config`
	- Saves running configuration to startup configuration
-  Router/Switch# `show ip interface brief`
	- Lists all interfaces, and gives IP information
- Router/Switch# `show interfaces description`
	- Shows interfaces w/ descriptions

## Global Configuration
- Router/Switch(config)#  `interface *interface-id*`
	- Enters Interface Configuration mode
- Router/Switch(config)# `do *command*`
	- Runs the command in EXEC mode
- Router/Switch(config)# `no *command*`
	- Removes the configuration set by the command
- Router/Switch(config)# `enable password *password*`
	- Enables a password for Global Configuration mode
- Router/Switch(config)# `enable secret *secret*`
	- Enables a password in configs with MD5 encryption (denoted with 5 in the config)
- Router/Switch(config)# `service password-encryption`
	- Encrypts all passwords in configs with Cisco's encryption (denoted with 7 in the config)
		- Enabling:
			- Current passwords will be encrypted
			- Future passwords will be encrypted
			- The enable secret will not be effected
		- Disabling:
			- Current passwords will not be decrypted
			- Future passwords will not be encrypted
			- The enable secret will not be effected

## Interface Configuration
- Router/Switch(config-if)# `ip address *address* *subnet-mask*`
	- Sets the IP address for the interface
- Router/Switch(config-if)# `no shutdown`
	- Enables the interface
- Router/Switch(config-if)#  `description *text*`
	- Sets the interface's description