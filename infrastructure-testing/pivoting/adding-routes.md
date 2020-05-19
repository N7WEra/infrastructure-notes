# Adding routes

## Linux 

### Display your current routing table 

Open the Terminal or login to server using ssh/console. Type the following command to display routing table: 

```text
# route 
Or 
# route -n 
Or 
# ip route show 
Or 
# ip route list 
```

### Adding routes: 

Linux add a default route using route command 

Route all traffic via 192.168.1.254 gateway connected via eth0 network interface: 

```text
# route add default gw 192.168.1.254 eth0 
OR 
# route add -host {Target host} gw {Gateway IP} 
OR Range: 
# route add -net 192.168.1.0 netmask 255.255.255.0 gw 192.168.1.254 
```

### Linux add a default gateway \(route\) using ip command 

Route all traffic via 192.168.1.254 gateway connected via eth0 network interface: 

`# ip route add 192.168.1.0/24 dev eth0` 

Add route via other route: 

```text
root@UK198899:~# ping 172.16.0.128 
^CPING 172.16.0.128 (172.16.0.128) 56(84) bytes of data. 
--- 172.16.0.128 ping statistics --- 
7 packets transmitted, 0 received, 100% packet loss, time 6154ms 
root@UK198899:~# ip route add 172.16.0.0/24 via 192.168.0.30 
root@UK198899:~# ping 172.16.0.129 
PING 172.16.0.129 (172.16.0.129) 56(84) bytes of data. 
64 bytes from 172.16.0.129: icmp_seq=1 ttl=64 time=2.07 ms 
64 bytes from 172.16.0.129: icmp_seq=2 ttl=64 time=0.445 ms 
^C 
--- 172.16.0.129 ping statistics --- 
2 packets transmitted, 2 received, 0% packet loss, time 1002ms 
rtt min/avg/max/mdev = 0.445/1.255/2.065/0.810 ms 
```

or 

`route add –host 172.2.202.20 gw 10.2.202.2` 

## Windows 

Open command prompt as administrator 

### **Print routes:** 

`route print` 

### **Add route** 

`route ADD 192.168.35.0 MASK 255.255.255.0 192.168.0.2` 

### Remove route: 

`route delete destination_network` 

