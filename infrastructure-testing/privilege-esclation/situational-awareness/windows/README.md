# Windows

**User details:** 

`Whoami` 

`hostname` 

`Echo %username%` 

`Net users` 

`Net user USERNAME` 

**Get Windows User and Domain Information** 

`set` 

`whoami /all` 

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

```text

```

