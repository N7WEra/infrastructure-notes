---
description: >-
  Virtual Network Computing (VNC) is a graphical desktop sharing system that
  uses the Remote Frame Buffer protocol (RFB) to remotely control another
  computer. It transmits the keyboard and mouse events
---

# 5900 - VNC

**Nmap**: 

Script: vnc-info 

 **Example Usage** 

```text
nmap -p 5901 --script vnc-info 192.168.0.50

PORT    STATE SERVICE 
5900/tcp open  vnc 
| vnc-info: 
|   Protocol version: 3.889 
|   Security types: 
|     Mac OS X security type (30) 
|_    Mac OS X security type (35) 
```

**Connect to VNC:**

`vncviewer $IP:5901` 

**Brute Force with Metasploit** 

`use auxiliary/scanner/vnc/vnc_login` 

