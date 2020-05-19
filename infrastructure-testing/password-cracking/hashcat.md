---
description: Advanced password recovery
---

# Hashcat

## Basics

**Installation:** 

You can download Hashcat from [https://hashcat.net/hashcat/](https://hashcat.net/hashcat/). 

**Example:** 

`hashcat64.exe -a 0 -m 0 example0.hash example.dict -r rules/best64.rule` 

Dictionary Attack: 

`hashcat64.exe -a 0 -m 0 example_md5_hashes.txt combined_seclists_password_list.txt -O` 

Note: Consider using `-w 4` for workload optimized 

## Mask attack 

```text
?l = abcdefghijklmnopqrstuvwxyz 
?u = ABCDEFGHIJKLMNOPQRSTUVWXYZ 
?d = 0123456789 
?h = 0123456789abcdef 
?H = 0123456789ABCDEF 
?s = «space»!"#$%&'()*+,-./:;<=>?@[\]^_`{|}~ 
?a = ?l?u?d?s 
?b = 0x00 - 0xff 
```

example: 

`root@attackdefense:~# hashcat -m 10400 -a 3 pdfnew ?d?d?d?d198?d?u --force` 

## Using different charset: 

hashcat -m 0 -a 3 -1 ?l?d hash2 ?1?1?1?1 --force 

* -m 0 = hash type = md5 
* -a 3 = mode = brute-force 
* -1 ?l?d= -1  charset  = use charset of ?l \(lower alpha\) and ?d numbers 
* Hash2 = hash file  
* ?1?1?1?1 = the length of the password, use the charset we created  \(-1\) 
* --force = use cpu 

## Hash examples

[https://hashcat.net/wiki/doku.php?id=example\_hashes](https://hashcat.net/wiki/doku.php?id=example_hashes)

