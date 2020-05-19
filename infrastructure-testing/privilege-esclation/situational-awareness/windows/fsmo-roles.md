---
description: >-
  Flexible single master operation (FSMO) is a Microsoft Active Directory
  feature that is a specialized domain controller task used when standard data
  transfer and update methods are inadequate.
---

# FSMO Roles

##  The roles 

**Forest Wide Roles:** 

Schema Master 

Domain naming master 

**Domain Wide Roles:** 

PDC 

RID pool manager 

Infrastructure Master 

## How to Quickly check FSMO roles

**CMD:** 

`netdom query fsmo` 

**Powershell** 

`Get-ADForest yourdomain | Format-Table SchemaMaster,DomainNamingMaster` 

`Get-ADDomain yourdomain | format-table`  

