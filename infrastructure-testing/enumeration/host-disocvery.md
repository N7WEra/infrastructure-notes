# Host Discovery

## IP

Local IP

```text
ip addr show dev tun0
```

ipv6

```text
ip -6 addr show dev tun0
```

Find local hosts:

`ip neigh`

IPv6 hosts:

`ip -6 neigh` 

#### How do I change the state of the device to UP or DOWN?

The syntax is as follows:  
`ip link set dev {DEVICE} {up|down}`

## Nmap

Useful parameters: 

| Parameter | Info |
| :--- | :--- |
| -sS  | Syn Scan  |
| -v | Verbose |
| -A  | OS, Scripts and service scan  |
| -p-  | Full port Scan  |
| -sU  | UDP Scan  |
| --script=smb-vuln-scan  | Run smb script  |
| --script-args=unsafe=1  | run the script with arguments  |
| -iL  | Scan from a target file  |
| --exclude  | Exclude listed hosts  |
| --excludefile  | Exclude file list  |
| -sL   | No scan list targets only  |
| -sn  | Disable port scanning, host discovery only  |
| -sV   | Attempts to determine the service version  |
| -T0 to -T5  | Scan speed, 0 the slowest and best at evasion, 5 insane speed scan  |
| --max-retries  | Maximum member of port scan retransmissions  |

## netdiscover

Discovers IP, MAC Address and MAC vendor on the subnet from ARP, helpful for confirming you're on the right VLAN at client site

`netdiscover -r 192.168.1.0/24` 

**Results:** 

```text
 Currently scanning: Finished!   |   Screen View: Unique Hosts                  
 4 Captured ARP Req/Rep packets, from 4 hosts.   Total size: 240                
 _____________________________________________________________________________ 
   IP            At MAC Address     Count     Len  MAC Vendor / Hostname       
 ----------------------------------------------------------------------------- 
 192.168.17.1    00:50:56:c0:00:08      1      60  VMware, Inc.                 
 192.168.17.2    00:50:56:e5:1f:80      1      60  VMware, Inc.                 
 192.168.17.131  00:0c:29:31:8a:2b      1      60  VMware, Inc.                 
 192.168.17.254  00:50:56:f1:23:55      1      60  VMware, Inc.     
```

## ARP

```text
root@kali:~/Documents/Training# arp-scan -l -v -I wlan0  
Interface: wlan0, datalink type: EN10MB (Ethernet) 
Using 192.168.1.0:255.255.255.0 for localnet 
Starting arp-scan 1.9.5 with 256 hosts (https://github.com/royhills/arp-scan) 
192.168.1.1 00:1d:aa:15:e5:28 DrayTek Corp. 
192.168.1.2 7c:4c:a5:76:14:54 BSkyB Ltd Padding=0f3d00100000000100000000000000000000 
192.168.1.3 c4:71:54:30:5f:cc (Unknown) 
192.168.1.11 94:c6:91:a1:1e:c6 (Unknown) 
192.168.1.17 f0:03:8c:67:a0:49 AzureWave Technology Inc.  
Padding=ea92d2071ad532a1a4afe6ff000000000000 
192.168.1.17 f0:03:8c:67:a0:49 AzureWave Technology Inc.  
Padding=882079afbeb6ee4c29bcf66f000000000000 (DUP: 2) 
192.168.1.18 98:01:a7:89:5b:e7 Apple, Inc. Padding=6fb47320fa70a613398d8560020405b40402 
192.168.1.25 d4:6e:0e:18:40:3b TP-LINK TECHNOLOGIES CO.,LTD.  
Padding=8aa9f02e679cb9dfd238cef2000000000000 
192.168.1.33 98:01:a7:89:5b:e7 Apple, Inc. 
192.168.1.12 18:1d:ea:9d:b9:17 (Unknown) 
--- Pass 1 complete 
192.168.1.34 18:1d:ea:9d:b9:17 (Unknown) 
192.168.1.21 d4:25:8b:e6:b2:c5 (Unknown) 
--- Pass 2 complete 

12 packets received by filter, 0 packets dropped by kernel 
Ending arp-scan 1.9.5: 256 hosts scanned in 2.412 seconds (106.14 hosts/sec). 12 responded 
```

## OneLiners - Ping sweep

### Windows

`for /L %i in (1,1,255) do @ping -n 1 -w 200 172.21.10.%i > nul && echo 192.168.1.%i is up.`

### Linux

`for i in {1..254} ;do (ping -c 1 172.21.10.$i | grep "bytes from" &) ;done`

