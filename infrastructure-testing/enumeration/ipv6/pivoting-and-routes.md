---
description: >-
  Credit to Roxana Kovaci (https://twitter.com/RoxanaKovaci) and her SteelCon
  IPv6 workshop
---

# Pivoting and routes

### Adding routes

**Using IP:** 

`# /sbin/ip -6 route add <ipv6network>/<prefixlength> dev <device>` 

Example: 

`# /sbin/ip -6 route add default dev eth0 metric 1` 

or

`# /sbin/ip -6 route add <ipv6> via 2001:0db8:0:f101::1` 

Metric ”1” is used here to be compatible with the metric used by route, because the default metric on using ”ip” is ”1024”. 

\*\*\*\*

**Using "route"**: 

Usage: 

`# /sbin/route -A inet6 add <ipv6network>/<prefixlength> dev <device>` 

Example: 

`# /sbin/route -A inet6 add default dev eth0`  

### Removing routes

Removing an IPv6 route through an interface 

Not so often needed to use by hand, configuration scripts will use such on shutdown. 

**Using "ip"** 

Usage: 

`# /sbin/ip -6 route del <ipv6network>/<prefixlength> dev <device>`   
 Example: 

`# /sbin/ip -6 route del default dev eth0`  

**Using "route"** 

Usage: 

`# /sbin/route -A inet6 del <network>/<prefixlength> dev <device>` 

Example: 

`# /sbin/route -A inet6 del default dev eth0`

### Port-forwarding from IPv6 -&gt; IPv4 

socat port-forwarding 

`socat TCP4-LISTEN:8080,reuseaddr,fork TCP6:[fe80::20c:29ff:fe69:c4e5%eth0]:80` 

- you can then browse to 127.0.0.1:8080 and reach the IPv6 host on port 80 

### SSH local port-forwarding 

`ssh -6 user@fe80::cdf3:42e1:63d8:5227 -L 80:[fe80::20c:29ff:fe69:c4e5%ens33]:80` 

- After this, connecting to \[::1\]:80 will actually connect to the service on fe80::20c:29ff:fe69:c4e5 on port 80 dynamic port-forwarding 

`ssh -6 -D 9010 user@fe80::cdf3:42e1:63d8:5227`  

- change your proxychains.conf file to point to socks5 ::1 9010 

After this, prefix all your commands with proxychains: 

`./proxychains4 -f src/proxychains.conf nmap -sT -p21,80,445,1433,3389 -n -Pn fe80::3c0c:8c8f:6abd:93ae%ens33` 

quick port scanner through proxychains 

`while read -r line; do timeout 0.2s ./proxychains4 -f src/proxychains.conf ncat -6 -w1 -z fe80::cdf3:42e1:63d8:5227%ens33 $line 2>&1 | grep OK;done < ports.txt` 

where ports.txt has port numbers line by line  

Accessing RDP through the proxychains: 

`./proxychains4 -f src/proxychains.conf rdesktop fe80::cdf3:42e1:63d8:5227%ens33`  

