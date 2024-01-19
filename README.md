DaysSinceLastUpdate.ps1

Returns an integer showing how many days since Windows Update was last run.

#Count Days Since last Windows Update Was Run
#Author: Sean Bradley
#License: BSD-3-Clause-Attribution
#Attribution: https://sbcode.net/zabbix/powershell-windows-updates/
$date = Get-Date
$diff = (Get-HotFix | Sort-Object -Property InstalledOn)[-1] | Select-Object InstalledOn
$diff3 = New-TimeSpan -Start $diff.InstalledOn -end $date
write-host $diff3.days

CountUninstalledUpdates.ps1

Counts how many updates are assigned to the computer and not yet installed.

#Count Uninstalled Updates
#Author: Sean Bradley
#License: BSD-3-Clause-Attribution
#Attribution: https://sbcode.net/zabbix/powershell-windows-updates/
[Int]$Count = 0
$Searcher = new-object -com "Microsoft.Update.Searcher"
$Searcher.Search("IsAssigned=1 and IsHidden=0 and IsInstalled=0").Updates | ForEach-Object { $Count++ }
Write-Host $Count

ListUninstalledUpdates.ps1

Returns text layed out as a table of updates whether installed or not and severity

#Lists Windows Updates whether Installed or Not and Severity
#Author: Sean Bradley
#License: BSD-3-Clause-Attribution
#Attribution: https://sbcode.net/zabbix/powershell-windows-updates/
$Searcher = new-object -com "Microsoft.Update.Searcher"
$Searcher.Search("IsAssigned=1 and IsHidden=0").Updates | Format-Table title, MsrcSeverity, IsInstalled | Out-String -Width 256

