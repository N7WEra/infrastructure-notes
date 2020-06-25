# VLAN hopping

Yersinia is a framework for performing layer 2 attacks. It is designed to take advantage of some weaknesses in different network protocols. It pretends to be a solid framework for analyzing and testing the deployed networks and systems. Attacks for the following network protocols are implemented in this particular release:

* Spanning Tree Protocol \(STP\)
* Cisco Discovery Protocol \(CDP\)
* Dynamic Trunking Protocol \(DTP\)
* Dynamic Host Configuration Protocol \(DHCP\)
* Hot Standby Router Protocol \(HSRP\)
* 802.1q
* 802.1x
* Inter-Switch Link Protocol \(ISL\)
* VLAN Trunking Protocol \(VTP\)

Come build into Kali.

### Help menu

```text
root@kali:~# yersinia -h
    ۲�۲��
   �������۲�
 ۲��������۲�
�����۱����������
����۱������������
����۱�������������               Yersinia...
������������������۲��
۲���۱��������������۲��         The Black Death for nowadays networks
 ������۱�����������������
 �������۱���������������۲�             by Slay & tomac
  ۲�����۱�������������������
     �����۱�������������������        http://www.yersinia.net
      ۲����۱���������������۲            yersinia@yersinia.net
       ۲�����۱���������������
         �������۱����������۲�
         �۲���������۱�������     Prune your MSTP, RSTP, STP trees!!!!
             �������������۲�


Usage: yersinia [-hVGIDd] [-l logfile] [-c conffile] protocol [protocol_options]
       -V   Program version.
       -h   This help screen.
       -G   Graphical mode (GTK).
       -I   Interactive mode (ncurses).
       -D   Daemon mode.
       -d   Debug.
       -l logfile   Select logfile.
       -c conffile  Select config file.
  protocol   One of the following: cdp, dhcp, dot1q, dot1x, dtp, hsrp, isl, mpls, stp, vtp.

Try 'yersinia protocol -h' to see protocol_options help

Please, see the man page for a full list of options and many examples.
Send your bugs & suggestions to the Yersinia developers <yersinia@yersinia.net>



MOTD: The Hakin9 magazine owe money to us... 500 Euros 
```

### Graphical interface:

yersinia -G

### To Abuse DTP:

```text
click "Launch attack" 
click the tab "DTP" 
click "enable trunking" 
click "ok" 
```

And now add the new route you want to attack:

```text
modprobe 8021q
vconfig add eth {VLAN Number}
ifconfig eth0.200 up
Ifconfig eth.200 {ip range} up
```



