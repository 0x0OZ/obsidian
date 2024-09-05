# Windows Active Directory Enumeration.

#### check-list
- [PayloadAllTheThings](https://gitlab.com/pentest-tools/PayloadsAllTheThings/-/blob/6bcd2e8a6a39d26a547a70d83dfebef4c2c6f801/Methodology%20and%20Resources/Active%20Directory%20Attack.md)
- [S1ckB0y1337](https://github.com/S1ckB0y1337/Active-Directory-Exploitation-Cheat-Sheet)
- [PowerView](https://powersploit.readthedocs.io/en/latest/Recon/)
- check ldap for system/users informations
- check smb for shares
- check rpcbind for system/users informations
- check impacket examples scripts
- generate usernames from `fname lname` wordlist [namemash.py](https://gist.githubusercontent.com/superkojiman/11076951/raw/74f3de7740acb197ecfa8340d07d3926a95e5d46/namemash.py)
```powershell
# enuemrate valid users through kerberos
./kerbrute
# enumerate users do not require Kerberos preauthentication
GetNPUsers.py EGOTISTICAL-BANK.local/ -no-pass -usersfile usernames
# request other users krbt hash if you have a user cerdentials
GetUserSPNs.py
# runas user in a domain using credentials, or use bloodhound-python
runas /netonly /user:support cmd.exe
##
Get-NetUser -SPN #Kerberoastable users
# Show computerss 
Get-NetComputer | select operatingsystem,dnshostname
# Check if current user is logged in to a specific computer
Get-NetSession -ComputerName web04
# show AD computers
Get-NetComputer | select dnshostname,operatingsystem,operatingsystemversion
```
#### notes 
- use `runas` to connect to domain using user credentials and run bloodhound or use the credentials in bloodhoud from python
- lsass stores credentials that can be dumped using pypykatz

```powershell
# enumerate domain users and groups
net user /domain
net group /domain
# find PDC
[System.DirectoryServices.ActiveDirectory.Domain]::GetCurrentDomain()
([adsi]'').distinguishedName
```

```powershell
$PDC = [System.DirectoryServices.ActiveDirectory.Domain]::GetCurrentDomain().PdcRoleOwner.Name
$DN = ([adsi]'').distinguishedName 
$LDAP = "LDAP://$PDC/$DN"

$direntry = New-Object System.DirectoryServices.DirectoryEntry($LDAP)

$dirsearcher = New-Object System.DirectoryServices.DirectorySearcher($direntry)
$dirsearcher.FindAll()


```

- LDAP search
```powershell
function LDAPSearch {
    param (
        [string]$LDAPQuery
    )

    $PDC = [System.DirectoryServices.ActiveDirectory.Domain]::GetCurrentDomain().PdcRoleOwner.Name
    $DistinguishedName = ([adsi]'').distinguishedName

    $DirectoryEntry = New-Object System.DirectoryServices.DirectoryEntry("LDAP://$PDC/$DistinguishedName")

    $DirectorySearcher = New-Object System.DirectoryServices.DirectorySearcher($DirectoryEntry, $LDAPQuery)

    return $DirectorySearcher.FindAll()

}

# e.g
LDAPSearch -LDAPQuery "(samAccountType=805306368)"
LDAPSearch -LDAPQuery "(objectclass=group)"

# Enumerate every group's members, and every member of nested groups
foreach ($group in $(LDAPSearch -LDAPQuery "(objectCategory=group)")) {
$group.properties | select {$_.cn}, {$_.member}
 $groupName = $group.properties.cn
    $members = $group.properties.member
    echo "Group: $groupName"
    echo "Members:"
    foreach ($member in $members) {
        echo $member
    }
 }
```

