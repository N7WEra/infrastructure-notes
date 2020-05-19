---
description: >-
  John (aka John the Ripper) is a fast password cracker, currently available for
  many flavors of Unix, macOS, Windows, DOS, BeOS, and OpenVMS
---

# John

**Link**: [https://github.com/magnumripper/JohnTheRipper](https://github.com/magnumripper/JohnTheRipper)

## **Simple usage**

**JTR password cracking** 

`john --wordlist=/usr/share/wordlists/rockyou.txt hashes` 

**JTR forced descrypt cracking with wordlist** 

`john --format=descrypt --wordlist /usr/share/wordlists/rockyou.txt hash.txt` 

**JTR forced descrypt brute force cracking** 

`john --format=descrypt hash --show` 

**Display formats:** 

`john --list=formats` 

## **Type and mask:** 

`iron@kali2:/tmp$ sudo john lm.txt --mask=?l?l?l?l --format=lm` 

### **mask** 

Create a mask: 

example: 

```text
root@attackdefense:~# john pdfhash --mask=?d?d?d?d?d?d?d?d?l 
?d = digit 
?l = lower-case ASCII letters 
?u = upper-case ASCII letters 
```

**example with numbers in the middle:** 

```text
root@attackdefense:~# john pdfhash --mask=?d?d?d?d19?d?d?u 
Using default input encoding: UTF-8 
Loaded 1 password hash (PDF [MD5 SHA2 RC4/AES 32/64]) 
Press 'q' or Ctrl-C to abort, almost any other key for status 
01021980D        (/root/encrypted.pdf) 
1g 0:00:00:05 DONE (2019-10-31 10:10) 0.1721g/s 530466p/s 530466c/s 530466C/s 01021980D 
Use the "--show" option to display all of the cracked passwords reliably 
Session completed 
```

A mask may consist of: 

```text
- Static letters. 
- Ranges in [aouei] or [a-z] syntax. Or both, [0-9abcdef] is the same as 
     [0-9a-f]. 
- Placeholders that are just a short form for ranges, like ?l which is 
     100% equivalent to [a-z]. 
- ?l lower-case ASCII letters 
- ?u upper-case ASCII letters 
- ?d digits 
- ?s specials (all printable ASCII characters not in ?l, ?u or ?d) 
- ?a full 'printable' ASCII. Note that for formats that don't recognize case 
     (eg. LM), this only includes lower-case characters which is a tremendous 
     reduction of keyspace for the win. 
- ?B all 8-bit (0x80-0xff) 
- ?b all (0x01-0xff) (the NULL character is currently not supported by core). 
- ?h lower-case HEX digits (0-9, a-f) 
- ?H upper-case HEX digits (0-9, A-F) 
- ?L lower-case non-ASCII letters 
- ?U upper-case non-ASCII letters 
- ?D non-ASCII "digits" 
- ?S non-ASCII "specials" 
- ?A all valid characters in the current code page (including ASCII). Note 
     that for formats that don't recognize case (eg. LM), this only includes 
     lower-case characters which is a tremendous reduction of keyspace. 
- Placeholders that are custom defined, so we can e.g. define ?1 to mean [?u?l] 
  ?1 .. ?9 user-defined place-holder 1 .. 9 
 Placeholders for Hybrid Mask mode: 
  ?w is a placeholder for the original word produced by the parent mode in 
     Hybrid Mask mode. 
  ?W is just like ?w except the original word is case toggled (so PassWord 
     becomes pASSwORD). 
```

## Windows 

`C:\Users\David\Documents\Tools\john-1.9.0-jumbo-1-win64\run>john.exe ..\test.txt --format=raw-MD5` 

## Formats

### Common formats: 

| Type  | John Format  | Hash Example  |
| :--- | :--- | :--- |
| MD5  | raw-md5  | fc16ea469c37da07bac3ddbbdbfb3945  |
| LM  | lm  | 299BD128C1101FD6  |
| NTLM  | nt  | B4B9B02E6F09A9BD760F388B67351E2B  |
| NTLMv1  | netntlm  | netntlm::kNS:338d08f8e26de93300000000000000000000000000000000:9526fb8c23a90751cdd619b6cea564742e1e4bf33006ba41:cb8086049ec4736c  |
| NTLMv2  | netntlmv2  | admin::N46iSNekpT:08ca45b7d7ea58ee:88dcbe4446168966a153a0064958dac6:5c7830315c7830310000000000000b45c67103d07d7b95acd12ffa11230e0000000052920b85f78d013c31cdb3b92f5d765c783030  |
| Cisco Type 5  | Md5crpy  | enable\_secret\_level\_2:$1$WhZT$YYEI3f0wwWJGAXtAayK/Q.  |

### All Formats: 

```text
descrypt, bsdicrypt, md5crypt, md5crypt-long, bcrypt, scrypt, LM, AFS,  
tripcode, AndroidBackup, adxcrypt, agilekeychain, aix-ssha1, aix-ssha256,  
aix-ssha512, andOTP, ansible, argon2, as400-des, as400-ssha1, asa-md5,  
AxCrypt, AzureAD, BestCrypt, bfegg, Bitcoin, BitLocker, bitshares, Bitwarden,  
BKS, Blackberry-ES10, WoWSRP, Blockchain, chap, Clipperz, cloudkeychain,  
dynamic_n, cq, CRC32, sha1crypt, sha256crypt, sha512crypt, Citrix_NS10,  
dahua, dashlane, diskcryptor, Django, django-scrypt, dmd5, dmg, dominosec,  
dominosec8, DPAPImk, dragonfly3-32, dragonfly3-64, dragonfly4-32,  
dragonfly4-64, Drupal7, eCryptfs, eigrp, electrum, EncFS, enpass, EPI,  
EPiServer, ethereum, fde, Fortigate256, Fortigate, FormSpring, FVDE, geli,  
gost, gpg, HAVAL-128-4, HAVAL-256-3, hdaa, hMailServer, hsrp, IKE, ipb2,  
itunes-backup, iwork, KeePass, keychain, keyring, keystore, known_hosts,  
krb4, krb5, krb5asrep, krb5pa-sha1, krb5tgs, krb5-17, krb5-18, krb5-3,  
kwallet, lp, lpcli, leet, lotus5, lotus85, LUKS, MD2, mdc2, MediaWiki,  
monero, money, MongoDB, scram, Mozilla, mscash, mscash2, MSCHAPv2,  
mschapv2-naive, krb5pa-md5, mssql, mssql05, mssql12, multibit, mysqlna,  
mysql-sha1, mysql, net-ah, nethalflm, netlm, netlmv2, net-md5, netntlmv2,  
netntlm, netntlm-naive, net-sha1, nk, notes, md5ns, nsec3, NT, o10glogon,  
o3logon, o5logon, ODF, Office, oldoffice, OpenBSD-SoftRAID, openssl-enc,  
oracle, oracle11, Oracle12C, osc, ospf, Padlock, Palshop, Panama,  
PBKDF2-HMAC-MD4, PBKDF2-HMAC-MD5, PBKDF2-HMAC-SHA1, PBKDF2-HMAC-SHA256,  
PBKDF2-HMAC-SHA512, PDF, PEM, pfx, pgpdisk, pgpsda, pgpwde, phpass, PHPS,  
PHPS2, pix-md5, PKZIP, po, postgres, PST, PuTTY, pwsafe, qnx, RACF,  
RACF-KDFAES, radius, RAdmin, RAKP, rar, RAR5, Raw-SHA512, Raw-Blake2,  
Raw-Keccak, Raw-Keccak-256, Raw-MD4, Raw-MD5, Raw-MD5u, Raw-SHA1,  
Raw-SHA1-AxCrypt, Raw-SHA1-Linkedin, Raw-SHA224, Raw-SHA256, Raw-SHA3,  
Raw-SHA384, ripemd-128, ripemd-160, rsvp, Siemens-S7, Salted-SHA1, SSHA512,  
sapb, sapg, saph, sappse, securezip, 7z, Signal, SIP, skein-256, skein-512,  
skey, SL3, Snefru-128, Snefru-256, LastPass, SNMP, solarwinds, SSH, sspr,  
Stribog-256, Stribog-512, STRIP, SunMD5, SybaseASE, Sybase-PROP, tacacs-plus,  
tcp-md5, telegram, tezos, Tiger, tc_aes_xts, tc_ripemd160, tc_ripemd160boot,  
tc_sha512, tc_whirlpool, vdi, OpenVMS, vmx, VNC, vtp, wbb3, whirlpool,  
whirlpool0, whirlpool1, wpapsk, wpapsk-pmk, xmpp-scram, xsha, xsha512, ZIP,  
ZipMonster, plaintext, has-160, HMAC-MD5, HMAC-SHA1, HMAC-SHA224,  
HMAC-SHA256, HMAC-SHA384, HMAC-SHA512, dummy, crypt 
```

## Cracking examples

### Cracking /etc/shadow

```text
sudo /usr/sbin/unshadow /etc/passwd /etc/shadow > /tmp/crack.password.db 
john /tmp/crack.password.db 
Loaded 2 password hashes with 2 different salts (sha512crypt, crypt(3) $6$ [SHA512 256/256 AVX2 4x]) 
```

### Cracking pdf protected password

```text
pdf2john encrypted.pdf >> hash 
john hash --mask=?d?d?d?d?d?d?d?d?l 
pdftotext -upw PASSWORD encrypted.pdf 
```

### Cracking ssh key

```text
root@attackdefense:~# ssh2john id_rsa >> hash 
root@attackdefense:~# cat hash 
id_rsa:$ssh2$2d2d2d2d2d424547494e205253412050524956415445204b45592d2d2d2d2d0a50726f632d547970653a20342c454e435259505445440a44454b2d496e666f3a204145532d3132382d4342432c33333746334342463743413243453338413837383243464632453735423446440a0a4843694443447343745856372b2b49683178544971376b73467a375549517858796f45754f6d395a43516d416541736a7961535973634e42654b536974526d490a41423947586a7a7a796f336249584f47706e2f46695535312b784b4c45575678484261797856424e502f4d2b7237306b374869594766556571573064324c2b630a3749705633505865656e2b77455831314963316859683265696179716b6b384572626c6579616e386f63714d345331486d456642744d39356b395135714535310a5a6233686e585853304a44586774467134545147395538336b764b59596a5849546f4849474f34746e4f30754a4f57653670634b334e62517a47316a696c55350a774d5279545a796b2b36696a524f516565685a374b6c68644a373339446e494a5169675a3669706837585a666f65467a6e6533626975434f79514d76677349790a476b7750514a6b65454b2b6a424f6573536b592b6850546242576a3738316b553576436b5477666b64546b3979726d526a615433586375346532636d51456a430a6d6679375154773152475666595769505933304b64695841366e3652594550355149566544352b54363356473636357030734a45717952366479616f35436d480a78552f65586c47326f7a3135616c69647a373268776a4a454d706a4a374369727075667135355466527231465570365a535a325573754b7355503958707a4c670a62547a3846796c50434c62426942795665696d434d4a75456b6d6a2b4731615242414a2b444f626c54544d393150594f5245582b2b6744466c6b504230564e750a6d77412b32635a466b675537524a6b683653354e773141793957774b48454a2f6c754f4942555a4d4b586439316d56514c6853794755385146316f663861794b0a36696a6871315a6253353067427036556f2b746d3075515776444a596d417a764b4b717571414f533633693378666a684171626e626b4259527462316b73597a0a745a6d356f744a4f5668386d4335664663636f4c69743049645948773868563847696454754844546c69344c6164504a4c3166494a3777555552467a434667710a74357743486467664a447832794d785131754c4e65515364614742573758656a6355446f303669314a6a2f39586c734b537556574766524c6e6155636b41364b0a666a4b6f6f3877394c6842474f464653737257735533453543482b613674724655356761594d384a4f366132576b6c714155394f6f436548324f506854656d6f0a797a704e4344323744384543786e3474335049726a4c697a4b717245536e77595077323731725a47752f2b634c38554a4c6d46316234782b6f4e2b67616741350a694769544a54586470302f706d372f4a59376977344f71592f5675793248317559554d5357444367586f45766479326a64503653696f564d485654484f6c72370a6369643762322b546877697578326e304f393855684d563832434e5639527438364b6774774f322f4c37694c54417459487a4a75442f4a48474634543630446e0a766977755068627339774d2b326f2f6b6f6631503932716d376c614e355634426f78706c7174303273776c4d43346279683778657475346b633952686b4733510a366970726f6c6e465a4c712b4867546a47626a6751624c514734784372616470626870774f43427876547667434a5075592f487578393874725244535845744c0a586f76486b4a48482f452f762b495768734c7637784a724c396d575068646a507157654d4c3536634e537a34394e7653624e593647656c4b786d3566517772380a75334c4e6e7236363030763069774a7461323467506e766e71572b6c517176777a36396f45502b65653252756734717369433433784d304233316f584d5363510a4539684c345a5a5a6e4d666642377a2f704239644d2f4b5755325477746c736762576d6d6f4a34674e50544c455372344959707141546a3451634c72616b6d610a765048684e6b504a4d77544a78487431623159384b42652b486c796273364e325430695171636f714443373070635a754c4b447037532f6c325553536776522f0a4a7a474355704532584d6b4a336a2f4371724f483143785477594b525a4445432b2b70564576582f65563765707647566e74614d48697032776f52536378654d0a2f597267515830536a4f573261774667575347564537764e383167544a567462756841736b754e657a5355734a4d71745848456473435677654256766d61546b0a2d2d2d2d2d454e44205253412050524956415445204b45592d2d2d2d2d0a*1766*0 
```

### MD5 wordlist

```text
root@attackdefense:~# for x in $(cat wordlists/100-common-passwords.txt); do echo -n $x | md5sum >> wordlist.txt; done 
root@attackdefense:~# cat wordlist.txt | cut -d' ' -f1 >> new 
root@attackdefense:~# john hash --wordlist=new 
```





## John cheat sheet

[https://countuponsecurity.files.wordpress.com/2016/09/jtr-cheat-sheet.pdf](https://countuponsecurity.files.wordpress.com/2016/09/jtr-cheat-sheet.pdf) 

