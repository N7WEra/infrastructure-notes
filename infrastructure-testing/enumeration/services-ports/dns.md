---
description: >-
  The Domain Name System is a hierarchical and decentralized naming system for
  computers, services, or other resources connected to the Internet or a private
  network.
---

# 53 - DNS

### nslookup&#x20;

```
SERVER {DNS Server} 
{IP we want to check}
```

![](https://firebasestorage.googleapis.com/v0/b/gitbook-x-prod.appspot.com/o/spaces%2F-M4xwp6Mq18nX8yR4M5z%2Fuploads%2FXfXX780GixP7pcq16eRc%2Ffile.jpeg?alt=media)

### Records lookups&#x20;

```
dig a domainname.com @nameserver 
dig mx domainname.com @nameserver 
```

### **Find name server (NS)**&#x20;

```
root@Kali:~# dig ns zonetransfer.me 
[snip] 
;; ANSWER SECTION: 
zonetransfer.me. 7186 IN NS nsztm2.digi.ninja. 
zonetransfer.me. 7186 IN NS nsztm1.digi.ninja. 
```

### Dnsrecon&#x20;

`Dnsrecon.py -d {domain}`&#x20;

Link: [https://github.com/darkoperator/dnsrecon](https://github.com/darkoperator/dnsrecon)

**Reverse lookup:**&#x20;

`./dnsrecon.py -r <startIP-endIP>`&#x20;

### Dig

view all dns records

`dig zonetransfer.me -t ANY`

### Zone transfer

Using dig first find NS Server::

```
First find name server 

root@Kali:~# dig ns zonetransfer.me 

; <<>> DiG 9.11.5-P4-5.1+b1-Debian <<>> ns zonetransfer.me 
;; global options: +cmd 
;; Got answer: 
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 11380 
;; flags: qr rd ra; QUERY: 1, ANSWER: 2, AUTHORITY: 0, ADDITIONAL: 1 

;; OPT PSEUDOSECTION: 
; EDNS: version: 0, flags:; udp: 4096 
;; QUESTION SECTION: 
;zonetransfer.me. IN NS 

;; ANSWER SECTION: 
zonetransfer.me. 7186 IN NS nsztm2.digi.ninja. 
zonetransfer.me. 7186 IN NS nsztm1.digi.ninja. 
 
;; Query time: 22 msec 
;; SERVER: 192.168.0.1#53(192.168.0.1) 
;; WHEN: Tue Nov 12 08:29:13 GMT 2019 
;; MSG SIZE  rcvd: 96 


 


```

#### **Perform zone transfer:**

```
root@kali:~/$ dig axfr @nsztm1.digi.ninja zonetransfer.me

; <<>> DiG 9.16.3-Debian <<>> axfr @nsztm1.digi.ninja zonetransfer.me
; (1 server found)
;; global options: +cmd
zonetransfer.me.	7200	IN	SOA	nsztm1.digi.ninja. robin.digi.ninja. 2019100801 172800 900 1209600 3600
zonetransfer.me.	300	IN	HINFO	"Casio fx-700G" "Windows XP"
zonetransfer.me.	301	IN	TXT	"google-site-verification=tyP28J7JAUHA9fw2sHXMgcCC0I6XBmmoVi04VlMewxA"
zonetransfer.me.	7200	IN	MX	0 ASPMX.L.GOOGLE.COM.
zonetransfer.me.	7200	IN	MX	10 ALT1.ASPMX.L.GOOGLE.COM.
zonetransfer.me.	7200	IN	MX	10 ALT2.ASPMX.L.GOOGLE.COM.
zonetransfer.me.	7200	IN	MX	20 ASPMX2.GOOGLEMAIL.COM.
zonetransfer.me.	7200	IN	MX	20 ASPMX3.GOOGLEMAIL.COM.
zonetransfer.me.	7200	IN	MX	20 ASPMX4.GOOGLEMAIL.COM.
zonetransfer.me.	7200	IN	MX	20 ASPMX5.GOOGLEMAIL.COM.
zonetransfer.me.	7200	IN	A	5.196.105.14
zonetransfer.me.	7200	IN	NS	nsztm1.digi.ninja.
zonetransfer.me.	7200	IN	NS	nsztm2.digi.ninja.
_acme-challenge.zonetransfer.me. 301 IN	TXT	"6Oa05hbUJ9xSsvYy7pApQvwCUSSGgxvrbdizjePEsZI"
_sip._tcp.zonetransfer.me. 14000 IN	SRV	0 0 5060 www.zonetransfer.me.
14.105.196.5.IN-ADDR.ARPA.zonetransfer.me. 7200	IN PTR www.zonetransfer.me.
asfdbauthdns.zonetransfer.me. 7900 IN	AFSDB	1 asfdbbox.zonetransfer.me.
asfdbbox.zonetransfer.me. 7200	IN	A	127.0.0.1
asfdbvolume.zonetransfer.me. 7800 IN	AFSDB	1 asfdbbox.zonetransfer.me.
canberra-office.zonetransfer.me. 7200 IN A	202.14.81.230
cmdexec.zonetransfer.me. 300	IN	TXT	"; ls"
contact.zonetransfer.me. 2592000 IN	TXT	"Remember to call or email Pippa on +44 123 4567890 or pippa@zonetransfer.me when making DNS changes"
dc-office.zonetransfer.me. 7200	IN	A	143.228.181.132
deadbeef.zonetransfer.me. 7201	IN	AAAA	dead:beaf::
dr.zonetransfer.me.	300	IN	LOC	53 20 56.558 N 1 38 33.526 W 0.00m 1m 10000m 10m
DZC.zonetransfer.me.	7200	IN	TXT	"AbCdEfG"
email.zonetransfer.me.	2222	IN	NAPTR	1 1 "P" "E2U+email" "" email.zonetransfer.me.zonetransfer.me.
email.zonetransfer.me.	7200	IN	A	74.125.206.26
Hello.zonetransfer.me.	7200	IN	TXT	"Hi to Josh and all his class"
home.zonetransfer.me.	7200	IN	A	127.0.0.1
Info.zonetransfer.me.	7200	IN	TXT	"ZoneTransfer.me service provided by Robin Wood - robin@digi.ninja. See http://digi.ninja/projects/zonetransferme.php for more information."
internal.zonetransfer.me. 300	IN	NS	intns1.zonetransfer.me.
internal.zonetransfer.me. 300	IN	NS	intns2.zonetransfer.me.
intns1.zonetransfer.me.	300	IN	A	81.4.108.41
intns2.zonetransfer.me.	300	IN	A	167.88.42.94
office.zonetransfer.me.	7200	IN	A	4.23.39.254
ipv6actnow.org.zonetransfer.me.	7200 IN	AAAA	2001:67c:2e8:11::c100:1332
owa.zonetransfer.me.	7200	IN	A	207.46.197.32
robinwood.zonetransfer.me. 302	IN	TXT	"Robin Wood"
rp.zonetransfer.me.	321	IN	RP	robin.zonetransfer.me. robinwood.zonetransfer.me.
sip.zonetransfer.me.	3333	IN	NAPTR	2 3 "P" "E2U+sip" "!^.*$!sip:customer-service@zonetransfer.me!" .
sqli.zonetransfer.me.	300	IN	TXT	"' or 1=1 --"
sshock.zonetransfer.me.	7200	IN	TXT	"() { :]}; echo ShellShocked"
staging.zonetransfer.me. 7200	IN	CNAME	www.sydneyoperahouse.com.
alltcpportsopen.firewall.test.zonetransfer.me. 301 IN A	127.0.0.1
testing.zonetransfer.me. 301	IN	CNAME	www.zonetransfer.me.
vpn.zonetransfer.me.	4000	IN	A	174.36.59.154
www.zonetransfer.me.	7200	IN	A	5.196.105.14
xss.zonetransfer.me.	300	IN	TXT	"'><script>alert('Boo')</script>"
zonetransfer.me.	7200	IN	SOA	nsztm1.digi.ninja. robin.digi.ninja. 2019100801 172800 900 1209600 3600
;; Query time: 28 msec
;; SERVER: 81.4.108.41#53(81.4.108.41)
;; WHEN: Thu Jun 25 13:53:32 BST 2020
;; XFR size: 50 records (messages 1, bytes 1994)

```

#### **Using nmap**

`nmap --script dns-zone-transfer.nse --script-args dns-zone-transfer.domain=zonetrasnfer.me -p53`&#x20;

&#x20;Using dnsrecon

```
dnsrecon -d zonetransfer.me -a 
```
