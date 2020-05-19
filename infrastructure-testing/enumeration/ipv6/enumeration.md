---
description: >-
  Credit to Roxana Kovaci (https://twitter.com/RoxanaKovaci) and her SteelCon
  IPv6 workshop
---

# Enumeration

### Ping a host

```text
ping6 <IPv6>(ooptional:% <Interface to go out from>) 

root@kali:~/# ping6 dead:beef:0000:0000:0250:56ff:feb9:ec70 
PING dead:beef:0000:0000:0250:56ff:feb9:ec70(dead:beef::250:56ff:feb9:ec70) 56 data bytes 
64 bytes from dead:beef::250:56ff:feb9:ec70: icmp_seq=1 ttl=63 time=32.5 ms 
64 bytes from dead:beef::250:56ff:feb9:ec70: icmp_seq=2 ttl=63 time=40.5 ms 
^C 

--- dead:beef:0000:0000:0250:56ff:feb9:ec70 ping statistics --- 
2 packets transmitted, 2 received, 0% packet loss, time 3ms 
rtt min/avg/max/mdev = 32.458/36.465/40.473/4.012 ms 
```

### SSH

```text
root@kali:~/ # ssh -6 loki@dead:beef:0000:0000:0250:56ff:feb9:a37d 
The authenticity of host 'dead:beef::250:56ff:feb9:a37d (dead:beef::250:56ff:feb9:a37d)' can't be established. 
ECDSA key fingerprint is SHA256:deaxXTK7ORthfGcKdblPRUmgNrU20oclqMbwVj3hzYI.Are you sure you want to continue connecting (yes/no)? yes 
Warning: Permanently added 'dead:beef::250:56ff:feb9:a37d' (ECDSA) to the list of known hosts. 
loki@dead:beef::250:56ff:feb9:a37d's password:  
```

### Access web services

`http://[fe80::20c:29ff:fe69:c4e5%eth0]:8888/index.html`

### Curl

`curl -g -6 ”http://[fe80::a00:27ff:fe33:498e%eth0]:8080/test.txt“ -o test.txt`

### SNMP

Note: Consider using Enyx \([https://github.com/trickster0/Enyx](https://github.com/trickster0/Enyx%20)\)

### SSH over IPv6 

`ssh -6 user@fe80::30a8:9d3d:3842:8593%eth0` 

### FTP over IPv6 

`ftp -6 fe80::a00:27ff:fe33:498e%eth0`

### Telnet over IPv6 

`telnet -6 fe80::30b8:9d3d:3842:8593%eth0` 

### MySQL over IPv6: 

`mysql -h fe80::30b8:9d3d:3842:8593%eth0 -u user -p pass` 

### RDP over IPv6 on a different port than the default one 

`rdesktop [fe80::a00:27ff:fe33:498e%eth0]:45001` 

### Password cracking over IPv6

`hydra -v -f -6 fe80::20c:29ff:fe69:c4e5%eth0 -l root -P passwords.txt ssh` 

`ncrack -f -6 -v --user admin -P passwords.txt rdp://ipv6-localhost` 

### Reverse connections for getting a foothold 

Metasploit framework:

`payload/windows/meterpreter/reverse_ipv6_tcp`  

or  

`payload/bsd/x86/shell_reverse_tcp_ipv6, etc`













