---
description: Useful tools to calculate subnets and ranges
---

# IP Calculation

The math behind IP addresses is convoluted. Our nice IPv4 addresses start out as 32-bit binary numbers, which are then converted to base 10 numbers in four 8-bit fields. Decimal numbers are easier to manage than long binary strings; still, calculating address ranges, netmasks, and subnets is a bit difficult and error-prone, except for the brainiacs who can do binary conversions in their heads. For the rest of us, meet `ipcalc` and `ipv6calc`.

`ipcalc` is for IPv4 networks, and `ipv6calc` is for IPv6 networks.

### Calculate using subnet 

```text
root@Kali:~# ipcalc 192.168.0.0/25 
Address:   192.168.0.0          11000000.10101000.00000000.0 0000000 
Netmask:   255.255.255.128 = 25 11111111.11111111.11111111.1 0000000 
Wildcard:  0.0.0.127            00000000.00000000.00000000.0 1111111 
=> 
Network:   192.168.0.0/25       11000000.10101000.00000000.0 0000000 
HostMin:   192.168.0.1          11000000.10101000.00000000.0 0000001 
HostMax:   192.168.0.126        11000000.10101000.00000000.0 1111110 
Broadcast: 192.168.0.127        11000000.10101000.00000000.0 1111111 
Hosts/Net: 126                   Class C, Private Internet 
```

### Calculate using netmask 

```text
root@DESKTOP99:~# ipcalc 192.168.0.0 255.255.128.0 
Address:   192.168.0.0          11000000.10101000.0 0000000.00000000 
Netmask:   255.255.128.0 = 17   11111111.11111111.1 0000000.00000000 
Wildcard:  0.0.127.255          00000000.00000000.0 1111111.11111111 
=> 
Network:   192.168.0.0/17       11000000.10101000.0 0000000.00000000 
HostMin:   192.168.0.1          11000000.10101000.0 0000000.00000001 
HostMax:   192.168.127.254      11000000.10101000.0 1111111.11111110 
Broadcast: 192.168.127.255      11000000.10101000.0 1111111. 
```

### Classful IP Ranges 

E.g Class A,B,C \(depreciated\) 

| Class  | IP Address Range  |
| :--- | :--- |
| Class A IP Address Range  | 0.0.0.0 - 127.255.255.255  |
| Class B IP Address Range  | 128.0.0.0 - 191.255.255.255  |
| Class C IP Address Range  | 192.0.0.0 - 223.255.255.255  |
| Class D IP Address Range  | 224.0.0.0 - 239.255.255.255  |
| Class E IP Address Range  | 240.0.0.0 - 255.255.255.255  |

### IPv4 Private Address Ranges 

| Class  |  Range  |
| :--- | :--- |
| Class A Private Address Range  | 10.0.0.0 - 10.255.255.255  |
| Class B Private Address Range  | 172.16.0.0 - 172.31.255.255  |
| Class C Private Address Range  | 192.168.0.0 - 192.168.255.255  |
| Localhost Range  | 127.0.0.0 - 127.255.255.255  |

### IPv4 Subnet Cheat Sheet 

Subnet cheat sheet, not really related to pen testing but a useful reference. 

| CIDR  | Decimal Mask  | Number of Hosts  |
| :--- | :--- | :--- |
| /31  | 255.255.255.254  | 1 Host  |
| /30  | 255.255.255.252  | 2 Hosts  |
| /29  | 255.255.255.249  | 6 Hosts  |
| /28  | 255.255.255.240  | 14 Hosts  |
| /27  | 255.255.255.224  | 30 Hosts  |
| /26  | 255.255.255.192  | 62 Hosts  |
| /25  | 255.255.255.128  | 126 Hosts  |
| /24  | 255.255.255.0  | 254 Hosts  |
| /23  | 255.255.254.0  | 512 Host  |
| /22  | 255.255.252.0  | 1022 Hosts  |
| /21  | 255.255.248.0  | 2046 Hosts  |
| /20  | 255.255.240.0  | 4094 Hosts  |
| /19  | 255.255.224.0  | 8190 Hosts  |
| /18  | 255.255.192.0  | 16382 Hosts  |
| /17  | 255.255.128.0  | 32766 Hosts  |
| /16  | 255.255.0.0  | 65534 Hosts  |
| /15  | 255.254.0.0  | 131070 Hosts  |
| /14  | 255.252.0.0  | 262142 Hosts  |
| /13  | 255.248.0.0  | 524286 Hosts  |
| /12  | 255.240.0.0  | 1048674 Hosts  |
| /11  | 255.224.0.0  | 2097150 Hosts  |
| /10  | 255.192.0.0  | 4194302 Hosts  |
| /9  | 255.128.0.0  | 8388606 Hosts  |
| /8  | 255.0.0.0  | 16777214 Hosts  |

