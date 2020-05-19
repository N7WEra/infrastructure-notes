---
description: >-
  NetBIOS is an acronym for Network Basic Input/Output System. It provides
  services related to the session layer of the OSI model allowing applications
  on separate computers to communicate over a local.
---

# 137 - Netbios

### Nbtscan \(Kali version\)

`root@DESKTOP99:~/Downloads# nbtscan -vh 192.168.0.15` 

Use –h for human readable  \(-v is verbose\)

### nbt-wizz version version: 

```text
root@Kali:~/Downloads# ./nbtscan-1.0.35-redhat-linux -A 192.168.0.15 
192.168.0.38    WORKGROUP\DOOKOSSEL             SHARING 
  DOOKOSSEL      <00> UNIQUE Workstation Service 
  DOOKOSSEL      <03> UNIQUE Messenger Service<3> 
  DOOKOSSEL      <20> UNIQUE File Server Service 
  ..__MSBROWSE__.<01> GROUP  Master Browser 
  WORKGROUP      <00> GROUP  Domain Name 
  WORKGROUP      <1d> UNIQUE Master Browser 
  WORKGROUP      <1e> GROUP  Browser Service Elections 
  00:00:00:00:00:00   ETHER 
```

### FSMO roles \(will be on the nbtscan\)

* Schema master
* Domain naming master
* RID master
* PDC emulator
* Infrastructure master

