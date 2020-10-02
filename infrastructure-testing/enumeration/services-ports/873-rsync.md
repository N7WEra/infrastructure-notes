---
description: >-
  Rsync is a utility for transferring and synchronizing files between two
  servers (usually Linux).
---

# 873 - RSYNC

example of Rsyncd.conf file that allows anonymous root access to the entire file system:

```text
motd file = /etc/Rsyncd.motd
lock file = /var/run/Rsync.lock
log file = /var/log/Rsyncd.log
pid file = /var/run/Rsyncd.pid

[files]
path = /
comment = Remote file share.
uid = 0
gid = 0
read only = no
list = yes
```

## enumeration

`nmap -p 873 192.168.0.1`

List directory

```text
rsync 192.168.1.171::
```

List sub directory contents

```text
rsync 192.168.1.171::files
```

List directories and files recursively

```text
rsync -r 192.168.1.171::files/tmp/
```

Download files

```text
rsync 192.168.1.171::files/home/test/mypassword.txt .
```

Download folders

```text
rsync -r 192.168.1.171::files/home/test/
```

Upload files

```text
rsync ./myfile.txt 192.168.1.171::files/home/test
```

Upload folders

```text
rsync -r ./myfolder 192.168.1.171::files/home/test
```

source:

[https://blog.netspi.com/linux-hacking-case-studies-part-1-rsync/](https://blog.netspi.com/linux-hacking-case-studies-part-1-rsync/)

