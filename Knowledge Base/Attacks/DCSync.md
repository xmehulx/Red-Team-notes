---
tags:
  - windows
  - MicrosoftAD
---
# #dcsync
DCSync is a technique for stealing the AD password database by using the built-in `Directory Replication Service Remote Protocol`, used by Domain Controllers to replicate domain data. This allows mimicking a Domain Controller to retrieve user NTLM password hashes.
# Requirements
Domain user having `DS-Replication-Get-Changes-All` extended right.
![[adnunn_right_dcsync.webp]]
# The Attack
