Title: pwsh Command Cache
Published: 11/19/2019
Tags: Powershell Core, pwsh, Shell
---
_This article formerly titled as 'Powershell Command Cache'_

Please be aware of notation below in command outlines. `$` represents a command and rests of the lines following that line are output. Powershell is superset of traditional command prompt. Hence, all usual binaries still run on powershell for exxample, `takeown`.

## Initialization of Shell
_This section is for my personalized shell; please feel free to skip it._

Since move to pwsh this has changed to this (sorry for replacing simple Console, but it uses an inconvenient Env:WinDir/ symbolic link),
```
pwsh -NoExit D:\Code\fftsys_ws\PowerShell\Init.ps1
```
For Machine Learning customized shell, I try adding an arg,
```
pwsh -NoExit D:\Code\fftsys_ws\PowerShell\Init.ps1 ML
```

Bluetooth cmds are replaced by,
```
Powershell.exe -NoProfile -File Bluetooth.ps1 Off / On
```

## New to pwsh?
clear-host is equivalent to cls
`Get-Location` instead of pwd`
ls, cat, popd, pushd etc still work

Write-Host is equivalent to echo. Example,
echo 'hello'
ps is equivalent to Get-Process or to List processes or doing `tasklist`.
Stop-Proccess instead of taskkill


## Application Run Examples
Here are some handy cmds,  Access sys properties,
```
Start DevEnv /Edit, Stream-Converter.ps1
Start Microsoft-Edge:https://google.com
Start Skype:

Start CVpn-Client

Start Atom
```
where CVpn-Client usually links to `C:\Program Files (x86)\Cisco\Cisco AnyConnect Secure Mobility Client\vpnui.exe`
where atom is usually linked to `C:\Users\atiq\AppData\Local\atom\atom.exe`

## Balanced Power options
Turn hibernation off (run from elevated PS),
```
powercfg.exe /h off
```

Open Power Options Window,
```
Show-ControlPanelItem -Name "Power Options"
```

```

```
List Power plans,
```
$ powercfg list
Existing Power Schemes (* Active)
-----------------------------------
Power Scheme GUID: 381b4222-f694-41f0-9685-ff5bb260df2e  (Balanced) *
Power Scheme GUID: 433299bd-efc0-474e-a61e-4a940c85e632  (Timers off (Presentation))
Power Scheme GUID: 496d75e1-8cba-4d72-b778-535d67c976ea  (Airplane)
```

How to export Balanced plan (we copy paste id from output of above command),
```
Powercfg -Export D:\Efficient_BP.pow 381b4222-f694-41f0-9685-ff5bb260df2e
```

automate importing registry (requires privilege) ref,
```
reg import 'D:\Soft\reg files soft settings\app_paths\KeePass.reg'
```

Control Panel Cmdlet
Handy cmds follow, [ref](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.management/show-controlpanelitem) to access sys properties,
```
Show-ControlPanelItem -Name System
```
Names supported by `Show-ControlPanelItem`,
- Network and Sharing Center
- Device Manager
- Programs and Features
- Default Programs

For, Sound mouse etc we do,
- Sound
- Mouse

## Other Cmdlets
Get list of running processes (unique),
```
Get-Process | Select-Object -Unique Path
```


Rename Machine,
```
Rename-Computer -NewName JohnPC -Restart
```

Event Log,
```
Show-EventLog
```

Show console host info,

    $ Get-Host
    Name             : ConsoleHost
    Version          : 6.2.3
    InstanceId       : cddce532-924c-4a66-a671-bbea53caf430
    UI               : System.Management.Automation.Internal.Host.InternalHostUserInterface
    CurrentCulture   : en-US
    CurrentUICulture : en-US
    PrivateData      : Microsoft.PowerShell.ConsoleHost+ConsoleColorProxy
    DebuggerEnabled  : True
    IsRunspacePushed : False
    Runspace         : System.Management.Automation.Runspaces.LocalRunspace

## Service Management
Managing services,
```
Stop-Service -Name VPNAgent
Start-Service -Name VPNAgent
```

## Scripting using pwsh
These basic stuffs might come handy,

### Type Conversion
command to find ascii number of a bunch of characters,

```
$ [int[]][char[]] 'abcd'
97
98
99
100
```

to find ascii number of single char,
```
$ [int][char] 'z'
122
```

Array Sort,
```
$a = [int[]] @(9,5)
[Array]::Sort($a)
```

Because these literatls i.e., 'xxx' in powershell is considered as string literal like python.
```
$ 'z'.gettype()
IsPublic IsSerial Name                                     BaseType
-------- -------- ----                                     --------
True     True     String                                   System.Object
```

#### String Helpers
nll or empty related where `$ConfigName` is an example variable,
```
[string]::IsNullOrEmpty($ConfigName)
[string]::IsEmpty($ConfigName)
[string]::Empty($ConfigName)
```

## Show OS Version
Using Net Framework Library,

    $ [Environment]::OSVersion
    Platform ServicePack Version      VersionString
    -------- ----------- -------      -------------
    Win32NT             10.0.18362.0 Microsoft Windows NT 10.0.18362.0

    $ [Environment]::OSVersion.Version
    Major  Minor  Build  Revision
    -----  -----  -----  --------
    10     0      18362  0

Additionally, we can do this inspecting `hal.dll`,

    $ [Version](Get-ItemProperty -Path "$($Env:Windir)\System32\hal.dll" -ErrorAction SilentlyContinue).VersionInfo.FileVersion.Split()[0]
    Major  Minor  Build  Revision
    -----  -----  -----  --------
    10     0      18362  356
