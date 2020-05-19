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

