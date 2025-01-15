# Constrained Language Mode
PowerShell [Constrained Language Mode](https://devblogs.microsoft.com/powershell/powershell-constrained-language-mode/) locks down many of the features needed to use PowerShell effectively, such as blocking COM objects, only allowing approved .NET types, XAML-based workflows, PowerShell classes, and more. We can find whether we are in Full Language Mode or Constrained Language Mode with below command.
```powershell
PS > $ExecutionContext.SessionState.LanguageMode
ConstrainedLanguage
```