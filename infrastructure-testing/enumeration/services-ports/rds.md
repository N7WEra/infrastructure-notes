---
description: >-
  RDS can be utilized to provide users with remote access to an entire desktop
  or just specific applications and programs required for their day-to-day work.
  RDS is server-based and allows for multiple
---

# RDS

**Password Spraying RDS** 

In order to perform a password spraying attack we first need the internal domain name of the target. This can be found quickly in the RDS logon page source as the WorkSpaceID. 

`curl -s -insecure https://192.168.88.150/RDWeb/Pages/en-US/Login.aspx?returnUrl=/RDWeb/Pages/en-US/Default.aspx`

grep for 'WorkSpaceID'



**Source**:

[https://medium.com/vartai-security/attack-chain-series-remote-access-service-compromise-part-1-rds-e984101c78b7](https://medium.com/vartai-security/attack-chain-series-remote-access-service-compromise-part-1-rds-e984101c78b7)

