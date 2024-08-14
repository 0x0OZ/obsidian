# JWT Attacks


###### notes:
- JWT extended by JWS and JWE
- JWS are JWT encoded
- JWE are JWT encrypted
- usually JWT tokens refers to JWS tokens 
- most JWT libs has `verify()` and `decode()` functions if `verify()` not used to verify signiture, any JWT is valid
- JWK tokens stored in `/.well-known/jwks.json` or `/jwks.json`
- it's possible to bypass JWT verify by removing token signiture, editing the `alg` to `none` in header, and leaving a trailing dot at the end
- bruteforcing jwt token using `hashcat` and public passwords wordlists: `hashcat -a 0 -m 16500 jwt.txt passwords.txt`
- only `alg` parameter is mandotary in JWT header, here list of interesting optional parameters:
	-   `jwk` (JSON Web Key) - Provides an embedded JSON object representing the key.
	-   `jku` (JSON Web Key Set URL) - Provides a URL from which servers can fetch a set of keys containing the correct key.
	-   `kid` (Key ID) - Provides an ID that servers can use to identify the correct key in cases where there are multiple keys to choose from. Depending on the format of the key, this may have a matching `kid` parameter.
- to perform JWK injection add `jkw` parameter to the header includes public RSA made, edit the RSA and the header `kid` to be the same.
- injecting JKU by adding `jku` paramter with url to the public key on the exploit server, note that servers validate the url use techniques mentioned in [[SSRF]]
- injecting `kid` if the server support symetric keys and it used as key path, create one and assign `k` parameter value to null `AA==` and the `kid` in key and JWT header to `/dev/null` will lead to return valid signature as they both equals `null`
- if the key stored in database, server can be vuln to [[SQL Injection]]
- other interesting [JWT header parameters](https://portswigger.net/web-security/jwt#:~:text=Other%20interesting%20JWT%20header%20parameters) on portswigger
- algorithm-confusion attack can arise when using symmetric `alg` instead of asymmetric by doing:
	- exposing the public key of the JWT 
	- convert the public key to PEM then base64
	- create symmetric key and edit `k` value to the exposed key in b64 format
	- in JWT key edit `alg` in header to `HS256`, sign using symmetric key and send.
- if the public key is not exposed, get any two JWK tokens from the server by logging twice for example, use [CVE-2017-11424](https://github.com/silentsignal/rsa_sign2n/tree/release/CVE-2017-11424 "CVE-2017-11424") to calculate the public key, using docker `docker run --rm -it portswigger/sig2n <token1> <token2>`
- more JWT attacks on [github](https://github.com/ticarpi/jwt_tool/)
