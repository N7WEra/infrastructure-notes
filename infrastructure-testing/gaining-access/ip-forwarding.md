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

