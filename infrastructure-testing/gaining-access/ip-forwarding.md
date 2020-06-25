# IP Forwarding

Detects whether the remote device has ip forwarding or "Internet connection sharing" enabled, by sending an ICMP echo request to a given target using the scanned host as default gateway.

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

