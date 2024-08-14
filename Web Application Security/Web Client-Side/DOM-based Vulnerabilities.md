# DOM-based vulnerabilities


Example sink

###### DOM-based vuln  can be in:
- [DOM XSS](https://portswigger.net/web-security/cross-site-scripting/dom-based) LABS
- [Open redirection](https://portswigger.net/web-security/dom-based/open-redirection) LABS
- [Cookie manipulation](https://portswigger.net/web-security/dom-based/cookie-manipulation) LABS
- [JavaScript injection](https://portswigger.net/web-security/dom-based/javascript-injection)
- [Document-domain manipulation](https://portswigger.net/web-security/dom-based/document-domain-manipulation)
- [WebSocket-URL poisoning](https://portswigger.net/web-security/dom-based/websocket-url-poisoning)
- [Link manipulation](https://portswigger.net/web-security/dom-based/link-manipulation)
- [Web message manipulation](https://portswigger.net/web-security/dom-based/web-message-manipulation)
- [Ajax request-header manipulation](https://portswigger.net/web-security/dom-based/ajax-request-header-manipulation)
- [Local file-path manipulation](https://portswigger.net/web-security/dom-based/local-file-path-manipulation)
- [Client-side SQL injection](https://portswigger.net/web-security/dom-based/client-side-sql-injection)
- [HTML5-storage manipulation](https://portswigger.net/web-security/dom-based/html5-storage-manipulation)
- [Client-side XPath injection](https://portswigger.net/web-security/dom-based/client-side-xpath-injection)
- [Client-side JSON injection](https://portswigger.net/web-security/dom-based/client-side-json-injection)
- [DOM-data manipulation](https://portswigger.net/web-security/dom-based/dom-data-manipulation)
- [Denial of service](https://portswigger.net/web-security/dom-based/denial-of-service)


###### notes:
- simple postmessage  
```html
<iframe src="//vulnerable-website" onload="this.contentWindow.postMessage('print()','*')">
```
- `javascript:alert()` is required to perform xss on location.href property
- `document.domain` sink can lead to unrestricted cross-domain between the infected site and a weak subdomain if it was infected to manipulation
- read about [DOM clobbering](https://portswigger.net/web-security/dom-based/dom-clobbering)
- DOMPurify enables you to use `cid:` which let us injecet encoded quotes that leads to DOM colebbering

###### sinks for each DOM vulnerability type:
- sinks for dom javascript injection
```javascript
eval() 
Function() 
setTimeout()
setInterval()
setImmediate()
execCommand()
execScript()
msSetImmediate()
range.createContextualFragment()
crypto.generateCRMFRequest()
``` 
- sinks for open-redirect vuln
```javascript
location
location.host
location.hostname
location.href
location.pathname
location.search
location.protocol
location.assign()
location.replace()
open()
element.srcdoc
XMLHttpRequest.open()
XMLHttpRequest.send()
jQuery.ajax()
$.ajax()
```
- sinks for link-manipulation:
```javascript
element.href
element.src 
element.action
``` 
- sinks for ajax request-header manipulation:
```javascript
XMLHttpRequest.setRequestHeader() 
XMLHttpRequest.open()
XMLHttpRequest.send()
jQuery.globalEval()
$.globalEval()
```
- sinks for local path file manipulation:
```javascript
FileReader.readAsArrayBuffer()
FileReader.readAsBinaryString()
FileReader.readAsDataURL()
FileReader.readAsText()
FileReader.readAsFile()
FileReader.root.getFile()
```
- DOM-data manipulation - virtual site defacement
```javascript
script.src
script.text
script.textContent
script.innerText
element.setAttribute()
element.search 
element.text
element.textContent
element.innerText
element.outerText
element.value
element.name
element.target
element.method
element.type
element.backgroundImage
element.cssText
element.codebase
document.title
document.implementation.createHTMLDocument()
history.pushState()
history.replaceState()
```
- other sinks for DOM vulns 
```javascript
document.cookie
document.domain
postMessage() // web message manipulation on [eventListener]
executeSql() // client-side sql injection
sessionStorage.setItem() // html5 storage manipulaton
localStorage.setItem()   // html5 storage manipulaton
document.evaluate() // client-side xpath injection
element.evaluate()  // client-side xpath injection
JSON.parse()
jQuery.parseJSON()
$.parseJSON()
requestFileSystem() // DOM-based dos 
RegExp() // DOM-based dos
```
