# command injection


***techniques*** : 
- check every input ~!
- use for break out of command : `| - || - & - && - ;`
- for spaces : `%20` , `+` And `${IFS}`
- blind injection: 
	- use time delay
	- redirect output to accessable file
	- use OAST techniques [nslookup]: `$()` - \``(``) [backticks]
- use new lines to inject multiple commands : `0x0a` - `\n`
- bypass char filter using hex encoding

```sh
echo -e "\x2f\x65\x74\x63\x2f\x70\x61\x73\x73\x77\x64"
# /etc/passwd
```
