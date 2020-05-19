---
description: >-
  SQLite is a relational database management system contained in a C library. In
  contrast to many other database management systems
---

# SQLite

SQLite is not a client–server database engine. Rather, it is embedded into the end program.

open sqlite database using sqlite3: 

`student@attackdefense:~$ sqlite3 data.db` 

**display tables:** 

```text
sqlite> .tables 
FLAG  app
```

 **Select from table:** 

```text
sqlite> SELECT * FROM FLAG; 
44dd29a07a0086948ad19ad6376db7d6 
```

**display table columns names:** 

```text
sqlite> PRAGMA table_info(FLAG); 
0|ID|INT|1||1 
1|NAME|TEXT|1||0 
2|AGE|INT|1||0 
3|ADDRESS|CHAR(50)|0||0 
```

**exit sqlite3:** 

```text
sqlite> .quit 
```

**Count columns:** 

```text
sqlite> SELECT count(*) FROM employees; 
8 
```

**Filter columns:** 

```text
sqlite> SELECT * FROM artists WHERE Name LIKE 'The%'; 
137|The Black Crowes 
138|The Clash 
139|The Cult 
140|The Doors 
141|The Police 
142|The Rolling Stones 
143|The Tea Party 
144|The Who 
156|The Office 
174|The Postal Service 
176|The Flaming Lips 
200|The Posies 
247|The King's Singers 
259|The 12 Cellists of The Berlin Philharmonic 
```

## **Resource**: 

Can practice in AttackDencesLabs 

