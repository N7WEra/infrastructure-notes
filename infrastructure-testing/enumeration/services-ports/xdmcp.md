# 177 - XDMCP

**Scan:**

```text
root@kali:~# nmap -sU 192.168.0.42 
Starting Nmap 7.80 ( https://nmap.org ) at 2020-01-22 14:57 GMT 
Nmap scan report for 192.168.0.42 
Host is up (0.00065s latency). 
Not shown: 999 open|filtered ports 
PORT    STATE SERVICE 
177/udp open  xdmcp 

Nmap done: 1 IP address (1 host up) scanned in 4.27 seconds 
```

### **Nmap:**

```text
nmap -sU -p 177 --script xdmcp-discover <ip> 

PORT    STATE         SERVICE 
177/udp open|filtered xdmcp 
| xdmcp-discover: 
|   Session id: 0x0000703E 
|   Authorization name: MIT-MAGIC-COOKIE-1 
|_  Authorization data: c282137c9bf8e2af88879e6eaa922326 
```

### Xepher

[https://doc.rogerwhittaker.org.uk/Xephyr/](https://doc.rogerwhittaker.org.uk/Xephyr/) 

`Xephyr -from 192.168.0.42 -query 192.168.0.41 -screen 800x600 :1` 

Note: Make sure you add '-from', if not added the XDMCP won't know where to send the screen back to.

### Remmina

install xdmcp support: 

`Apt install remmina-plugin-xdmcp` 

Press 'new connection profile' and change protocol to XDMCP

### Xnest 

Install:

`apt-get install xnest` 

Run:

`Xnest -ac -query ip_address :1 -geometry 1024x768` 

### **Enable XDMCP on Ubuntu host**

Ubuntu 12.04 LTS uses lightdm by default. XDMCP is disabled by default but can be enabled by adding the following to "/etc/lightdm/lightdm.conf":

```text
[XDMCPServer] 
enabled=true 


```

Restart lightdm after this:

`sudo restart lightdm`

and execute:

xhost +

