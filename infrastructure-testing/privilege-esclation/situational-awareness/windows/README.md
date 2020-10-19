# Windows

| Domain | Comment |
| :--- | :--- |
| net view | list computers on domain |
| net view \\&lt;target name&gt; | list shares on host |
| net view /domain | list domains |
| net view /domain:&lt;domain name&gt; | list computers on a named domain |
| net users &lt;username&gt; &lt;password&gt; /add | add user |
| net localgroup Administrators &lt;username&gt; | add to administrators group |
| nltest /dclist:&lt;domain name&gt; | Domain Controllers list |

### **User details:** 

`Whoami` 

`hostname` 

`Echo %username%` 

`Net users` 

`Net user USERNAME` 

### **Get Windows User and Domain Information** 

`set` 

`whoami /all` 

`Get-ADTrust`

### **Information on current domain:**

Domain information:

`[System.DirectoryServices.ActiveDirectory.Domain]::GetCurrentDomain()`

Domain Trusts:

`([System.DirectoryServices.ActiveDirectory.Domain]::GetCurrentDomain()).GetAllTrustRelationships()`

Current forest info:

`[System.DirectoryServices.ActiveDirectory.Forest]::GetCurrentForest()`

Trust relationship:

`([System.DirectoryServices.ActiveDirectory.Forest]::GetForest((New-Object System.DirectoryServices.ActiveDirectory.DirectoryContext('Forest', 'forest-of-interest.local')))).GetAllTrustRelationships()`



**Get current privileges:** 

`whoami /priv` 

**Show routes:** 

`route print` 

**Enumerate local administrators** 

`net localgroup administrators` 

**Check for missing patches:** 

`wmic qfe get Caption,Description, HotFixID,InstalledOn` 

**get DCs of a domain** 

`net group "domain controllers" /domain` 

**Launch a cmd prompt as another user:** 

`runas /netonly /user:[Domain]\[username] cmd.exe` 

**Get windows version:** 

`ver` 

**Systeminfo:** 

`systeminfofindstr/B /C:"OS Name" /C:"OS Version` 

**View password policy:** 

`net accounts` 

On DC:

`Get-ADDefaultDomainPasswordPolicy` 

**List Drives:**

`gdr -PSProvider 'FileSystem'`

Or

`[System.IO.DriveInfo]::GetDrives() | Format-Table`



