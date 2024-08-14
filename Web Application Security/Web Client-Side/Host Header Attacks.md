# Host Header Attacks


## possible exploits for Host Header Attacks
-   [Password reset poisoning](https://portswigger.net/web-security/host-header/exploiting#password-reset-poisoning) LABS
-   [Web cache poisoning](https://portswigger.net/web-security/host-header/exploiting#web-cache-poisoning-via-the-host-header) LABS
-   [Exploiting classic server-side vulnerabilities](https://portswigger.net/web-security/host-header/exploiting#exploiting-classic-server-side-vulnerabilities) LABS
-   [Bypassing authentication](https://portswigger.net/web-security/host-header/exploiting#accessing-restricted-functionality) LABS
-   [Virtual host brute-forcing](https://portswigger.net/web-security/host-header/exploiting#accessing-internal-websites-with-virtual-host-brute-forcing)
-   [Routing-based SSRF](https://portswigger.net/web-security/host-header/exploiting#routing-based-ssrf) LABS

***techniques :***
 - check all Host Headers using param miner like:
	 -   `X-Host` 
	 -   `X-Forwarded-Server`
	 -   `X-HTTP-Host-Override`
	 -   `Forwarded`
	 -  `X-Real-Ip`
 - edit Host port
 - request internal host on the server : `192.168.1.1` ***or*** `localhost` ***etc*...** [SSRF]
 - request subdomains via domain 
 - absolute url in [GET request](https://portswigger.net/web-security/host-header/exploiting#:~:text=GET%20https%3A//vulnerable%2Dwebsite.com/%20HTTP/1.1)
 - use duplicated header :
	 - add line wapper in first duplicated header ` Host:`
- malformed request line:
	- `GET @private-intranet/example HTTP/1.1`
		- some proxies fail to validate the request
- *read more about how to bypass reverse proxies*

#### **what is the purpose for using host headers?~**
- **virtual hosting** : **each host for specific service**
- **hosts are widely sapreted** : *servers hosted on distinct servers* like `CDN`
more to read: [cracking the lens targeting https hidden attack surface](https://portswigger.net/research/cracking-the-lens-targeting-https-hidden-attack-surface)