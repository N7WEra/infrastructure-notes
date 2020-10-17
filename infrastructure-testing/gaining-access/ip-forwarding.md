# IP Forwarding

## Info

Detects whether the remote device has ip forwarding or "Internet connection sharing" enabled, by sending an ICMP echo request to a given target using the scanned host as default gateway.

### Check status

```text
# sysctl net.ipv4.ip_forward
net.ipv4.ip_forward = 1
```

### enable  ip forwarding

```text
sysctl -w net.ipv4.ip_forward=1
```

### How to identify systems on the local LAN

Use your favourite ARP scanning to identify systems on the local LAN. Save the output \(I use to arp.txt in the example below\).

* For IPv4
  * `arp-scan -l`

    * `arp-scan -l`

    ```text
     arp-scan -l | tee arp_scan_macs.txt


     Interface: eth0, datalink type: EN10MB (Ethernet)
     Starting arp-scan 1.6 with 256 hosts (http://www.nta-monitor.com/tools/arp-scan/)
     10.0.0.100     00:13:72:09:ad:76       Dell Inc.
     10.0.0.200     00:90:27:43:c0:57       INTEL CORPORATION
     10.0.0.254     00:08:74:c0:40:ce       Dell Computer Corp.

     3 packets received by filter, 0 packets dropped by kernel
     Ending arp-scan 1.6: 256 hosts scanned in 2.099 seconds (121.96 hosts/sec).  3 responded
    ```

  * `arp`

    * `arp -a`

    ```text
     arp -a | tee arp_macs.txt

     (10.10.2.1) at 1f:2e:39:d7:2f:04 [ether] on eth0
     (10.10.2.3) at 1f:23:39:d8:2e:44 [ether] on eth0
    ```
* For IPv6
  * `ip` `ip -6 neighbor`

    ```text
     fe80::ca21:aabe:fdc6:d7c1 dev eth0 lladdr f9:42:64:d6:0a:d5 router STALE
    ```

## Nmap Usage

`sudo nmap -sn  --script ip-forwarding --script-args='target=www.example.com'`

Example:

```text
root@Kali:~# nmap -sn 192.168.0.30 --script ip-forwarding --script-args='target=192.168.73.130'
Starting Nmap 7.80 (https://nmap.org) at 2020-01-23 14:20 GMT
Nmap scan report for 192.168.0.30
Host is up (0.00097s latency).
MAC Address: 00:0C:29:B2:A4:BD (VMware)

Host script results:
| ip-forwarding:
|_ The host has ip forwarding enabled, tried ping against (192.168.73.130)
```

## Nessus

Look for 'IP Forwarding Enabled'

## Gateway Finder

[https://github.com/pentestmonkey/gateway-finder](https://github.com/pentestmonkey/gateway-finder)

Gateway-finder is a scapy script that will help you determine which of the systems on the local LAN has IP forwarding enabled and which can reach the Internet.

### Usage

Collet mac address of the hosts you want to check

`arp-scan -l | tee arp.txt`

Step 2: Run gateway-finder on the list of local systems

Gateway-finder needs two bits of input from you:

* The MAC addresses of the potential gateways
* The IP address of a system on the Internet \(I use a google.com address in the example below\):

If arp.txt also contains an IP of each system on the same line as the MAC, you'll get much nicer output. If you need to use a different network interfaces, use the -I option.

`python gateway-finder.py -f arp.txt -i 209.85.227.99`

```text
# python gateway-finder.py -f arp.txt -i 209.85.227.99
gateway-finder v1.0 http://pentestmonkey.net/tools/gateway-finder

[+] Using interface eth0 (-I to change)
[+] Found 3 MAC addresses in arp.txt
[+] 00:13:72:09:AD:76 [10.0.0.100] appears to route ICMP Ping packets to 209.85.227.99.  Received ICMP TTL Exceeded in transit response.
[+] 00:13:72:09:AD:76 [10.0.0.100] appears to route TCP packets 209.85.227.99:80.  Received ICMP TTL Exceeded in transit response.
[+] We can ping 209.85.227.99 via 00:13:72:09:AD:76 [10.0.0.100]
[+] We can reach TCP port 80 on 209.85.227.99 via 00:13:72:09:AD:76 [10.0.0.100]
[+] Done

```

## Gateway Finder imp

Link: [https://github.com/whitel1st/gateway-finder-imp](https://github.com/whitel1st/gateway-finder-imp)

Usage:



* `sudo python3 gateway-finder-imp.py`
  * `-h` - help
  * `-M <MAC>` - use file with next-hop MACs
  * `-m <file_with_MACs>` - use selected next-hop MAC
  * `-d <IP>` - use selected destination IPs
  * `-D <file_with_IPs>` - use file with selected destination IPs
  * `-i <interface_name>` - use selected network interface
  * `-p <port_1> <port_2> ... <port_n>` - use ports
  * `--v` - verbose mode
  * `--vv` - maximum verbosity
* examples
  * `gateway-finder-imp.py -d 8.8.8.8 -m de:ad:be:af:de:ad -i enp0s31f6` use selected next-hop MAC and selected destination IP
  * `gateway-finder-imp.py -D dst_hosts.txt -M next_hop_macs.txt -i wlp3s0` - use selected next-hop MAC and file with selected destination IPs
  * `gateway-finder-imp.py -d 8.8.8.8 -M next_hop_macs.txt -i eth0` - use file with next-hop MACs and file with selected destination IPs
  * `gateway-finder-imp.py -D file_with_dst_IPs.txt -M file_with_nex_hop_MACs.txt -i eth1 -p 22 443 80 8080 23`
  * `gateway-finder-imp.py -d 2a00:1450:4010:c05::64 -M mac_with_ipv6_0.txt -i wlp3s0 -p 443 80 -6 --vTries to find a layer-3 gateway to the Internet. Attempts to reach an IP`

