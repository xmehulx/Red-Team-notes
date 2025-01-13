Port:
	UDP 137

[NetBIOS Name Service](https://docs.microsoft.com/en-us/previous-versions/windows/it-pro/windows-2000-server/cc940063(v=technet.10)?redirectedfrom=MSDN) is Microsoft Windows components that serve as alternate methods of host identification when [[LLMNR]] fails. It also allows **any hosts** on the same local link to **perform name resolution** for other hosts like LLMNR!
# Tools
- [[Responder]]
- [[InveighZero]]
- [[MSFConsole]]
# Resolution
>NBT-NS cannot be disabled via Group Policy but must be disabled locally on each host.
## 1. Disable NBT-NS resolution
- Manual
 `Control Panel` -> `Network and Sharing Center` -> `Change adapter settings`, right-click on the adapter -> `properties` -> `Internet Protocol Version 4 (TCP/IPv4)` -> `Properties` button -> `Advanced` -> `WINS` tab -> `Disable NetBIOS over TCP/IP`.
 
 - Autorun script
 Create a PowerShell script under Computer Configuration --> Windows Settings --> Script (Startup/Shutdown) --> Startup
 ```powershell
$regkey = "HKLM:SYSTEM\CurrentControlSet\services\NetBT\Parameters\Interfaces"
Get-ChildItem $regkey |foreach { Set-ItemProperty -Path "$regkey\$($_.pschildname)" -Name NetbiosOptions -Value 2 -Verbose}
```
In the Local Group Policy Editor, double click on `Startup` -> `PowerShell Scripts` tab -> select "For this GPO, run scripts in the following order" to `Run Windows PowerShell scripts first` -> click `Add` and choose the script.

To push this out to all hosts in a domain, create a GPO using `Group Policy Management` on the Domain Controller and host the script on the SYSVOL share in the scripts folder and then call it via its UNC path such as:
`\\inlanefreight.local\SYSVOL\INLANEFREIGHT.LOCAL\scripts`

Once the GPO is applied to specific OUs and those hosts are restarted, the script will run at the next reboot and disable NBT-NS, provided that the script still exists on the SYSVOL share and is accessible by the host over the network.
## 2. Enable SMB Signing
This prevents NTLM relay attacks.
## 3. Network Segmentation
To isolate hosts that require NetBIOS enabled to operate correctly.