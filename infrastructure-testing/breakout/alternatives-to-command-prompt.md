---
description: Different options to cmd and powershell
---

# Alternatives to command prompt

## HTA Shell

its text, so you could copy paste the text into notepad, save as a "shell.hta" using the quotes to enforce file extension 

then file open on notepad right click the shell.hta and open 

Code: 

{% embed url="https://raw.githubusercontent.com/nccgroup/OneLogicalMyth\_Shell/master/OneLogicalShell.hta" %}

## Powershell Alternatives

### PowerShdll

Link: 

[https://github.com/p3nt4/PowerShdll](https://github.com/p3nt4/PowerShdll) 

#### Example: 

`rundll32.exe PowerShdll.dll,main` 

#### dll mode: 

```text
rundll32 PowerShdll,main <script> 
rundll32 PowerShdll,main -h      Display this message 
rundll32 PowerShdll,main -f <path>       Run the script passed as argument 
rundll32 PowerShdll,main -w      Start an interactive console in a new window (Default) 
rundll32 PowerShdll,main -i      Start an interactive console in this console 
```

If you do not have an interactive console, use -n to avoid crashes on output 

Alternatives \(Credit to SubTee for these techniques\): 

```text
x86 - C:\Windows\Microsoft.NET\Framework\v4.0.30319\InstallUtil.exe /logfile= /LogToConsole=false /U PowerShdll.dll 
x64 - C:\Windows\Microsoft.NET\Framework64\v4.0.3031964\InstallUtil.exe /logfile= /LogToConsole=false /U PowerShdll.dll 
x86 C:\Windows\Microsoft.NET\Framework\v4.0.30319\regsvcs.exe PowerShdll.dll 
x64 C:\Windows\Microsoft.NET\Framework64\v4.0.30319\regsvcs.exe PowerShdll.dll 
x86 C:\Windows\Microsoft.NET\Framework\v4.0.30319\regasm.exe /U PowerShdll.dll 
x64 C:\Windows\Microsoft.NET\Framework64\v4.0.30319\regasm.exe /U PowerShdll.dll 
regsvr32 /s  /u PowerShdll.dll -->Calls DllUnregisterServer 
regsvr32 /s PowerShdll.dll --> Calls DllRegisterServer 
```

####  exe mode 

Usage: 

```text
PowerShdll.exe <script> 
PowerShdll.exe -h      Display this message 
PowerShdll.exe -f <path>       Run the script passed as argument 
PowerShdll.exe -i      Start an interactive console in this console (Default) 
```

Examples 

Run base64 encoded script 

`rundll32 Powershdll.dll,main [System.Text.Encoding]::Default.GetString([System.Convert]::FromBase64String("BASE64")) ^| iex` 

### PowerLine

Link: [https://github.com/fullmetalcache/PowerLine ](https://github.com/fullmetalcache/PowerLine%20)

### NPS

Not PowerShell - When powershell is blocked 

Link:  [https://github.com/Ben0xA/nps](https://github.com/Ben0xA/nps) 

#### Usage 

```text
nps.exe "{powershell single command}" 
nps.exe "& {commands; semi-colon; separated}" 
nps.exe -encodedcommand {base64_encoded_command} 
nps.exe -encode "commands to encode to base64" 
nps.exe -decode {base64_encoded_command} 
```

**Single Commands:** 

```text
 c:\Downloads>nps.exe Get-Date 
 12/18/2015 2:19:37 PM 
```

## CMD Alternatives

{% embed url="https://blog.didierstevens.com/2010/02/04/cmd-dll/" %}

Example:

`rundll32.exe cmd.dll,main` 

