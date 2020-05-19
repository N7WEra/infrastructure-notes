---
description: powershell on kali ( = Linux)
---

# pwsh

Kali come with PowerShell installed already, to run it use: 

`root@kali:~# pwsh` 

**If not installed use:** 

```text
apt update && apt -y install curl gnupg apt-transport-https 
curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add -
echo "deb [arch=amd64] https://packages.microsoft.com/repos/microsoft-debian-stretch-prod stretch main" > etc/apt/sources.list.d/powershell.list 
apt update 
apt install powershell 
```

**Example:** 

```text
root@Kali:/opt/PowerUpSQL# pwsh 
PowerShell 6.2.2 
Copyright (c) Microsoft Corporation. All rights reserved. 
https://aka.ms/pscore6-docs 
Type 'help' to get help. 
PS /opt/PowerUpSQL> Import-Module .\PowerUpSQL.ps1  
PS /opt/PowerUpSQL>  
```



