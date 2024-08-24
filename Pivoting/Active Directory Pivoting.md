

- Powershell portscanner
```powershell
1..1024 | % {echo ((New-Object Net.Sockets.TcpClient).Connect("192.168.50.151", $_)) "TCP port $_ is open"} 2>$null
```

```powershell
# view shares
net view \\dc01 /all
```