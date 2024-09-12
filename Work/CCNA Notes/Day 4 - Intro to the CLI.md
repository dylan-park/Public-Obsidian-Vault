---
tags:
  - work
  - ccna
---
- **CLI** - Command-line Interface
	- The interface you use to configure Cisco devices
- **Rollover cable** - Cable with an RJ45 end, and a serial port end, used to connect to a console port on a device
	- Pin 1 → Pin 8
	- Pin 2 → Pin 7
	- Pin 3 → Pin 6
	- Pin 4 → Pin 5
	- Pin 5 → Pin 4
	- Pin 6 → Pin 3
	- Pin 7 → Pin 2
	- Pin 8 → Pin 1
- Terminal Emulator - PuTTy
	- Serial settings (default for Cisco):
		- Speed (baud) - 9600
		- Data bits - 8
		- Stop bits - 1
		- Parity - None
		- Flow control - None
- User EXEC mode (user mode) = '>' in CLI
	- Very limited
	- Users can look at some things, but can't make any changes to the configuration
![[Assets/Pasted image 20240129183441.png]]
- Enter 'enable' to enter Privileged EXEC mode
- Privileged EXEC mode = '#' in CLI
	- Provides complete access to view the device's configuration, restart the device, etc.
	- Cannot change the configuration, but can change the time on the device, save the configuration file, etc.
![[Assets/Pasted image 20240129183728.png]]
- Use a question make (?) to view the commands available to you
- Enter `configure terminal` to enter Global Configuration mode
- Global Configuration mode = '(config)#' in CLI
- `enable password` enables a password for Global Configuration mode
	- Passwords are case-sensitive
- `enable secret` command will enable a password in configs with MD5 encryption (denoted with 5 in the config)
- `exit` logs out of current mode
- There are two separate configuration files kept on the device at once
	- **Running-config** - The current, active configuration file on the device. As you enter commands in the CLI, you edit the active configuration
	- **Startup-config** - the configuration file that will be loaded upon restart of the device
- `show running-config` will display current running config
- From privileged exec mode, write `write`, `write memory`, or `copy running-config startup-config` in order to save the configuration to the startup-config
- `service password-encryption` command will encrypt all passwords in configs with Cisco's encryption (denoted with 7 in the config)
	- Enabling:
		- Current passwords will be encrypted
		- Future passwords will be encrypted
		- The enable secret will not be effected
	- Disabling:
		- Current passwords will not be decrypted
		- Future passwords will not be encrypted
		- The enable secret will not be effected
- `do` is a shortcut to run User EXEC mode commands in
- entering `no` can cancel a command
![[Assets/Pasted image 20240129190510.png]]
![[Assets/Pasted image 20240129190527.png]]
![[Assets/Pasted image 20240129190546.png]]
![[Assets/Pasted image 20240129190611.png]]
![[Assets/Pasted image 20240129190622.png]]