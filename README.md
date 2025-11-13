# lockoutstatus-script
Скрипт находит устройства, которые привели к блокировке учётной записи в AD
```
$Username = 'agoncharov'
Get-ADDomainController -fi * | select -exp hostname | % {
$GweParams = @{
‘Computername’ = $_
‘LogName’ = ‘Security’
‘FilterXPath’ = "*[System[EventID=4740] and EventData[Data[@Name='TargetUserName']='$Username']]"
}
$Events = Get-WinEvent @GweParams
$Events | foreach {$_.Computer + " " +$_.Properties[1].value + ' ' + $_.TimeCreated}
}
```
