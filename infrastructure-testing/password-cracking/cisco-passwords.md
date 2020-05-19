---
description: >-
  Breaking different types of cisco passwords which can be obtained from the
  configuration file
---

# Cisco Passwords

## Summary

| Number | Hash type | Crack time |
| :--- | :--- | :--- |
| 0 | Clear-text | No need to crack |
| 4 | SHA-256 | Slow |
| 5 | MD5 | Fast |
| 7 | Vigenere cipher | Very Fast |
| 8 | PBKDF2-SHA-256 | Very slow |
| 9 | scrypt | Very Slow |

Password types can be identified \(same as in UNIX\) by the first part of the hash:

`$`**`8`**`$dsYGNam3K1SIJO$7nv/35M/qr6t.dVc7UY9zrJDWRVqncHub1PE9UlMQFs`

## Type 0

Password are in clear-text and no need to crack.

Command to use:

`enable password PASSWORD`

## Type 4

Cisco first attempt to create their own encryption and failed miserably, then they changed the encryption type to be sha256 without salt and 1 iteration and then based 64.

John:

John the Ripper recognizes this password type as **Raw-SHA256**. To crack it, we have to first convert it to the following john friendly format and save it in a file:

```text
admin:ds4zcEBHQMiiscBff5JmSaUctdI8fVdmGU18HAtxOCw
```

Then we can crack it like this using a dictionary, for example:

```text
john --format=Raw-SHA256 hashes.txt
```



Command to encrypt:

`enable secret 4 {HASH}`

Note: this type is deprecated starting from IOS 15.3\(3\)



## Type 5 

Using md5 as encryption, quite quick to crack \(depending on the length\)

### Using John

```text
root@Kali:/tmp# cat hash.txt  
021201481F1207285F  
root@Kali:/tmp# john hash.txt  
Warning: detected hash type "md5crypt", but the string is also recognized as "md5crypt-long" 
Use the "--format=md5crypt-long" option to force loading these as that type instead 
Using default input encoding: UTF-8 
Loaded 1 password hash (md5crypt, crypt(3) $1$ (and variants) [MD5 256/256 AVX2 8x3]) 
Will run 4 OpenMP threads 
Proceeding with single, rules:Single 
Press 'q' or Ctrl-C to abort, almost any other key for status 
Almost done: Processing the remaining buffered candidate passwords, if any. 
Warning: Only 53 candidates buffered for the current salt, minimum 96 needed for performance. 
Proceeding with wordlist:/usr/share/john/password.lst, rules:Wordlist 
test             (hash) 
1g 0:00:00:00 DONE 2/3 (2019-11-12 09:22) 1.886g/s 36245p/s 36245c/s 36245C/s 123456..larry 
Use the "--show" option to display all of the cracked passwords reliably 
Session completed 
```

**Command to encrypt:**

 `enable secret 5 00271A5307542A02D22842`  
\(notice above is not the password string it self but the hash of the password\)  
or  
`enable secret cisco123`  
\(notice above is the password string it self\)

## Type 7 

Encrypted using Vigenere cipher \(very very weak\)

Use the following script: 

### Python3

```text
#!/usr/bin/env python3 
 
import sys 
from binascii import unhexlify 
 
if len(sys.argv) != 2: 
    print(f"Usage: {sys.argv[0]} [level 7 hash]") 
    exit() 
 
static_key = "tfd;kfoA,.iyewrkldJKD" 
enc = sys.argv[1] 
start = int(enc[:2], 16) - 1 
enc = unhexlify(enc[2:]) 
key = static_key[start:] + static_key[:start] 
 
plain = ''.join([chr(x ^ ord(key[i % len(key)]))  for i, x in enumerate(enc)]) 
print(plain) 
```

### Perl

