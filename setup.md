Run the following in powershell
```
if (!(Test-Path -Path $PROFILE.CurrentUserAllHosts)) {
   New-Item -ItemType File -Path $PROFILE.CurrentUserAllHosts -Force
}
ii $PROFILE.CurrentUserAllHosts
```
then paste in
```
[10/7 3:10 PM] Sanjay Govind
    Runthe following in powershell

if (!(Test-Path -Path $PROFILE.CurrentUserAllHosts)) {​
   New-Item -ItemType File -Path $PROFILE.CurrentUserAllHosts -Force
}​
ii $PROFILE.CurrentUserAllHosts

then paste in

Set-PSReadlineKeyHandler -Key Tab -Function MenuComplete
# Autocompletion for arrow keys
Set-PSReadlineKeyHandler -Key UpArrow -Function HistorySearchBackward
Set-PSReadlineKeyHandler -Key DownArrow -Function HistorySearchForward
$scriptblock = {​
    param($wordToComplete, $commandAst, $cursorPosition)
        write-host $wordToComplete
        $values =  get-content ~/.ssh/config | Select-String "Host (.+)" -AllMatches | % {​ $_.Matches }​ | % {​ $_.Groups[1].Value }​
        $values += Invoke-RestMethod -Uri oxidized.forge.wetaworkshop.co.nz/nodes.json | % {​ $_.name }​
        $values | Sort-Object | where-object {​$_.StartsWith($wordToComplete)}​ | ForEach-Object {​
            [System.Management.Automation.CompletionResult]::new($_, $_, 'ParameterValue', $_)
        }​
        # write-host $cursorPosition
}​
Register-ArgumentCompleter -Native -CommandName ssh -ScriptBlock $scriptblock
```
Then run 
```
ii ~\.ssh\config
```
then put in 
```
Host <name>
  HostName <name>
  ForwardAgent yes
  User <username>
```
