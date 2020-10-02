# FreeBSD

view installed software:

`pkg_info` or `pkg info`

view passwords:

`vipw` or `cat /etc/master.passwd`

View listening ports:

`netstat -an -p tcp`

view services:

`ps aux`

Shows operating system info

`uname -a`

search files for password:

`grep -ri 'password' *`

cracking passwords

```text
sudo john master.passwd --wordlist=pass 
Using default input encoding: UTF-8
Loaded 1 password hash (sha512crypt, crypt(3) $6$ [SHA512 256/256 AVX2 4x])
Cost 1 (iteration count) is 5000 for all loaded hashes
Press 'q' or Ctrl-C to abort, almost any other key for status
password          (iron)
1g 0:00:00:00 DONE (2020-10-02 11:35) 25.00g/s 100.0p/s 100.0c/s 100.0C/s iron..577
Use the "--show" option to display all of the cracked passwords reliably
Session completed

```

