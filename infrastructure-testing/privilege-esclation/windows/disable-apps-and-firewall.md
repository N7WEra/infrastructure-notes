# Disable Apps and Firewall

## Disable Defender 

`powershell Set-MpPreference -DisableRealtimeMonitoring $true` 

OR 

`Set-ItemProperty -Path "HKLM:SOFTWARE\Policies\Microsoft\Windows Defender" -Name DisableAntiSpyware -Value 0x00000001 -Force` 

## Disable Firewall 

**Show status:** 

`netsh firewall show state` 

**Disable:** 

`netsh advfirewall set  currentprofile state off` 

Or 

`netsh firewall set opmode disable` 

**To check the status of Windows Firewall:** 

`Netsh Advfirewall show allprofiles` 

