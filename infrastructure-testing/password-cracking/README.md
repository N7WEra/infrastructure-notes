# Password Cracking

## Identify the hash \(By Length\)

**Hash Lengths** 

| Hash  | Size  |
| :--- | :--- |
| MD5 Hash Length  | 16 Bytes  |
| SHA-1 Hash Length  | 20 Bytes  |
| SHA-256 Hash Length  | 32 Bytes  |
| SHA-512 Hash Length  | 64 Bytes  |

## **Tools To identify hashes**

### **hash-identifier**

Builtin in Kali.

Usage: `hash-identifier`

### **Hash-Buster**

Link: [https://github.com/s0md3v/Hash-Buster](https://github.com/s0md3v/Hash-Buster)

**Usage:** `buster -s <hash>`

### Decodify

It can detect and decode encoded strings, recursively.

Link: [https://github.com/s0md3v/Decodify](https://github.com/s0md3v/Decodify)

## **Empty hashes** 

Administrator:500:aad3b435b51404eeaad3b435b51404ee:8118cb8789b3a147c790db402b016a08::: 

Administrator = user 

500 = RID 

Aad3b435b51404eeaad3b435b51404ee = empty LM hash 

## Hash Examples 

Likely just use hash-identifier for this but here are some example hashes: 

<table>
  <thead>
    <tr>
      <th style="text-align:left">Hash</th>
      <th style="text-align:left">Example</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">MD5 Hash Example</td>
      <td style="text-align:left">8743b52063cd84097a65d1633f5c74f5</td>
    </tr>
    <tr>
      <td style="text-align:left">MD5 $PASS:$SALT Example</td>
      <td style="text-align:left">01dfae6e5d4d90d9892622325959afbe:7050461</td>
    </tr>
    <tr>
      <td style="text-align:left">MD5 $SALT:$PASS</td>
      <td style="text-align:left">f0fda58630310a6dd91a7d8f0a4ceda2:4225637426</td>
    </tr>
    <tr>
      <td style="text-align:left">SHA1 Hash Example</td>
      <td style="text-align:left">b89eaac7e61417341b710b727768294d0e6a277b</td>
    </tr>
    <tr>
      <td style="text-align:left">SHA1 $PASS:$SALT</td>
      <td style="text-align:left">2fc5a684737ce1bf7b3b239df432416e0dd07357:2014</td>
    </tr>
    <tr>
      <td style="text-align:left">SHA1 $SALT:$PASS</td>
      <td style="text-align:left">cac35ec206d868b7d7cb0b55f31d9425b075082b:5363620024</td>
    </tr>
    <tr>
      <td style="text-align:left">SHA-256</td>
      <td style="text-align:left">
        <p>127e6fbfe24a750e72930c220a8e138275656b</p>
        <p>8e5d8f48a98c3c92df2caba935</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">SHA-256 $PASS:$SALT</td>
      <td style="text-align:left">c73d08de890479518ed60cf670d17faa26a4a71f995c1dcc978165399401a6c4</td>
    </tr>
    <tr>
      <td style="text-align:left">SHA-256 $SALT:$PASS</td>
      <td style="text-align:left">eb368a2dfd38b405f014118c7d9747fcc97f4f0ee75c05963cd9da6ee65ef498:560407001617</td>
    </tr>
    <tr>
      <td style="text-align:left">SHA-512</td>
      <td style="text-align:left">82a9dda829eb7f8ffe9fbe49e45d47d2dad9664fbb7adf72492e3c81ebd3e29134d9bc12212bf83c6840f10e8246b9db54a4859b7ccd0123d86e5872c1e5082f</td>
    </tr>
    <tr>
      <td style="text-align:left">SHA-512 $PASS:$SALT</td>
      <td style="text-align:left">e5c3ede3e49fb86592fb03f471c35ba13e8d89b8ab65142c9a8fdafb635fa2223c24e5558fd9313e8995019dcbec1fb584146b7bb12685c7765fc8c0d51379fd</td>
    </tr>
    <tr>
      <td style="text-align:left">SHA-512 $SALT:$PASS</td>
      <td style="text-align:left">976b451818634a1e2acba682da3fd6efa72adf8a7a08d7939550c244b237c72c7d42367544e826c0c83fe5c02f97c0373b6b1386cc794bf0d21d2df01bb9c08a</td>
    </tr>
    <tr>
      <td style="text-align:left">NTLM Hash Example</td>
      <td style="text-align:left">b4b9b02e6f09a9bd760f388b67351e2b</td>
    </tr>
  </tbody>
</table>## Generating wordlists

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

