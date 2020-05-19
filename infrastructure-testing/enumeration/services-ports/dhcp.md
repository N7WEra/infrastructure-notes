---
description: >-
  The Dynamic Host Configuration Protocol is a network management protocol used
  on Internet Protocol networks whereby a DHCP server dynamically assigns an IP
  address.
---

# 67 - DHCP

### Discovering DHCP Server

```text
root@kali:~# nmap --script broadcast-dhcp-discover 
Starting Nmap 7.70 ( 
https://nmap.org
 ) at 2019-04-10 15:19 BST 
Pre-scan script results: 
| broadcast-dhcp-discover:  
| Response 1 of 1:  
| IP Offered: 192.168.0.16 
| DHCP Message Type: DHCPOFFER 
| Server Identifier: 192.168.0.1 
| IP Address Lease Time: 1d00h00m00s 
| Subnet Mask: 255.255.255.0 
| Router: 192.168.0.1 
| Domain Name Server: 192.168.0.1 
|_ Domain Name: Home 
WARNING: No targets were specified, so 0 hosts scanned. 
Nmap done: 0 IP addresses (0 hosts up) scanned in 2.59 seconds 
```

**From command Line:**

`cat /var/lib/dhcp/dhclient.leases`

