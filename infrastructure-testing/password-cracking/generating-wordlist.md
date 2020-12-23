# Generating wordlist

### CeWL 

From a web use CeWL 

[https://github.com/digininja/CeWL](https://github.com/digininja/CeWL)

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

script: [https://github.com/nyxgeek/weakpass\_generator](https://github.com/nyxgeek/weakpass_generator)

automating output:

[http://www.weakpasswords.net/](http://www.weakpasswords.net/)

