




###### Headers list
```
X-Forwarded-Proto: http
X-HTTP-Host-Override: HEAD
X-Rewrite-URL: /admin
X-Original-URL: localhost
```
- `X-Original-URL` - `X-Rewrite-URL`
-   `X-Host` 
-   `X-Forwarded-Server`
-   `Forwarded`
-  `X-Real-Ip`
- `Fastly-Host: victim.com
- `x-forwarded-proto`
- X-Forwarded-Proto : https

```HTTP
TRACE / HTTP/1.1
PURGE / HTTP/1.1
```
- hop-by-hop headers [removed at first hop]
```HTTP
GET / HTTP/1.1
Keep-Alive
Transfer-Encoding
TE
Connection
Trailer
Upgrade
Proxy-Authorization
Proxy-Authenticate
```
- adding custom hop-by-hop header using `connection`
```HTTP
GET / HTTP/1.1
X-Foo: hop-by-hop
X-Bar: hop-by-hop
Connection: close,X-Foo,X-Bar
```
