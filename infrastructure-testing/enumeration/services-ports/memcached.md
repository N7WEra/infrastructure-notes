---
description: >-
  Memcached is a general-purpose distributed memory-caching system. It is often
  used to speed up dynamic database-driven websites by caching data and objects
  in RAM to reduce the number of times an exte
---

# 11211 - Memcached

**Nmap**: 

```text
root@attackdefense:~# nmap --script memcached-info -p 11211 192.170.149.3 
Starting Nmap 7.70 ( https://nmap.org ) at 2019-10-30 10:54 UTC 
Nmap scan report for target-1 (192.170.149.3) 
Host is up (0.000053s latency). 
 
PORT      STATE SERVICE 
11211/tcp open  memcache 
| memcached-info:  
|   Process ID: 7 
|   Uptime: 1037 seconds 
|   Server time: 2019-10-30T10:54:06 
|   Architecture: 64 bit 
|   Used CPU (user): 0.101069 
|   Used CPU (system): 0.148907 
|   Current connections: 3 
|   Total connections: 9 
|   Maximum connections: 2147 
|   TCP Port: 11211 
|   UDP Port: 0 
|_  Authentication: no 

MAC Address: 02:42:C0:AA:95:03 (Unknown) 
```

**Connecting:**

`root@attackdefense:~# nc 192.170.149.3 11211` 

**You can query the current traffic statistics using the command** 

`stats` 

**View items:** 

`stats items` 

Example:

```text
ITEM username [5 b; 0 s] 
ITEM email [18 b; 0 s] 
ITEM name [4 b; 0 s] 
END 
stats cachedump 5 0 
ITEM secret [142 b; 0 s] 
END 
stats cachedump 25 0 
ITEM image [19514 b; 0 s] 
END 
```

**view key:** 

```text
get secret 
VALUE secret 0 142 
(dp0 
S'flag-part-1' 
p1 
S'886a1cb' 
p2 
sS'flag-part-2' 
p3 
S'00dfaa588' 
```

## **memcdump**

memdump dumps a list of "keys" from all servers that it is told to fetch from. Because memcached does not guarentee to provide all keys it is not possible to get a complete "dump".

```text
root@attackdefense:~# memcdump --server 192.170.149.3 
maxUsers 
last-update 
maintainer 
```

## memcached Cheat Sheet

[https://lzone.de/cheat-sheet/memcached](https://lzone.de/cheat-sheet/memcached) 

