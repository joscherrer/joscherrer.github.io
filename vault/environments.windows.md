---
id: Odo5JvNX2nDLnJ1jim3Pg
title: Windows
desc: ''
updated: 1641850983009
created: 1641739377033
---

## Keyboard rate

To set my preferred keyboard rate, either execute the following `.reg` file :

```reg
Windows Registry Editor Version 5.00

[HKEY_CURRENT_USER\Control Panel\Accessibility\Keyboard Response]
"AutoRepeatDelay"="200"
"AutoRepeatRate"="6"
"DelayBeforeAcceptance"="0"
"Flags"="27"
"BounceTime"="0"
```

Or execute these commands in a powershell :

```powershell
Set-ItemProperty -path 'HKCU:\Control Panel\Accessibility\Keyboard Response' -Name "AutoRepeatDelay" -Value "200"
Set-ItemProperty -path 'HKCU:\Control Panel\Accessibility\Keyboard Response' -Name "AutoRepeatRate" -Value "6"
Set-ItemProperty -path 'HKCU:\Control Panel\Accessibility\Keyboard Response' -Name "DelayBeforeAcceptance" -Value "0"
Set-ItemProperty -path 'HKCU:\Control Panel\Accessibility\Keyboard Response' -Name "Flags" -Value "27"
Set-ItemProperty -path 'HKCU:\Control Panel\Accessibility\Keyboard Response' -Name "BounceTime" -Value "0"
```

⚠️ Don't forget to logout for the changes to be seen ⚠️