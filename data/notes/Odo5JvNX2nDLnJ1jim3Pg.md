
## Prerequisites

Install [scoop](https://scoop.sh)
```powershell
iwr -useb get.scoop.sh | iex
```

Install gsudo
```powershell
scoop install gsudo
```

Update Powershell Help
```powershell
Update-Help
```

## Openssh server

First install the OpenSSH server 
```powershell
gsudo "Get-WindowsCapability -Online | Where-Object Name -like 'OpenSSH.Server'"
gsudo Add-WindowsCapability -Online -Name OpenSSH.Server~~~~0.0.1.0
gsudo Start-Service sshd
gsudo Set-Service -Name sshd -StartupType 'Automatic'

if (!(Get-NetFirewallRule -Name "OpenSSH-Server-In-TCP" -ErrorAction SilentlyContinue | Select-Object Name, Enabled)) {
    Write-Output "Firewall Rule 'OpenSSH-Server-In-TCP' does not exist, creating it..."
    New-NetFirewallRule -Name 'OpenSSH-Server-In-TCP' -DisplayName 'OpenSSH Server (sshd)' -Enabled True -Direction Inbound -Protocol TCP -Action Allow -LocalPort 22
} else {
    Write-Output "Firewall rule 'OpenSSH-Server-In-TCP' has been created and exists."
}
```

Then configure keys
```powershell
Get-Content .\.ssh\id_rsa.pub | Out-File -Append .\.ssh\authorized_keys
New-Item C:\ProgramData\ssh\ -ItemType Directory
gsudo "Get-Content .\.ssh\id_rsa.pub | Out-File -Append C:\ProgramData\ssh\administrators_authorized_keys"
gsudo "icacls.exe 'C:\ProgramData\ssh\administrators_authorized_keys' /inheritance:r /grant 'Administrators:F' /grant 'SYSTEM:F'"
```

Change the default shell
```powershell
gsudo New-ItemProperty -Path "HKLM:\SOFTWARE\OpenSSH" -Name DefaultShell -Value "C:\Program Files\powershell\7\pwsh.exe" -PropertyType String -Force
```

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

## Taskbar Hover time

```powershell
Set-ItemProperty -path 'HKCU:\Control Panel\Mouse' -Name "MouseHoverTime" -Value 200
```

## WSL

Install WSL
```powershell
wsl --install -d "Ubuntu-20.04"
```

## Fonts

Install NERD fonts
```powershell
git clone --filter=blob:none --sparse git@github.com:ryanoasis/nerd-fonts
cd nerd-fonts
git sparse-checkout add patched-fonts/CascadiaCode
.\install.ps1 CascadiaCode
```

## Clipboard

Install [win32yank](https://github.com/equalsraf/win32yank/latest/releases/download/win32yank-x64.zip)

In WSL 
```bash
curl -OL https://github.com/equalsraf/win32yank/releases/latest/download/win32yank-x64.zip
unzip win32yank-x64.zip win32yank.exe -d ~/.local/bin/
chmod +x ~/.local/bin/win32yank.exe
```
