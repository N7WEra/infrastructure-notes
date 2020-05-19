---
description: >-
  Secure Shell is a cryptographic network protocol for operating network
  services securely over an unsecured network. applications include remote
  command-line, login, and remote command execution.
---

# 22 - SSH

### Files

Each SSH server has its own key and signature which it presents upon initial connection by a client. This is an extra integrity step to minimise the risk of man-in-the-middle attacks. Once the host key has been accepted its signature is saved in .ssh/known\_hosts on the client. 

This means that we would have, at least the following files on the server 

 `.ssh/authorized_keys – holding the signature of the public key of any authorised clients` 

And the following files on the client: 

`.ssh/id_rsa – Holds the private key for the client` 

`.ssh/id_rsa.pub – Holds the public key for the client` 

`.ssh/known_hosts – Holds a list of host signatures of hosts that the client has previously connected to` 

Generating ssh key: 

```text
root@Kali:~# ssh-keygen  
Generating public/private rsa key pair. 
Enter file in which to save the key (/root/.ssh/id_rsa):  
Created directory '/root/.ssh'. 
Enter passphrase (empty for no passphrase):  
Enter same passphrase again:  
Your identification has been saved in /root/.ssh/id_rsa. 
Your public key has been saved in /root/.ssh/id_rsa.pub. 
The key fingerprint is: 
SHA256:0S22hr1iXCscptJ3CUDSsKPMYrFVOfFJIgvH8pEtst8 root@DESKTOP99 
The key's randomart image is: 
+---[RSA 3072]----+ 
| ..o=*+.         | 
| oo=+B= .. .     | 
| .=o= oo. + .    | 
| ++o . . = o     | 
|.o= .   S =      | 
|.. . E = = +     | 
|    . o B =      | 
|     . o +       | 
|                 | 
+----[SHA256]-----+ 
```

**Choice encryption and key length:** 

ssh-keygen -t rsa -b 4096 

Copy the id\_rsa.pub to the authorized\_keys

or use the `ssh-copy-id` command

```text
ssh-copy-id -i ~/.ssh/mykey user@host
```

### Enumeration

```text
msf5 auxiliary(scanner/ssh/ssh_enumusers) > show options  
Module options (auxiliary/scanner/ssh/ssh_enumusers): 
Name Current Setting Required Description 
---- --------------- -------- ----------- 
CHECK_FALSE false no Check for false positives (random username) 
Proxies no A proxy chain of format type:host:port[,type:host:port][...] 
RHOSTS yes The target address range or CIDR identifier 
RPORT 22 yes The target port 
THREADS 1 yes The number of concurrent threads 
THRESHOLD 10 yes Amount of seconds needed before a user is considered found (timing attack only) 
USERNAME no Single username to test (username spray) 
USER_FILE no File containing usernames, one per line 
Auxiliary action: 
Name Description 
---- ----------- 
Malformed Packet Use a malformed packet 
msf5 auxiliary(scanner/ssh/ssh_enumusers) > set THREADS 50 
THREADS => 50 
msf5 auxiliary(scanner/ssh/ssh_enumusers) > set RHOSTS 10.10.10.86 
RHOSTS => 10.10.10.86 
msf5 auxiliary(scanner/ssh/ssh_enumusers) > set USER_FILE users.lst 
USER_FILE => users.lst 
msf5 auxiliary(scanner/ssh/ssh_enumusers) > run 
[*] 10.10.10.86:22 - SSH - Using malformed packet technique 
[*] 10.10.10.86:22 - SSH - Starting scan 
[-] 10.10.10.86:22 - SSH - User 'jackie.abbott' not found 
[-] 10.10.10.86:22 - SSH - User 'isidro' not found 
[-] 10.10.10.86:22 - SSH - User 'roy' not found 
[-] 10.10.10.86:22 - SSH - User 'colleen' not found 
[-] 10.10.10.86:22 - SSH - User 'harrison.hessel' not found 
[-] 10.10.10.86:22 - SSH - User 'asa.christiansen' not found 
[-] 10.10.10.86:22 - SSH - User 'jessie' not found 
[-] 10.10.10.86:22 - SSH - User 'milton_hintz' not found 
[-] 10.10.10.86:22 - SSH - User 'demario_homenick' not found 
[-] 10.10.10.86:22 - SSH - User 'paris' not found 
[-] 10.10.10.86:22 - SSH - User 'gardner_ward' not found 
[-] 10.10.10.86:22 - SSH - User 'daija.casper' not found 
[-] 10.10.10.86:22 - SSH - User 'alanna.prohaska' not found 
[-] 10.10.10.86:22 - SSH - User 'russell_borer' not found 
[-] 10.10.10.86:22 - SSH - User 'domenica.kulas' not found 
[-] 10.10.10.86:22 - SSH - User 'nick' not found 
[-] 10.10.10.86:22 - SSH - User 'rose' not found 
[-] 10.10.10.86:22 - SSH - User 'pat_wilkinson' not found 
[+] 10.10.10.86:22 - SSH - User 'genevieve' found 
[-] 10.10.10.86:22 - SSH - User 'blaise.sauer' not found 
[-] 10.10.10.86:22 - SSH - User 'abbigail' not found 
```

[https://www.rapid7.com/db/modules/auxiliary/scanner/ssh/ssh\_enumusers](https://www.rapid7.com/db/modules/auxiliary/scanner/ssh/ssh_enumusers) 

### SSH Mismatch

if you get the error: 

`Unable to negotiate with 123.123.123.123 port 22: no matching key exchange method found. Their offer: diffie-hellman-group1-sha1`

Use the '-oKexAlgorithms' or '-keyexchange' 

**Example**: 

`ssh -oKexAlgorithms=+diffie-hellman-group1-sha1 user@legacyhost`



### Install ssh v1 

`sudo apt-get install -y openssh-client-ssh1`  

