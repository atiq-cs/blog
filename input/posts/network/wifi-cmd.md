Title: WiFi Networking with Powershell and netsh
Published: 11/22/2019
Tags: Powershell Core; pwsh; netsh; Network; System Administration
---
Please be aware of notation below in command outlines. `$` represents a command and rests of the lines following that line are output. Powershell is superset of traditional command prompt. Hence, all usual binaries still run on powershell for exxample, `takeown`.

# Introduction
*To connect to a specific network or SSID*

    netsh wlan connect name='Google Starbucks'

*To disconnect from WiFi*

    $ netsh wlan disconnect interface=Wi-Fi
    Disconnection request was completed successfully for interface "Wi-Fi".

*To disable Net WiFi Adapter (requires admin privilege),*

    Disable-NetAdapter -Name Wi-Fi -Confirm:$false

*To disable Net WiFi Adapter (requires admin privilege),*

    Enable-NetAdapter -Name Wi-Fi


To provide more context, new cmdlets and netsh features enable us to control network interfaces of the PC. Here's a netsh example to list WiFi SSIDs,

    $ netsh wlan show profiles

    Profiles on interface Wi-Fi:

    Group policy profiles (read only)
    ---------------------------------

    User profiles
    -------------
        All User Profile     : CSC-Public
        All User Profile     : Google Starbucks
        All User Profile     : VTA Free WiFi
        All User Profile     : Philz Tesora
        All User Profile     : PEETS
    .......

This command does not show currently available WiFi Networks. It only shows the networks that are saved in the System because it has connected to those in the past.

Following shows pretty much the same list probably because most WiFi have clear type Key,

    $ netsh wlan show profiles key=clear

*To view information on the currently connect WiFi network*

    $ Get-NetConnectionProfile
    Name             : Google Starbucks  12
    InterfaceAlias   : Wi-Fi
    InterfaceIndex   : 16
    NetworkCategory  : Public
    IPv4Connectivity : Internet
    IPv6Connectivity : NoTraffic

Applying above command on a specific SSID provides us more info,

    $ netsh wlan show profiles name='Google Starbucks'

    Profile Google Starbucks on interface Wi-Fi:
    =======================================================================

    Applied: All User Profile

    Profile information
    -------------------
        Version                : 1
        Type                   : Wireless LAN
        Name                   : Google Starbucks
        Control options        :
            Connection mode    : Connect automatically
            Network broadcast  : Connect only if this network is broadcasting
            AutoSwitch         : Do not switch to other networks
            MAC Randomization  : Disabled

    Connectivity settings
    ---------------------
        Number of SSIDs        : 1
        SSID name              : "Google Starbucks"
        Network type           : Infrastructure
        Radio type             : [ Any Radio Type ]
        Vendor extension          : Not present

    Security settings
    -----------------
        Authentication         : Open
        Cipher                 : None
        Security key           : Absent
        Key Index              : 1

    Cost settings
    -------------
        Cost                   : Unrestricted
        Congested              : No
        Approaching Data Limit : No
        Over Data Limit        : No
        Roaming                : No
        Cost Source            : Default

Following is equivalent,

    $ netsh wlan show profiles name='Google Starbucks' key=clear

### Other Commands
To view currently connected SSID etc,

    $ netsh wlan show interfaces

    There is 1 interface on the system:

        Name                   : Wi-Fi
        Description            : Dell Wireless 1830 802.11ac
        GUID                   : ebf95ed0-e6b2-455b-b863-291169f3ffb3
        Physical address       : 6a:ca:81:ee:ce:d6
        State                  : connected
        SSID                   : PEETS
        BSSID                  : 8a:15:14:b2:c4:50
        Network type           : Infrastructure
        Radio type             : 802.11ac
        Authentication         : Open
        Cipher                 : None
        Connection mode        : Profile
        Channel                : 157
        Receive rate (Mbps)    : 216.5
        Transmit rate (Mbps)   : 866.5
        Signal                 : 87%
        Profile                : PEETS

        Hosted network status  : Not available

#### Show Interfaces Info
Using powershell cmdlet,

View information on all network interfaces in the system,

    $ Get-NetAdapter

    Name                      InterfaceDescription                    ifIndex Status       MacAddress             LinkSpeed
    ----                      --------------------                    ------- ------       ----------             ---------
    Wi-Fi                     Intel(R) Dual Band Wireless-AC 8265           5 Disconnected 1C-4D-70-1F-A5-C2         6 Mbps
    Ethernet                  Intel(R) Ethernet Connection (5) I21...      15 Disconnected A4-4C-C8-20-10-DE          0 bps

