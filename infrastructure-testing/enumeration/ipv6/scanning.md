---
description: Find your IPv6 and find other hosts
---

# Scanning

### Identifying your own address over IPv6 

`ip -6 addr show dev eth0` 

OR 

`ifconfig eth0 | grep "inet6"` 

### Discovering other hosts

#### **Discover router advertisement**

**Use metasploit:**

`auxiliary/scanner/discovery/ipv6_neighbor_router_advertisement`

**Use linux utility:**

`sudo radvdump`

**Using atk6-dump\_router6:**

`atk6-dump_router6 eth0`

**Wireshark Router Solicitation:**

\``icmpv6.type==133`

**Wireshark Router** **advertisement:**

`icmpv6.type==134`

**tcpdump router advertisement**

`sudo tcpdump -vvvv -ttt -i eth1 icmp6 and 'ip6[40] = 134'`

```text
15:43:49.484751 fe80::212:34ff:fe12:3450 > ff02::1: icmp6: router
� advertisement(chlim=64, router_ltime=30, reachable_time=0,
� retrans_time=0)(prefix info: AR valid_ltime=30, preffered_ltime=20,
� prefix=2002:0102:0304:1::/64)(prefix info: LAR valid_ltime=2592000,
� preffered_ltime=604800, prefix=2001:0db8:0:1::/64)(src lladdr:
� 0:12:34:12:34:50) (len 88, hlim 255)
Discover local hosts using IPv6
```

Router with link-local address "fe80::212:34ff:fe12:3450" send an advertisement to the all-node-on-link multicast address "ff02::1" containing two prefixes "2002:0102:0304:1::/64" \(lifetime 30 s\) and "2001:0db8:0:1::/64" \(lifetime 2592000 s\) including its own layer 2 MAC address "0:12:34:12:34:50".

**Using ping6**:

```text
root@kali:~/Downloads# ping6 -I wlan0 -c 2 ip6-allnodes
ping6: Warning: source address might be selected on device other than wlan0. 
PING ff02::01(ff02::1) from :: wlan0: 56 data bytes 
64 bytes from fe80::3828:a4e:f629:cc7f%wlan0: icmp_seq=1 ttl=64 time=0.071 ms 
64 bytes from fe80::a2bd:cdff:fe37:8881%wlan0: icmp_seq=1 ttl=64 time=79.8 ms (DUP!) 
64 bytes from fe80::cd5:6ea0:cd5c:b11d%wlan0: icmp_seq=1 ttl=64 time=85.3 ms (DUP!) 
64 bytes from fe80::f87e:295f:1aed:1069%wlan0: icmp_seq=1 ttl=64 time=88.8 ms (DUP!) 
64 bytes from fe80::3828:a4e:f629:cc7f%wlan0: icmp_seq=2 ttl=64 time=0.118 ms  

--- ff02::01 ping statistics --- 
2 packets transmitted, 2 received, +3 duplicates, 0% packet loss, time 2ms 
rtt min/avg/max/mdev = 0.071/50.801/88.773/41.500 ms 
```

Note: `ip6-allnodes` is a alias to `ff02:01`

Or you can use on of the following options:

```text
::1     ip6-localhost ip6-loopback
fe00::0 ip6-localnet
ff00::0 ip6-mcastprefix
ff02::1 ip6-allnodes
ff02::2 ip6-allrouters
```

**Using ip:**

```text
root@kali:~/Downloads# ip -6 neigh 
fe80::f87e:295f:1aed:1069 dev wlan0 lladdr 08:c5:e1:9b:fb:5f STALE 
fd9c:1db3:7210:0:a2bd:cdff:fe37:8880 dev wlan0 lladdr a0:bd:cd:37:88:81 router STALE 
fe80::cd5:6ea0:cd5c:b11d dev wlan0 lladdr 18:81:0e:57:8a:c9 STALE 
fe80::3828:a4e:f629:cc7f dev vmnet1 FAILED 
fe80::a2bd:cdff:fe37:8881 dev vmnet1 FAILED 
fe80::a2bd:cdff:fe37:8881 dev wlan0 lladdr a0:bd:cd:37:88:81 router REACHABLE  
```

**using alive6:**

```text
iron@kali:~/Documents$ sudo atk6-alive6 eth0
Alive: fe80::8079:79a8:439f:431a [ICMP parameter problem]

Scanned 1 address and found 1 system alive

```

**Using IPv6finder:**

Link: ****[**https://github.com/phillips321/phillips321/blob/master/ipv6finder.sh**](https://github.com/phillips321/phillips321/blob/master/ipv6finder.sh)\*\*\*\*

```text
root@UK198899:~# ./ipv6finder.sh eth0 
[+]Pinging (ff02::1) broadcast for nodes on link localping6: Warning: source address might be selected on device other than: eth0 
.Done 
[+]Pinging (ff02::2) broadcast for routersping6: Warning: source address might be selected on device other than: eth0 
.Done 
[+]Pinging (ff02::1) broadcast for nodes on Global Interface./ipv6finder.sh: line 41: [: 2a02:c7f:9e17:fc00:20c:29ff:fe65:dde1: binary operator expected 
Done 
[+]ArpScanning local IPv4.Done 
--------------------------------|---------------|--------------------|--------------------|------------- 
                IPV6 Link Local |   IPV6 Global |        MAC Address |        IPV4Address |         Info 
--------------------------------|---------------|--------------------|--------------------|------------- 
 fe80::20c:29ff:fe65:dde1%eth0: |      NotFound |  00:0c:29:65:dd:e1 |       192.168.0.43 |         You? 
 fe80::20c:29ff:feb2:a4bd%eth0: |      NotFound |  00:0c:29:b2:a4:bd |       192.168.0.30 |         Node 
--------------------------------|---------------|--------------------|--------------------|------------- 
```

\*\*\*\*

