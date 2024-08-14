# Webcache Poisoning




###### notes: 
- find keyed and unkeyed inputs, quick way is `paramMiner` 
- find if there is any reflected input in the page or any loaded resources
- there are specific chars allowed in headers, invalid chars sometimes are chached
- some tricks for manipuliting host headers:
	- add port after the host header name
	- multiple headers
	- space before one of the duplicated headers
- using `x-forwarded-host:attacker` and `x-forwarded-scheme: http` might cause redirect to attacker page which can be used to perform xss by poisoning js urls 
- cachebuster is input added to the request to make a cache hit
- caches don't do normalisation but the backend to a normalisation on the path 
- rails split url path at semicolon `;` as some caches do, this called param cloaking
```http
GET /?keyed=1&unkeyed;keyed=arbiyary_code HTTP/1.1
```
- to inject arbitary codes in keyed parameters use `__` with unkeyed parameter to exploit the cache behaviour
```http
GET /?keyed=1&__unkeyed=arbitary_code HTTP/1.1
```
- some caches accept body in GET requests, called fat GET request 
- burp param miner support automatticaly adding cachebuster to requests in repeater
- last thing: internal caches are dummy 
###### useful headers:
```http
GET / HTTP/1.1
Host: attacker.com
X-Forwarded-Host: attacker.com
X-Forwarded-Scheme: http
X-Host: attacker.com
X-Original-URL: /path1\path2

```


##### some good resources: 
- more about exploiting css and js resources : [gadgets](https://portswigger.net/research/web-cache-entanglement#gadgets)
- web cache [deception](https://youtube.com/watch?v=mroq9eHFOIU) attacks, the [whitepaper](https://www.blackhat.com/docs/us-17/wednesday/us-17-Gil-Web-Cache-Deception-Attack-wp.pdf)
- [writeup 2020](https://enumerated.wordpress.com/2020/08/05/the-case-of-the-missing-cache-keys/)