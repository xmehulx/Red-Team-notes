![[metasploit-framework.png]]
# #encoding

# Database

# Plugins
# Payloads
## Singles
Inline `single` payloads are by design more stable, but some exploits can't handle the size.
## Stagers
`Stager` payloads work with Stage payloads to perform a specific task, where the stager waits on attacking system for the stage to establish a connection to the victim host.  Typically used to set up small and reliable network connection.
## Stages
## Staged
## Meterpreter
- Uses DLL injection ()
- Resides in memory, with no trace on disk
- Could be used for #av-evasion 