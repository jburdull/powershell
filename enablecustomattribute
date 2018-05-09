#accounts created within the last 30 days
$When = ((Get-Date).AddDays(-30)).Date

#active AD accounts within the last 30 days
$ADnames = Get-ADUser -Filter {enabled -eq 'true' -AND whenCreated -ge $When -AND msExchUserAccountControl -eq '2'} | Select SamAccountName

#disabled AD accounts under 10,000 to indicate shared inbox
$groupADnames = Get-ADUser -Filter {enabled -eq 'false' -AND whenCreated -ge $When -AND msExchUserAccountControl -eq '2'} -Properties employeeID | where-object {[int]$_.employeeID -lt 10000} | Select SamAccountName, employeeID
Write-Output $ADnames

#loop through active AD accounts and set attribute to 0
foreach ($ADname in $ADnames)
{
Set-ADUser $ADname -replace @{msExchUserAccountControl="0"}
}

#loop through inactive AD accounts and set attribute to 0
foreach ($groupADname in $groupADnames)
{
Set-ADUser $groupADname -replace @{msExchUserAccountControl="0"}
}
