# Intune-Backup-Restore

The following is some information I found from: <br/>
I do not take credit for this, I only run this for testings purposes.<br/>

```ruby 
#Installing reqs Only to be done once the its whole life, then you can Remark it with a "#"
Install-Module -Name AzureAD ,  MSGraphFunctions, IntuneBackupAndRestore -Force

Import-Module  -Name MSGraphFunctions, IntuneBackupAndRestore
#Authentication
Connect-Graph -Username "UPN/EMAIL-Goeshere"

<#
Things to Understand:
    1 - The reports for Policies will reset back to 0
    2 - Depending on the Restore you will be doing, you will find doubles settings if one still exist.
    3 - This will not Restore the Groups that you have assigned these policies to, -or At least I haven't found a way to do so, but you will find the group object ID.
    4 - If you are trying to do Recover some settings, then go Inside of the backed up folder and remove the JSON file, you think you already have.
#>

#Most common Settings: Please feel free to explore other cmdlets

#Backup your Intune
if (!(Test-Path "C:\temp")){New-Item -Path "c:\temp" -Name "logfiles" -ItemType "directory"}
Start-IntuneBackup -Path "C:\temp\IntuneBackup-RevA"

#This will restore everything back but it will not check if settings exist or not, Doubles might come up.
Start-IntuneRestoreConfig -Path "C:\temp\IntuneBackup-RevA"

#This method would just do the Entire Config Section for Compliance
Invoke-IntuneRestoreDeviceCompliancePolicy -Path "C:\temp\IntuneBackup-RevA"

#Comparing revisions for backups
Compare-IntuneBackupDirectories -Verbose -ReferenceDirectory "C:\temp\IntuneBackup-RevA" -DifferenceDirectory "C:\temp\IntuneBackup-RevB"
```

