Title: Generic Windows Command Cache
Published: 11/19/2019
Tags:
  - windows command prompt
  - system administration
---
Please be aware of notation below in command outlines. `$` represents a command and rests of the lines following that line are output. Powershell is superset of traditional command prompt. Hence, all usual binaries still run on powershell for exxample, `takeown`.

## Misc

Customized ping command, waits 20 seconds for reply and keep pinging non-stop [ping ref](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/ping),

    ping -w 20000 -t myserver.company.com

Enable or disable Linux Subsystem feture,

    Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Windows-Subsystem-Linux
    Disable-WindowsOptionalFeature -Online -FeatureName Microsoft-Windows-Subsystem-Linux

Get host name,

    HOSTNAME

associating file type for extension `.fob`,

    cmd /c assoc .fob=foobarfile
    cmd /c ftype foobarfile=powershell.exe -File `"C:\path\to\your.ps1`" `"%1`"

Find files with hidden attributes,

    dir /A:H general-solving\lintcode

Modify files to remove hidden attributes using attrib

    attrib -h my-hidden-file.txt

## References
- [MS Docs - Windows Commands](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/commands-by-server-role)
