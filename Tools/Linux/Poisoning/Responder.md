---
tags:
  - linux
---
>Run as `sudo`

[Responder](https://github.com/lgandx/Responder) is an LLMNR, NBT-NS, and mDNS poisoner tool with different capabilities, one of them is the possibility to set up fake services, like SMB, to steal NetNTLM v1/v2 hashes. In its default configuration, it will find LLMNR and NBT-NS traffic. Then, it will respond on behalf of the servers the victim is looking for and capture their NetNTLM hashes.

```shell-session
$ responder -I <INTERFACE>
```
# Useful Flags
- `-A`: analyze traffic without poisoning.
- `-wf`: start WPAD rogue proxy server which will capture all HTTP requests by any user that launches IE with [Auto-detect settings](https://docs.microsoft.com/en-us/internet-explorer/ie11-deploy-guide/auto-detect-settings-for-ie11) enabled.

>**Note:** If we notice multiples hashes for one account this is because NTLMv2 utilizes both a client-side and server-side challenge that is randomized for each interaction. Because the resulting hashes that are sent are salted with randomized string of numbers, the hashes don't match but still represent the same password.

If the hash cannot be cracked, we can potentially relay it to another machine using [[NTLMRelayX]] (set `SMB = Off` in `/etc/responder/Responder.conf` before) or Reponder's [MultiRelay.py](https://github.com/lgandx/Responder/blob/master/tools/MultiRelay.py)

# MultiRelay