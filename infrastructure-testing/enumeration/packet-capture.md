---
description: >-
  Packet capture is a computer networking term for intercepting a data packet
  that is crossing or moving over a specific computer network.
---

# Packet Capture

## Tcpdump

**Save a packet capture:** 

`tcpdump -i  -s0 -w pcapfile.pcap`

Ctrl-C to stop after about 5 minutes. 

Replay capture and look for interesting protocols:

`tcpdump -r pcapfile.pcap not  and not arp`

Tcpdump filter for DHCPv6:

`tcpdump -i eth0 -n -vv '(udp port 546 or 547) or icmp6`

## Wireshark

**Router solicitation filter**

When analyzing IPv6 traffic in Wireshark, you can simply use the filter `icmpv6.type==133` to show only "Router Solicitation" messages.

