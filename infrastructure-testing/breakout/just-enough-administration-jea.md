---
description: >-
  Just Enough Administration, or JEA. It allows administrators to limit the
  commands that specific users can run
---

# Just Enough Administration \(JEA\)

Link: [https://docs.microsoft.com/en-us/powershell/scripting/learn/remoting/jea/overview?view=powershell-7.1](https://docs.microsoft.com/en-us/powershell/scripting/learn/remoting/jea/overview?view=powershell-7.1)

Part of the JEA platform Administrators can:

* Limit what users can do by specifying which cmdlets, functions, and external commands they can run

## What we can do

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

[10.10.10.210]: PS> $ExecutionContext.SessionState.LanguageMode
ConstrainedLanguage

```

## Breakout

### Define function

We might not be able to run a command directly, but we can try and create a function and run the command inside

```text
[10.10.10.210]: PS>get-location
The term 'Get-Location' is not recognized as the name of a cmdlet, function, script file, or operable program. Check the spelling of the name, or if a path was 
included, verify that the path is correct and try again.
    + CategoryInfo          : ObjectNotFound: (Get-Location:String) [], CommandNotFoundException
    + FullyQualifiedErrorId : CommandNotFoundException
 
[10.10.10.210]: PS> function gl {get-location}; gl

Path                         
----                         
C:\Users\k.svensson\Documents
```

Other way to do the same is to use the `&` operator with script block

```text
[10.10.10.210]: PS> &{ get-location }

Path                         
----                         
C:\Users\k.svensson\Documents
```

