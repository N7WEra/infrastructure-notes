---
description: >-
  Gained access to a lockdown host and need to find way to escape the restrict
  shell?
---

# Breakout

## Methodology

### Gaining shell access

1. Check what software's you can access 
   1. Try common such as cmd, powershell, powershell\_ise, ftp and etc
   2. Try  to use alternatives to powershell and cmd such as PowerShdll and 
   3. Try explorer bar commands 
   4. [Check if you can access internet explorer, paint, excel, file explorer and etc ](windows-utilities.md)
2. [Check keyboard shortcuts ](windows-utilities.md#shortcuts)
3. Try to create a new file \(Use the malicious[ HTA file](alternatives-to-command-prompt.md#hta-shell)\) or a new shortcut and point it to a executable 
4. [Load files via a SMB share and execute them](../enumeration/ipv6/transfering-files.md)
5. Windows 10 - try[ Cortana exploit ](windows-utilities.md#cortana)
6. Try and to copy powershell.exe or cmd.exe and change it to a different name and then run it 
7. Try and access \\127.0.0.1\c$

### Once a shell was obtained

1. [Bypassing powershell restrictions](powershell-constrained-language-byass.md)
   1. If it's powershell try and download reverse shell and run it, if it's version 4 check if you can downgrade to powershell v2 
   2. Try to use Powershell alternatives \(nps, powershelld and etc\) 
2. Test if you can execute commands via[ LOLBAS](lolbas.md) 
3. Attempt [UAC Bypass](../privilege-esclation/windows/uac-bypass.md) to gain administrative privileges 

