# Constrained Language Mode
PowerShell [Constrained Language Mode](https://devblogs.microsoft.com/powershell/powershell-constrained-language-mode/) locks down many of the features needed to use PowerShell effectively, such as blocking COM objects, only allowing approved .NET types, XAML-based workflows, PowerShell classes, and more. We can find whether we are in Full Language Mode or Constrained Language Mode with below command.
```powershell
PS > $ExecutionContext.SessionState.LanguageMode
ConstrainedLanguage
```
# [Credential Support](https://learn.microsoft.com/en-us/powershell/scripting/learn/deep-dives/add-credentials-to-powershell-functions?view=powershell-7.4)
## Interactive Credential
```powershell
$Cred = Get-Credential 
$Cred = Get-Credential -Credential <DOMAIN\USER> 
$Cred = Get-Credential -UserName <DOMAIN\USER> -Message 'Enter Password'
```
## Static Credential
```powershell
$password = ConvertTo-SecureString "<PASS>" -AsPlainText -Force
$Cred = New-Object System.Management.Automation.PSCredential ("<USER>", $password)
```
# Logs
With [Script Block Logging](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_logging_windows?view=powershell-7.2) enabled, whatever we type into the terminal gets sent to this log. The logs can be seen in the [[Event Viewer]],
1. All executed commands are logged in `Applications and Services Logs` > `Microsoft` > `Windows` > `PowerShell` > `Operational`. 
2. The `Windows PowerShell` log located at `Applications and Services Logs` > `Windows PowerShell` is also a good place to check. An entry will be made here when we start an instance of PowerShell