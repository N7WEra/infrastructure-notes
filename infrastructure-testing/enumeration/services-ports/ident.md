---
description: >-
  Internet protocol that helps identify the user of a particular TCP connection.
  One popular daemon program for providing the ident service is identd.
---

# 113 - ident

### ident-user-enum

\*not in Kali, download: [http://pentestmonkey.net/tools/user-enumeration/ident-user-enum](http://pentestmonkey.net/tools/user-enumeration/ident-user-enum) 

**Example**: 

```text
/opt/ident-user-enum.pl perl ident-user-enum.pl 10.10.xx.xx 22 53 111 113 512 513 514 515 
ident-user-enum v1.0 (http://pentestmonkey.net/tools/ident-user-enum) 

10.10.xx.xx:22         [U2FsdGVkX19U+FaOs8zFI+sBFw5PBF2/hxWdfeblTXM=] 
10.10.xx.xx:53         [U2FsdGVkX1+fVazmVwSBwobo05dskDNWG8mogAWzHS8=] 
10.10.xx.xx:111        [U2FsdGVkX1+GPhL0rdMggQOQmNzsxtKe+ro+YQ28nTg=] 
10.10.xx.xx:113        [U2FsdGVkX1+5f5j9c2qnHFL5XKMcLV7YjUW8LYWN1ac=] 
10.10.xx.xx:512        [U2FsdGVkX1+IWVqsWohbUhjr3PAgbkWTaImWIODMUDY=] 
10.10.xx.xx:513        [U2FsdGVkX19EEjrVAxj0lX0tTT/FoB3J9BUlfVqN3Qs=] 
10.10.xx.xx:514        [U2FsdGVkX18/o1MMaGmcU4ul7kNowuhfBgiplQZ0R5c=] 
10.10.xx.xx:515        [U2FsdGVkX1/8ef5wkL05TTMi+skSs65KRGIQB9Z8WnE=] 
```

