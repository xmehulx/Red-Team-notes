>Requires to be run from a domain-joined host or in a domain-user context.

[Snaffler](https://github.com/SnaffCon/Snaffler) is a tool that helps us acquire credentials or other sensitive data in an AD environment. It obtains a list of hosts within the domain and then enumerats them for shares and readable directories. Once that is done, it iterates through any directories readable by our user and hunts for files that could serve to better our position within the assessment.
# Example Usage
```powershell
PS > Snaffler.exe -s -d inlanefreight.local -o snaffler.log -v data
```
## Useful flags
- `-s`: Print results on console
- `-d`: Specify the domain
- `-o`: Save results to a logfile
- `-v`: Verbosity level