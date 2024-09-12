---
tags:
  - work
  - ccna
---
![[Assets/Pasted image 20240210160014.png]]
- In order to find the next subnet address in a subnet, take the first one, and look at the last bit in the network portion. Add that many to the end of the address to get the next subnet.

- The process of subnetting Class A, B, and C networks is exactly the same

## Class B Subnetting

| Prefix Length | Netmask         | Number of usable addresses     | Possible Subnets (2^x) |
| ------------- | --------------- | ------------------------------ | ---------------------- |
| /17           | 255.255.128.0   | 32,766 (2^15-2)                | 2                      |
| /18           | 255.255.192.0   | 16,382 (2^14-2)                | 4                      |
| /19           | 255.255.224.0   | 8,190 (2^13-2)                 | 8                      |
| /20           | 255.255.240.0   | 4,094 (2^12-2)                 | 16                     |
| /21           | 255.255.248.0   | 2,046 (2^11-2)                 | 32                     |
| /22           | 255.255.252.0   | 1,022 (2^10-2)                 | 64                     |
| /23           | 255.255.254.0   | 510 (2^9-2)                    | 128                    |
| /24           | 255.255.255.0   | 254 (2^8-2)                    | 256                    |
| /25           | 255.255.255.128 | 126 (2^7-2)                    | 512                    |
| /26           | 255.255.255.192 | 62 (2^6-2)                     | 1,024                  |
| /27           | 255.255.255.224 | 30 (2^5-2)                     | 2,048                  |
| /28           | 255.255.255.240 | 14 (2^4-2)                     | 4,096                  |
| /29           | 255.255.255.248 | 6 (2^3-2)                      | 8,192                  |
| /30           | 255.255.255.252 | 2 (2^2-2)                      | 16,384                 |
| **/31**       | 255.255.255.254 | 2 (2^1) (No broadcast/network) | 32,768                 |
| **/32**       | 255.255.255.255 | 1 (No broadcast/network)       | 65,536                 |
