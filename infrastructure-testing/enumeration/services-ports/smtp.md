---
description: >-
  The Simple Mail Transfer Protocol is a communication protocol for electronic
  mail transmission.
---

# 25 - SMTP

### SMTP User Enumeration Utility 

Allows the enumeration of users: VRFY \(confirming the names of valid users\) and EXPN \(which reveals the actual address of users aliases and lists of e-mail \(mailing lists\)\). Through the implementation of these SMTP commands can reveal a list of valid users. User files contains only Unix usernames so it skips the Microsoft based Email SMTP Server. This can be changed using UNIXONLY option and custom user list can also be provided. 

**Metasploit:** 

`use auxiliary/scanner/smtp/smtp_enum` 

### Manual Enumeration

You can guess for valid user account through the following command and if you receive response code 550 it means unknown user account:

telnet into the host:

`telnet 192.168.0.1 25`

Using vrfy: 

```text
vrfy raj@mail.lab.ignite
```

Using rcpt:

```text
RCPT TO:root 
```

If you received a message code 250,251,252 which means the server has accepted the request and user account is valid. 

### smtp-user-enum \(builtin in Kali\) 

`smtp-user-enum -M VRFY -U users.txt -t 10.0.0.1` 

### Nmap

`nmap –script smtp-enum-users.nse 172.16.212.133` 