```text
#!/usr/bin/perl 
use File::Copy; 
############################################################################ 
# Vigenere translation table 
############################################################################ 
@V=(0x64, 0x73, 0x66, 0x64, 0x3b, 0x6b, 0x66, 0x6f, 0x41, 0x2c, 0x2e, 
    0x69, 0x79, 0x65, 0x77, 0x72, 0x6b, 0x6c, 0x64, 0x4a, 0x4b, 0x44, 
    0x48, 0x53, 0x55, 0x42, 0x73, 0x67, 0x76, 0x63, 0x61, 0x36, 0x39, 
    0x38, 0x33, 0x34, 0x6e, 0x63, 0x78, 0x76, 0x39, 0x38, 0x37, 0x33, 
    0x32, 0x35, 0x34, 0x6b, 0x3b, 0x66, 0x67, 0x38, 0x37); 
############################################################################ 
############################################################################ 
# Usage guidelines 
############################################################################ 
if ($ARGV[0] eq ""){ 
   print "This script reveals the IOS passwords obfuscated using the Vigenere algorithm.n"; 
   print "n"; 
   print "Usage guidelines:n"; 
   print " cdecrypt.pl 04480E051A33490E     # Reveals a single passwordn"; 
   print " cdecrypt.pl running-config.rcf   # Changes all passwords in a file to cleartextn"; 
   print "                                  # Original file stored with .bak extensionn"; 
} 
############################################################################ 
# Process arguments and execute 
############################################################################ 
if(open(F,"<$ARGV[0]")){    # If argument passed can be opened then convert a file 
  open(FO,">cdcout.rcf") || die("Cannot open 'cdcout.rcf' for writing ($!)n"); 
  while(<F>){ 
    if (/(.*passwords)(7s)([0-9a-fA-F]{4,})/){     # Find password commands 
      my $d=Decrypt($3);                             # Deobfuscate passwords 
      s/(.*passwords)(7s)([0-9a-fA-F]{4,})/$1$d/;  # Remove '7' and add cleartext password 
    } 
    print FO $_; 
  } 
  close(F); 
  close(FO); 
  copy($ARGV[0],"$ARGV[0].bak")||die("Cannot copy '$ARGV[0]' to '$ARGV[0].bak'"); 
  copy("cdcout.rcf",$ARGV[0])||die("Cannot copy '$ARGV[0]' to '$ARGV[0].bak'"); 
  unlink "cdcout.rcf"; 
}else{                      # If argument passed cannot be opened it is a single password 
  print Decrypt($ARGV[0]) . "\n"; 
} 
############################################################################ 
# Vigenere decryption/deobfuscation function 
############################################################################ 
sub Decrypt{ 
  my $pw=shift(@_);                             # Retrieve input obfuscated password 
  my $i=substr($pw,0,2);                        # Initial index into Vigenere translation table 
  my $c=2;                                      # Initial pointer 
  my $r="";                                     # Variable to hold cleartext password 
  while ($c<length($pw)){                       # Process each pair of hex values 
    $r.=chr(hex(substr($pw,$c,2))^$V[$i++]);    # Vigenere reverse translation 
    $c+=2;                                      # Move pointer to next hex pair 
    $i%=53;                                     # Vigenere table wrap around 
  }                                             # 
  return $r;                                    # Return cleartext password 
} 
```

###  Decrypt 

```text
root@Kali:/opt/decrypt-cisco-7# perl decrypt.pl 04480E051A33490E 
secure  
```

**Command to encrypt:**

```text
ena password PASSWORD
service password-encryption
```

## Type 8

Encrypted using PBKDF2-SHA-256 with 10 character salt \(80 bits\).

starting from IOS 15.3\(3\) - really strong

```text
R1(config)#enable algorithm-type sha256 secret cisco
R1(config)#do sh run | i enable
enable secret 8 $8$mTj4RZG8N9ZDOk$elY/asfm8kD3iDmkBe3hD2r4xcA/0oWS5V3os.O91u.
```

John the Ripper recognizes this password type as **pbkdf2-hmac-sha256**. To crack it, we have to again first convert it to the following john friendly format and save it in a file:

```text
admin:$8$dsYGNam3K1SIJO$7nv/35M/qr6t.dVc7UY9zrJDWRVqncHub1PE9UlMQFs
```

Then we can crack it like this using a dictionary, for example:

```text
john --format=pbkdf2-hmac-sha256 hashes.txt
```

## Type 9

Encrypted using scrypt \(very strong\) starting from IOS 15.3\(3\)

Example:

```text
R1(config)#ena algorithm-type scrypt secret cisco
R1(config)#do sh run | i enable
enable secret 9 $9$WnArItcQHW/uuE$x5WTLbu7PbzGDuv0fSwGKS/KURsy5a3WCQckmJp0MbE
```

**Cracking using john:**

```text
john --format=scrypt hashes.txt
```

## **Sources**

{% embed url="https://www.infosecmatter.com/cisco-password-cracking-and-decrypting-guide/" %}

{% embed url="https://community.cisco.com/t5/networking-documents/understanding-the-differences-between-the-cisco-password-secret/ta-p/3163238" %}

{% embed url="https://learningnetwork.cisco.com/s/article/cisco-routers-password-types" %}



