# SSRF 

	SSRF : Server-Side Request-Forgery
```
file:///etc/passwd
```
- most functionalities vuln to SSRF
```
First Request Line: 0.8%
Host header : 1.6%
Routing bug : 2.4%
Sensetive Integration : 3.2%
Headless browser : 5.6%
Securiy mechanism / library bug : 5.6%
Proxy: 6.5%
Webhooks : 6.5%
File storage integration : 7.3%
File upload : 13.7%
import by URL : 17.7%
App-specific/unknown : 27.4%

```
###### techniques:
- find administrative page,action
- fuzz for internal server
- always test refferer and user-agent headers
	- shellshock: `() { :; }; whoami`, more on [owasp](https://owasp.org/www-pdf-archive/Shellshock_-_Tudor_Enache.pdf)

###### defense bypass techniques:
- bypass black-listed input defense using other alternative like:
	- `2130706433` - `017700000001` - `127.1` - list from github 
- bypass white-listed input defense using features for ad hoc parsing and validation of urls:
	- `https://expected-host@evil-host` - `https://evil-host#expected-host` - `https://expected-host.evil-host` - URL-encode - combain them together 
	- read [more](https://portswigger.net/web-security/ssrf#:~:text=SSRF%20with%20whitelist%2Dbased%20input%20filters) on portswigger about white-listed input defense bypass
- ###### [URL PARSING INCONSISTENCIES](https://claroty.com/wp-content/uploads/2022/01/Exploiting-URL-Parsing-Confusion.pdf) pdf 
- multi slashes at url pegin
- use backslashes instead of using slashes 
- try to use pages redirection to access internal servers 
- example of how python libs treat urls
- try mutation headers like `X-Forwarded-For abcd: z` - `X-Forwared-For: 10.0.0.1`   
- X-Forwarded-For: 127.0.0.1
- X-Client-IP: 127.0.0.1
- Client-IP: 127.0.0.1
- X-HTTP-Method-Override: PUT
- some techniques parsers will not handle correctly:
```HTTP
GET / HTTP/1.1
https://127.0.0.1:11211:80/ 
http://foo@evil.com:80@google.com/
'\\o\\r\\a\\n\\g\\e.t\\w'

```
- A new Era of [SSRF](https://youtu.be/D1S-G8rJrEk)
- A new Era Of SSRF [whitepaper](https://www.blackhat.com/docs/us-17/thursday/us-17-Tsai-A-New-Era-Of-SSRF-Exploiting-URL-Parser-In-Trending-Programming-Languages.pdf )
![[python-libs-url-specification.png]]
- ![[host smuggling in curl - SSRF.png]]
- ![[host smuggling - SSRF.png]]

