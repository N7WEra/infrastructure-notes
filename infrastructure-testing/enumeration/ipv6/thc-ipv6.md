---
description: >-
  THC-IPV6-ATTACK-TOOLKIT  (c) 2005-2020 vh@thc.org
  https://github.com/vanhauser-thc/thc-ipv6
---

# THC IPv6

#### The THC IPV6 ATTACK TOOLKIT comes already with lots of effective attacking tools:

* parasite6: ICMPv6 neighbor solitication/advertisement spoofer, puts you

  as man-in-the-middle, same as ARP mitm \(and parasite\)

* alive6: an effective alive scanng, which will detect all systems

  listening to this address

* dnsdict6: parallized DNS IPv6 dictionary bruteforcer
* fake\_router6: announce yourself as a router on the network, with the

  highest priority

* redir6: redirect traffic to you intelligently \(man-in-the-middle\) with

  a clever ICMPv6 redirect spoofer

* toobig6: mtu decreaser with the same intelligence as redir6
* detect-new-ip6: detect new IPv6 devices which join the network, you can

  run a script to automatically scan these systems etc.

* dos-new-ip6: detect new IPv6 devices and tell them that their chosen IP

  collides on the network \(DOS\).

* trace6: very fast traceroute6 with supports ICMP6 echo request and TCP-SYN
* flood\_router6: flood a target with random router advertisements
* flood\_advertise6: flood a target with random neighbor advertisements
* fuzz\_ip6: fuzzer for IPv6 
* implementation6: performs various implementation checks on IPv6 
* implementation6d: listen daemon for implementation6 to check behind a FW
* fake\_mld6: announce yourself in a multicast group of your choice on the net
* fake\_mld26: same but for MLDv2
* fake\_mldrouter6: fake MLD router messages
* fake\_mipv6: steal a mobile IP to yours if IPSEC is not needed for authentication
* fake\_advertiser6: announce yourself on the network
* smurf6: local smurfer
* rsmurf6: remote smurfer, known to work only against linux at the moment
* exploit6: known IPv6 vulnerabilities to test against a target
* denial6: a collection of denial-of-service tests againsts a target
* thcping6: sends a hand crafted ping6 packet
* sendpees6: a tool by willdamn@gmail.com, which generates a neighbor

  solicitation requests with a lot of CGAs \(crypto stuff ;-\) to keep the

  CPU busy. nice.

  and about 25 more tools for you to discover :-\)

### Discover Hosts: 

```text
root@kali:~# atk6-alive6 eth0 
Alive: 2a02:c7f:9e17:fc00:20c:29ff:fe89:235a [ICMP echo-reply] 
Alive: 2a02:c7f:9e17:fc00:20c:29ff:feb2:a4bd [ICMP echo-reply] 

Scanned 1 address and found 2 systems alive 
```

### Discover routers

`root@kali:~# dump_router6` 

