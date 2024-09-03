# Windows Active Directory Enumeration.

#### check-list
- [PayloadAllTheThings](https://gitlab.com/pentest-tools/PayloadsAllTheThings/-/blob/6bcd2e8a6a39d26a547a70d83dfebef4c2c6f801/Methodology%20and%20Resources/Active%20Directory%20Attack.md)
- [S1ckB0y1337](https://github.com/S1ckB0y1337/Active-Directory-Exploitation-Cheat-Sheet)
- check ldap for system/users informations
- check smb for shares
- check rpcbind for system/users informations
- check impacket examples scripts
- generate usernames from `fname lname` wordlist [namemash.py](https://gist.githubusercontent.com/superkojiman/11076951/raw/74f3de7740acb197ecfa8340d07d3926a95e5d46/namemash.py)
```bash
# enuemrate valid users through kerberos
./kerbrute
# enumerate users do not require Kerberos preauthentication
GetNPUsers.py EGOTISTICAL-BANK.local/ -no-pass -usersfile usernames
# request other users krbt hash if you have a user cerdentials
GetUserSPNs.py
# runas user in a domain using credentials, or use bloodhound-python
runas /netonly /user:support cmd.exe
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