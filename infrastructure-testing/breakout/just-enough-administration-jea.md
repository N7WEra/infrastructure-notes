---
description: >-
  Just Enough Administration, or JEA. It allows administrators to limit the
  commands that specific users can run
---

# Just Enough Administration \(JEA\)

Link: [https://docs.microsoft.com/en-us/powershell/scripting/learn/remoting/jea/overview?view=powershell-7.1](https://docs.microsoft.com/en-us/powershell/scripting/learn/remoting/jea/overview?view=powershell-7.1)

Part of the JEA platform Administrators can:

* Limit what users can do by specifying which cmdlets, functions, and external commands they can run

To view what commands we can run in JEA session run:

```text
[10.10.10.210]: PS>Get-Command 

CommandType     Name                                               Version    Source
-----------     ----                                               -------    ------
Function        Clear-Host
Function        Exit-PSSession
Function        Get-Command
Function        Get-FormatData
Function        Get-Help
Function        Measure-Object
Function        Out-Default
Function        Select-Object
```

We might also be able to access environment variables:

```text
[10.10.10.210]: PS> $env:username
k.svensson
```

