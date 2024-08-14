
##### enumerate nfs 
```bash
showmount -e $target 
mount -t nfs $target:/ /tmp/f/nfs

```
##### rcpbind 
```bash
nmap -sSUC -p 111 $host
# connect to rpc using null session
rpcclient -U "" $host -N
rpcclient -U "ldap" $host -c "queryuser support" --password='nvEfEK16^1aM4$e7AclUf8x$tRWxPWO1%lmz'
# dump rpc endpoints
rpcdump.py $host
### rpc commands
# set a user password if connected to rpc and has permissions
setuserinfo2 audit2020 23 'Password!23'
# query users descriptions
querydispinfo
# enum users
enumdomusers
###
rpcinfo -p $host
```
- ldap enumeration [hacktricks](https://book.hacktricks.xyz/network-services-pentesting/pentesting-ldap)
```bash
[# anonymous/credentialed ldap data dump
ldapsearch -LLL -x -H ldap://$host -b "" -s base -L "objectclass=*" namingcontexts
ldapsearch -H ldap://$host/ -x -s base -b '' "(objectClass=*)" "*" +
ldapsearch -LLL -x -H ldap://$host -b '' -s base '(objectclass=*)'
ldapsearch -LLL -x -H ldap://$host -b 'DC=htb,DC=local'
ldapsearch -LLL -x -H ldap://$host -b '' -s base '(objectclass=Person)'
ldapsearch -x -LLL -H ldap://$host -D support\\ldap -W -b 'DC=support,' 
ldapsearch -x -LLL -H ldap://$host -D support\\ldap -W -b 'DC=support,' -s sub '(objectClass=*)' 'givenName=username*'
]()

# null creds
ldapsearch -x -H ldap://$host -D '' -w '' -b "DC=aka,DC=sex"
# check creds
ldapsearch -x -H ldap://<IP> -D '<DOMAIN>\<username>' -w '<password>' -b "DC=<1_SUBDOMAIN>,DC=<TLD>"

```
- smb enumeration
```bash
# get password policy 
crackmapexec smb --pass-pol $host
# password spray using crackmapexec
crackmapexec smb $host -u users -p NewIntelligenceCorpUser9876 --continue-on-success
# list shares using crackmapsexec
crackmapexec smb $host --shares -u '' -p ''
#list shares using 
smbmap -H $host
# smb null session
smbclient -N -L //$host/
# list shares using smbclient 
smbclient -L //$host/
# install all files in smb share
smbclient //$host/
recurse ON
prompt OFF
mget *
# mount smb share
mount -t cifs -o username=user //10.10.10.10/share /mnt/smb/
mount -t cifs -o username=<share user>,password=<share password> //WIN_PC_IP/<share name> /mnt

```
- dns enumeration
```bash
dnsrecon -d domain -r network/range
# enumeration subdomains if dns is open
dig axft domain @$ip
# zone transfer
dig axfr domain @$ip
#
nslookup
server $ip
127.0.0.1
```
- wordpress enumeration
```bash

# scan for vulnerable plugins
wpscan --url localhost -e vp
# scan for vulnerable themes
wpscan --url localhost -e vt
# passive and active scan
wpscan --url localhost --plugins-detection mixed --plugins-version-detection mixed
# aggressive ---mode---
wpscan --url localhost --detection-mode aggressive

```
-  ftp enum
```sh
quote PASSV
```