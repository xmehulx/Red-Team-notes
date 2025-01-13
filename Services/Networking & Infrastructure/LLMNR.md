Port:
	UDP 5355

[Link-Local Multicast Name Resolution](https://datatracker.ietf.org/doc/html/rfc4795) (LLMNR) is Microsoft Windows components that could serve as alternate methods of host identification when [[Services/Networking & Infrastructure/DNS|DNS]] fails. LLMNR is based on DNS format and allows **any hosts** on the same local link to **perform name resolution** for other hosts!
# Tools
- [[Responder]]
- [[InveighZero]]
- [[MSFConsole]]
# Resolution
## 1. Disable LLMNR resolution
LLMNR can be disabled in Group Policy; Computer Configuration --> Administrative Templates --> Network --> DNS Client and enabling "Turn OFF Multicast Name Resolution."
## 2. Enable SMB Signing
This prevents NTLM relay attacks.
## 3. Network Segmentation
To isolate hosts that require LLMNR enabled to operate correctly.