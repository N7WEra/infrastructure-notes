# Generating wordlist

## Types of wordlist

There 5 types of wordlists

1. Weak common Password \(e.g. rockyou.txt,  [darkweb2017-top100.txt](https://github.com/danielmiessler/SecLists/blob/master/Passwords/darkweb2017-top100.txt) and etc\)
2. Scrapped wordlist - scrape a website for words that can be used as password \(tool - CeWL\)
3. Generated words - generate a common pattern words \(e.g. aaaa, bbbb , cccc\) \(tool - crunch\)
4. Generate keyboard walks \(tool - kwprocessor\)
5. Wordlists based on current year / season \(e.g. Summer2020 , Winter2019 and etc\) \(tool - weakpass\_generator\)

### CeWL 

CeWL - Custom Word List generator

Creating custom word lists spidering a targets website and collecting unique words.

GitHub: [https://github.com/digininja/CeWL](https://github.com/digininja/CeWL)

#### **Usage**:

```text
└─$ cewl https://digi.ninja/
CeWL 5.4.8 (Inclusion) Robin Wood (robin@digi.ninja) (https://digi.ninja/)
the
and
you
with
this
for
that
Share
The
but
close
can
ninja
get
are
was
from
have
all
site
they
[--sniped--]
```

### Weak Passwords

SecList-  [https://github.com/danielmiessler/SecLists/tree/master/Passwords](https://github.com/danielmiessler/SecLists/tree/master/Passwords)

### Crunch 

crunch enables us to create a custom password-cracking wordlist that we can use with such tools like Hashcat, Cain and Abel, John the Ripper, Aircrack-ng, and others. This custom wordlist might be able to save us hours or days in password cracking if we can craft it properly. 

Syntax: 

`kali > crunch <min> max<max> <characterset> -t <pattern> -o <output filename>` 

Example: 

`crunch 4 4 -f /usr/share/crunch/charset.lst lalpha-numeric -o wordlist.txt` 

We could generate all the possibilities of ten-character passwords that end with 0728 and send the output to a file in the root user's directory named birthdaywordlist.lst, by typing: 

`crunch 10 10 -t @@@@@@0728 -o /root/birthdaywordlist.lst` 

### kwprcessor

Advanced keyboard-walk generator with configureable basechars, keymap and routes

Link: [https://github.com/hashcat/kwprocessor](https://github.com/hashcat/kwprocessor)

Example:

`./kwp basechars/full.base keymaps/en-us.keymap routes/2-to-16-max-3-direction-changes.route > wordlist.txt`

### weakpass generator

script: [https://github.com/nyxgeek/weakpass\_generator](https://github.com/nyxgeek/weakpass_generator) or online version [http://www.weakpasswords.net/](http://www.weakpasswords.net/)

#### Usage:

```text
─$ python3 weakpass_generator.py 
Here are the results:
Christmas1
Christmas123
Christmas20
Christmas20!
Christmas2020
Christmas2020!
Christmas@20
Christmas@2020
December1
December123
December20
December20!
December2020
December2020!
December@20
December@2020
February1
February123
February2021
February2021!
February21
February21!
February@2021
February@21
January1

```



