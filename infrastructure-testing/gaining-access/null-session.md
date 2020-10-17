---
description: >-
  null session is an anonymous connection to an inter-process communication
  network service on Windows-based computers
---

# Null session

## From windows

```text
net use \\TARGET\IPC$ "" /u:"" 
```

## From Linux

```text
smbclient -L //192.168.99.131 
smbclient //10.10.10.100/Replication -U ""%"" 
rpcclient -U "" 10.10.10.10 
```

## CME

```text
crackmapexec smb 10.10.10.192 -u 'anonymous' -p ''
SMB         10.10.10.192    445    DC01             [*] Windows 10.0 Build 17763 x64 (name:DC01) (domain:BLACKFIELD.local) (signing:True) (SMBv1:False)
SMB         10.10.10.192    445    DC01             [+] BLACKFIELD.local\anonymous: 
```

