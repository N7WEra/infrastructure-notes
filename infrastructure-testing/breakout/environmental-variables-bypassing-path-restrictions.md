# Environmental Variables / Bypassing Path Restrictions

In some systems where minimal hardening has taken place, it may not be possible to browse directly to an obvious directory such as C:\Windows\System32. There are however various symbolic links that one can use to potentially bypass this restriction:

```text
%ALLUSERSPROFILE% 
%APPDATA% 
%CommonProgramFiles% 
%COMMONPROGRAMFILES(x86)% 
%COMPUTERNAME% 
%COMSPEC% 
%HOMEDRIVE% 
%HOMEPATH% 
%LOCALAPPDATA% 
%LOGONSERVER% 
%PATH% 
%PATHEXT% 
%ProgramData% 
%ProgramFiles% 
%ProgramFiles(x86)% 
%PROMPT% 
%PSModulePath% 
%Public% 
%SYSTEMDRIVE% 
%SYSTEMROOT% 
%TEMP% 
%TMP% 
%USERDOMAIN% 
%USERNAME% 
%USERPROFILE% 
%WINDIR% 
shell:Administrative Tools 
shell:DocumentsLibrary 
shell:Librariesshell:UserProfiles 
shell:Personal 
shell:SearchHomeFolder 
shell:System shell:NetworkPlacesFolder 
shell:SendTo 
shell:UserProfiles 
shell:Common Administrative Tools 
shell:MyComputerFolder 
shell:InternetFolder 
```

