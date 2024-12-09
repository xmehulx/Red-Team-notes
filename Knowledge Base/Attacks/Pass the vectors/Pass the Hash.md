---
tags:
  - PtH
  - PtT
---
Hashes can be obtained in several ways, including:
- Dumping the local SAM database from a compromised host
- Extracting hashes from the NTDS database (ntds.dit) on a Domain Controller
- Pulling the hashes from memory (lsass.exe)

# Pass the Key or OverPass the Hash
[Abusing Microsoft Kerberos - Sorry you guys don't get it](https://www.slideshare.net/gentilkiwi/abusing-microsoft-kerberos-sorry-you-guys-dont-get-it/18) 
User's hash/key (rc4_hmac, aes256_cts_hmac_sha1, etc.) for a domain-joined user can be converted into a full `TGT` for [[Pass the Ticket]] attack 