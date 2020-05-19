---
description: >-
  icacls is a command-line utility that can be used to modify NTFS file system
  permissions in Windows.
---

# icacls

icacls is a command-line utility that can be used to modify NTFS file system permissions in Windows Server 2003 SP2, Windows Server 2008, Windows Vista and Windows 7. It builds on the functionality of similar previous utilities, including cacls, Xcacls.exe, Cacls.exe, and Xcacls.vbs.   

**example**: 

```text
PS htb\amanda@SIZZLE documents> icacls clean.bat 
clean.bat NT AUTHORITY\SYSTEM:(I)(F) 
          BUILTIN\Administrators:(I)(F) 
          HTB\Administrator:(I)(F) 
          HTB\amanda:(I)(F) 
```

**Change permissions:** 

icacls C:\PS /grant  John:M 

**Remove permissions:** 

icacls C:\PS /remove John 

Opposed to each group and the user’s access level is specified. Access rights are indicated using abbreviations. Consider the permissions for the user CORP\someusername. The following permissions are assigned to this user: 

* \(OI\) — object inherit 
* \(CI\) — container inherit 
* \(M\) —  modify access 

This means that this user has the rights to write and modify data in this directory. These rights are inherited to all child objects in this directory. 

Below is a complete list of permissions that can be set using the icacls utility: 

iCACLS inheritance settings: 

* \(OI\)  —  object inherit 
* \(CI\)  —  container inherit 
* \(IO\)  —  inherit only 
* \(NP\)  —  don’t propagate inherit 
* \(I\)  — permission inherited from parent container 

List of basic access permissions: 

* D  —  delete access 
* F  —  full access 
* N  —  no access 
* M  —  modify access 
* RX  —  read and eXecute access 
* R  —  read-only access 
* W  —  write-only access 

Detailed permissions: 

* DE  —  delete 
* RC  —  read control 
* WDAC  —  write DAC 
* WO  — write owner 
* S  —  synchronize 
* AS  —  access system security 
* MA  —  maximum allowed permissions 
* GR  —  generic read 
* GW  —  generic write 
* GE  —  generic execute 
* GA  —  generic all 
* RD  —  read data/list directory 
* WD  —  write data/add file 
* AD  — append data/add subdirectory 
* REA  —  read extended attributes 
* WEA  —  write extended attributes 
* X  —  execute/traverse 
* DC  —  delete child 
* RA  —  read attributes 
* WA  —  write attributes 

