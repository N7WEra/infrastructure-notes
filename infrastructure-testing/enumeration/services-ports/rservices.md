---
description: >-
  The Berkeley r-commands are a suite of computer programs designed to enable
  users of one Unix system to log in or issue commands to another Unix computer.
---

# 512/513 - R Services

**Note** that on kali the r services by default mapped to SSH, so the application will need to be installed by running:

`apt install rwho rlogin rsh-client`

### RSH

RSH Run Commands 

`rsh <target> <command>` 

Metasploit RSH Login Scanner 

`auxiliary/scanner/rservices/rsh_login` 

rusers Show Logged in Users 

`rusers -al 192.168.2.1` 

rusers scan whole Subnet 

`rlogin -l <user> <target>` 

### Rlogin 

One of the services that you can discover in Unix environments is the rlogin.This service runs on port 513 and it allows users to login to the host remotely.This service was mostly used in the old days for remote administration but now because of security issues this service has been replaced by the slogin and the ssh.However if you find a system that is not properly configured and is using this service then you should try to exploit. 

Install rlogin client:  

`apt install rsh-client` 

The last step is to use the command: 

`rlogin -l root IP` 

This command will try to login to the remote host by using the login name root 

Metasploit module: 

`use auxiliary/scanner/rservices/rsh_login` 

### Rwho 

Use nmap to identify machines running rwhod \(513 UDP\) 

Use rwho \(apt install rwho\) 

```text
mike@ubuntu12:~$ rwho 
Mike  ubuntu12:pts/0 Jan 22 13:24 
Mike  ubuntu12:tty7 Jan 22 13:24 
```

