---
description: >-
  Java Remote Method Invocation (Java RMI) is a Java API that performs remote
  method invocation, the object-oriented equivalent of remote procedure calls
  (RPC)
---

# 1099 - Java RMI

Use BaRMIe to enumerate functions

{% embed url="https://noraj.gitlab.io/the-hacking-trove/Tools/barmie/" %}

Download standalone: 

{% embed url="https://github.com/NickstaDB/BaRMIe/releases/" %}

Enumeration:

```text

root@kali:$ java -jar BaRMIe_v1.01.jar -enum 172.16.11.18 1100 
Picked up _JAVA_OPTIONS: -Dawt.useSystemAAFontSettings=on -Dswing.aatext=true 

  

  ▄▄▄▄    ▄▄▄       ██▀███   ███▄ ▄███▓ ██▓▓█████  

▓█████▄ ▒████▄    ▓██ ▒ ██▒▓██▒▀█▀ ██▒▓██▒▓█   ▀  

▒██▒ ▄██▒██  ▀█▄  ▓██ ░▄█ ▒▓██    ▓██░▒██▒▒███    

▒██░█▀  ░██▄▄▄▄██ ▒██▀▀█▄  ▒██    ▒██ ░██░▒▓█  ▄  

░▓█  ▀█▓ ▓█   ▓██▒░██▓ ▒██▒▒██▒   ░██▒░██░░▒████▒ 

░▒▓███▀▒ ▒▒   ▓▒█░░ ▒▓ ░▒▓░░ ▒░   ░  ░░▓  ░░ ▒░ ░ 

▒░▒   ░   ▒   ▒▒ ░  ░▒ ░ ▒░░  ░      ░ ▒ ░ ░ ░  ░ 

  ░    ░   ░   ▒     ░░   ░ ░      ░    ▒ ░   ░    

  ░            ░  ░   ░            ░    ░     ░  ░ 

       ░                                     v1.0 

             Java RMI enumeration tool. 

               Written by Nicky Bloor (@NickstaDB) 

  

Warning: BaRMIe was written to aid security professionals in identifying the 

         insecure use of RMI services on systems which the user has prior 

         permission to attack. BaRMIe must be used in accordance with all 

         relevant laws. Failure to do so could lead to your prosecution. 

         The developers assume no liability and are not responsible for any 

         misuse or damage caused by this program. 

  

Scanning 1 target(s) for objects exposed via an RMI registry... 

  

[-] An exception occurred during the PassThroughProxyThread main loop. 

    java.net.SocketException: Socket closed 

[-] An exception occurred during the ReplyDataCapturingProxyThread main loop. 

    java.net.SocketException: Socket closed 

RMI Registry at 172.16.11.18:1100 

Objects exposed: 1 

Object 1 

  Name: creamtec/ajaxswing/JVMFactory 

  Endpoint: 172.16.11.18:49671 

  Classes: 3 

    Class 1 

      Classname: java.rmi.server.RemoteStub 

    Class 2 

      Classname: java.rmi.server.RemoteObject 

    Class 3 

      Classname: com.creamtec.ajaxswing.core.JVMFactory_Stub 

  

1 potential attacks identified (+++ = more reliable) 

[---] Java RMI registry illegal bind deserialization 

  

0 deserialization gadgets found on leaked CLASSPATH 

[~] Gadgets may still be present despite CLASSPATH not being leaked 

  

Successfully scanned 1 target(s) for objects exposed via RMI. 
```

Exploitation use 

```text
BaRMIe -attack
```

