---
description: >-
  Trivial File Transfer Protocol is a simple lockstep File Transfer Protocol
  which allows a client to get a file from or put a file onto a remote host.
---

# 69 - TFTP

## Enumeration

Confirm TFTP is open:

`nmap -sU -p 69 10.10.10.90`

Enumerate files:

`nmap -sU -p 69 --script tftp-enum.nse --script-args tftp-enum.filelist=customlist.txt <host>`

## Connect

Can use the built in utility:

```text
$ tftp 10.10.10.90
tftp> get non-existing-file
Error code 1: Could not find file 'C:\non-existing-file'.

tftp> get WINDOWS\System32\drivers\etc\hosts
Received 734 bytes in 0.1 seconds [58720 bits/sec]
```

Put files:

```text
$ tftp 10.10.10.90
tftp> put localfile.txt
```

## Brute force files

### Nmap

```text
nmap -sU -p 69 --script tftp-enum.nse --script-args tftp-enum.filelist=customlist.txt <host>
Script Output
PORT   STATE SERVICE REASON
69/udp open  tftp    script-set
| tftp-enum:
|_  bootrom.ld
```

Metasploit

```text
msf > use auxiliary/scanner/tftp/tftpbrute
```

