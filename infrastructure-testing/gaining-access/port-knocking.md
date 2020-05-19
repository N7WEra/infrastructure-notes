---
description: >-
  Port Knocking is a well-established method used by both defenders and
  adversaries to hide open ports from access.
---

# Port knocking

To enable a port, an adversary sends a series of packets with certain characteristics before the port will be opened. Usually this series of packets consists of attempted connections to a predefined sequence of closed ports, but can involve unusual flags, specific strings or other unique characteristics. After the sequence is completed, opening a port is often accomplished by the host based firewall, but could also be implemented by custom software.

**Example:** 

`for x in 7000 8000 9000; do nmap -Pn --host_timeout 201 --max-retries 0 -p $x 10.10.10.10; done` 

OR 

`ports="40809 50212 46969"; for port in $ports; do echo "a" | nc -u -w 1 10.10.10.96 ${port}; sleep 0.5; done; echo "knock done"; nc -w 1 -nvv 10.10.10.96 22` 

