# Registry

### Read Registry values

**Powershell**: 

`Get-ItemProperty -Path Registry::HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion` 

 Or

`cd HKCU:` or `cd HKLM:`  
**CMD**: 

`REG QUERY HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Windows\` 

