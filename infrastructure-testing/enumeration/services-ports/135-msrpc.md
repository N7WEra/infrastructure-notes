---
description: Microsoft RPC is a modified version of DCE/RPC.
---

# 135 - MSRPC

TCP 135 is the Endpoint Mapper and Component Object Model \(COM\) Service Control Manager.

## Enumeration

### Nmap

Queries an MSRPC endpoint mapper for a list of mapped services and displays the gathered information.

```text
nmap <target> --script=msrpc-enum
```

### rpcdump.py

rpcdump.py from Impacket that will show these mappings of RPC

for unauthenticated RPC mapping use:

`python3 rpcdump.py '':''@10.10.10.213`

### rpcmap.py

rpcmap.py from Impacket that will show these mappings of RPC

```text
rpcmap.py 'ncacn_ip_tcp:10.10.10.213'
```

### IOXIDResolver

IOXIDResolver could be used to obtain information on other interfaces from RPC mapping

```text
oxdf@parrot$ python3 IOXIDResolver.py -t 10.10.10.213
[*] Retrieving network interface of 10.10.10.213
Address: apt
Address: 10.10.10.213
Address: dead:beef::b885:d62a:d679:573f
Address: dead:beef::9514:421b:5cde:a7da
```

