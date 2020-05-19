# enum4linux

**Verbose mode, shows the underlying commands being executed by enum4linux** 

enum4linux -v target-ip 

**Do Everything, runs all options apart from dictionary based share name guessing** 

enum4linux -a target-ip 

**Lists usernames, if the server allows it - \(RestrictAnonymous = 0\)** 

enum4linux -U target-ip 

**If you've managed to obtain credentials, you can pull a full list of users regardless of the RestrictAnonymous option**

enum4linux -u administrator -p password -U target-ip 

