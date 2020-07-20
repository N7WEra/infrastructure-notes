---
description: >-
  The Lightweight Directory Access Protocol is an open, vendor-neutral, industry
  standard application protocol for accessing and maintaining distributed
  directory information services.
---

# 363 - LDAP

### ldapdomaindump

Active Directory information dumper via LDAP

link: [https://github.com/dirkjanm/ldapdomaindump](https://github.com/dirkjanm/ldapdomaindump)

enumerating using username and password: 

```text
root@kali# ldapdomaindump -u 'htb.local\amanda' -p Ashare1972 10.10.10.103 -o ~/hackthebox/sizzle-10.10.10.103/ldap/ 
[*] Connecting to host... 
[*] Binding to host 
[+] Bind OK 
[*] Starting domain dump 
[+] Domain dump finished 
root@kali# ls ~/hackthebox/sizzle-10.10.10.103/ldap/ 
domain_computers_by_os.html  domain_computers.json  domain_groups.json  domain_policy.json  domain_trusts.json          domain_users.html 
domain_computers.grep        domain_groups.grep     domain_policy.grep  domain_trusts.grep  domain_users_by_group.html  domain_users.json 
domain_computers.html        domain_groups.html     domain_policy.html  domain_trusts.html  domain_users.grep 
```

### Nmap

nmap --script ldap-\* 10.10.10.169 

### ldapsearch

Anonymous Credential LDAP Dumping:

`ldapsearch -LLL -x -H ldap://10.10.10.175 -b ‘’ -s base ‘(objectclass=*)’`

* `-x` - simple auth
* `-h 10.10.10.175` - host to query
* `-s base` - set the scope to base

###  windapsearch

Python script to enumerate users, groups and computers from a Windows domain through LDAP queries

Link: [https://github.com/ropnop/windapsearch](https://github.com/ropnop/windapsearch)

Usage: 

```text
$  ./windapsearch.py -d lab.ropnop.com -u ropnop\\ldapbind -p GoCubs16 -U
```

### go-windapsearch

Link: [https://github.com/ropnop/go-windapsearch](https://github.com/ropnop/go-windapsearch)

Go version of windapsearch



