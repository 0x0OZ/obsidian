# oAuth 




###### notes: 
- most common Oauth grant types :
	- Authorization code grant type  ![[oAuth Authorization code.png]]
	- Implicit grant type ![[oAuth Implicit grant type.png]]
- some client applications don't validate POST body
- state param is optional if not used could lead to attacks similar to CSRF attacks -> keep reading techniques <-
- some client apps don't verify the user registration data, which can be manipulated to steal other users accounts 
- oAuth servers config  `/.well-known/openid-configuration`
- oAuth servers jwt tokens stored in  `/.well-known/jwks.json`
- some oAuth register client apps dynamically which can lead to phishing or be used for further exploitation like SSRF
- openid worthy checking parameters:
```
redirect_uri       : redirect to after auth
client_uri         : url of client homepage 
policy_uri         : replying party client app policy
tos-uri            : relying party terms
initiate_login_uri :  https scheme that a third party can use to initiate a login by the RP. Also should be used for client-side redirection.
```
- `/.well-known/webfinger` openid endpoint that display information about users and resources on the request:
```
/.well-known/webfinger?resource=http://x/anonymous&rel=http://openid.net/specs/connect/1.0/issuer

Further enumeration using LDAP injection on openam

/.well-known/webfinger?resource=http://x/anyuser)(sunKeyValue=userPassword=A*)(%2526&rel=http://openid.net/specs/connect/1.0/issuer


```
- final thoughts: read about oAuth again.

###### techniques: 
- if state not used, intercept the redirect from Oauth service to client app that grants token and perform CSRF attack 
- if oAuth server doesn't correctly validate the origin in `redirect_uri` this can lead to CSRF attack `https://oauth-server.com/oauth?redirect_uri=https://evil.com`
- technique to bypass `redirect_uri` validation `https://default-host.com &@foo.evil-user.net#@bar.evil-user.net/`
- another technique bypass `redirect_uri` is to use `localhost` in the beggining or the end of the origin `https://oauth.com/oauth/redirect_uri=https://localhost.evil.com`
- editing `response_mode` from `query` to `fragment` can bypass validations