to view information of the WiFi network interface,

    $ Get-NetAdapter -Name Wi-Fi

    Name                      InterfaceDescription                    ifIndex Status       MacAddress             LinkSpeed
    ----                      --------------------                    ------- ------       ----------             ---------
    Wi-Fi                     Intel(R) Dual Band Wireless-AC 8265           5 Up           1C-4D-70-1F-A5-C2       130 Mbps


Show cmdlets related to net adapter,

    $ gcm -Noun netadapter | select name, modulename

    Name               ModuleName
    ----               ----------
    Disable-NetAdapter NetAdapter
    Enable-NetAdapter  NetAdapter
    Get-NetAdapter     NetAdapter
    Rename-NetAdapter  NetAdapter
    Restart-NetAdapter NetAdapter
    Set-NetAdapter     NetAdapter

Additionally, now, we have cmdlet to show IP Address info without sing `netsh`,

    $ Get-NetIPAddress

    IPAddress         : fe80::1d40:6866:1418:1efe%22
    InterfaceIndex    : 22
    InterfaceAlias    : vEthernet (Default Switch)
    AddressFamily     : IPv6
    Type              : Unicast
    PrefixLength      : 64
    PrefixOrigin      : WellKnown
    SuffixOrigin      : Link
    AddressState      : Preferred
    ValidLifetime     : Infinite ([TimeSpan]::MaxValue)
    PreferredLifetime : Infinite ([TimeSpan]::MaxValue)
    SkipAsSource      : False
    PolicyStore       : ActiveStore

    IPAddress         : fe80::61e3:ae79:980e:1c5d%8
    InterfaceIndex    : 8
    InterfaceAlias    : Wi-Fi
    AddressFamily     : IPv6
    Type              : Unicast
    PrefixLength      : 64
    ... ...

    ... ...
    IPAddress         : 172.31.98.36
    InterfaceIndex    : 8
    InterfaceAlias    : Wi-Fi
    AddressFamily     : IPv4
    Type              : Unicast
    PrefixLength      : 23
    PrefixOrigin      : Dhcp
    SuffixOrigin      : Dhcp
    AddressState      : Preferred
    ValidLifetime     : 00:38:38
    PreferredLifetime : 00:38:38
    SkipAsSource      : False
    PolicyStore       : ActiveStore

    IPAddress         : 127.0.0.1
    InterfaceIndex    : 1
    InterfaceAlias    : Loopback Pseudo-Interface 1
    AddressFamily     : IPv4
    ... ...


Using `netsh`,

    $ netsh wlan show networks mode=bssid

    Interface name : Wi-Fi
    There are 2 networks currently visible.

    SSID 1 : S H Hall 3
        Network type            : Infrastructure
        Authentication          : Open
        Encryption              : None
        BSSID 1                 : e0:91:f5:f5:16:28
             Signal             : 60%
             Radio type         : 802.11g
             Channel            : 6
             Basic rates (Mbps) : 1 2
             Other rates (Mbps) : 5.5 6 9 11 12 18 24 36 48 54

    SSID 2 : S H Hall 1
        Network type            : Infrastructure
        Authentication          : Open
        Encryption              : None
        BSSID 1                 : a0:21:b7:7b:2b:be
             Signal             : 18%
             Radio type         : 802.11g
             Channel            : 6
             Basic rates (Mbps) : 1 2
             Other rates (Mbps) : 5.5 6 9 11 12 18 24 36 48 54

## References
 1. [ms technet - Get Wireless Network SSID and Password with PowerShell](https://blogs.technet.microsoft.com/heyscriptingguy/2015/11/23/get-wireless-network-ssid-and-password-with-powershell/)
 2. [netsh wlan commands](https://marckean.com/2017/03/16/auto-connect-to-wifi-access-points/)
 3. [changing autoconnect properties for some networks](https://blogs.technet.microsoft.com/heyscriptingguy/2013/06/15/weekend-scripter-use-powershell-to-find-auto-connect-wireless-networks/)
 4. [disabling unnecessary network adapters](https://blogs.technet.microsoft.com/heyscriptingguy/2014/01/13/enabling-and-disabling-network-adapters-with-powershell/)
 5. [Some possible default passwords for networks / routers / gateways](https://www.xfinity.com/support/internet/comcast-supported-routers-gateways-adapters/)
