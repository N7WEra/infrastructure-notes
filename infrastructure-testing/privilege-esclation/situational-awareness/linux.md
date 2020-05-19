# Linux

The first step when landing on host should be understanding who your against to - what OS, what process are running,  what users exists and more, this can be done by looking at the following files \(remember - in Linux everything is a file\):

### **Distribution type:** 

`cat /etc/*-release` 

### Kernel version: 

`cat /proc/version`   
`uname -a` 

### view if you can run anything as sudo: \(check for GTFObins\) 

`Sudo -l` 

### Check common files: 

```text
cat /etc/profile 
cat /etc/bashrc 
cat ~/.bash_history 
cat ~/.bashrc 
cat ~/.bash_logout
```

### What services running \(filter by root\): 

`ps aux` 

`ps -ef` 

`top` 

### Check configuration files: 

```text
cat /etc/syslog.conf 
cat /etc/chttp.conf 
cat /etc/lighttpd.conf 
cat /etc/cups/cupsd.conf 
cat /etc/inetd.conf 
cat /etc/apache2/apache2.conf 
cat /etc/my.conf 
cat /etc/httpd/conf/httpd.conf 
cat /opt/lampp/etc/httpd.conf
```

###  Check local ports and what listens: 

`netstat -antup` 

### View list of users: 

`cat /etc/passwd | cut -d: -f1`  

### Search for ssh keys: 

```text
cat ~/.ssh/authorized_keys 
cat ~/.ssh/identity.pub 
cat ~/.ssh/identity 
cat ~/.ssh/id_rsa.pub 
cat ~/.ssh/id_rsa 
cat ~/.ssh/id_dsa.pub 
cat ~/.ssh/id_dsa 
cat /etc/ssh/ssh_config 
cat /etc/ssh/sshd_config 
cat /etc/ssh/ssh_host_dsa_key.pub 
cat /etc/ssh/ssh_host_dsa_key 
cat /etc/ssh/ssh_host_rsa_key.pub 
cat /etc/ssh/ssh_host_rsa_key 
cat /etc/ssh/ssh_host_key.pub 
cat /etc/ssh/ssh_host_key 
```

### View crontabs

```text
crontab -e root 
crontab -l 
ls -alh /var/spool/cron 
ls -al /etc/ | grep cron 
ls -al /etc/cron* 
cat /etc/cron* 
cat /etc/at.allow 
cat /etc/at.deny 
cat /etc/cron.allow 
cat /etc/cron.deny 
cat /etc/crontab 
cat /etc/anacrontab 
cat /var/spool/cron/crontabs/root 
```

### Web servers files

```text
ls -alhR /var/www/ 
ls -alhR /srv/www/htdocs/ 
ls -alhR /usr/local/www/apache22/data/ 
ls -alhR /opt/lampp/htdocs/ 
ls -alhR /var/www/html/ 
```

