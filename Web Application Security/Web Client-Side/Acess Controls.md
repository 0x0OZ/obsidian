# Acess Controls


###### Acess Controls Models
- Discretionary access control {**DAC**}:
	- access based upon users named or groups of users Acess Controls
- Mandotary access controls {**MAC**}:
	- centrally controled system of access control in which acess resources is constrained 
	- the users and the owners of resources have no capibility to delegate or modify access rights for their resources 
- Role-Based access controls {**RBAC**}:
	- named roles are defined to  which access privileges are assigned
	- Users are then assigned to single or multiple roles
	- real-life example is discord server roles-management

###### Access Controls Categories:
-   [Vertical access controls](https://portswigger.net/web-security/access-control#vertical-access-controls) : higher and lower account level 
-   [Horizontal access controls](https://portswigger.net/web-security/access-control#horizontal-access-controls) : in same account level
-   [Context-dependent access controls](https://portswigger.net/web-security/access-control#context-dependent-access-controls) : based upon the state of the application or the user's interaction with it - *you can't edit your shopping cart after payment* -


###### techniques:
- crawl the site ,read pages sources, find admin pages,functions,check for any administrative cookies or headers
- bypass admin pages,functions access protection via headers override
	- headers to override the URL  `X-Original-URL` - `X-Rewrite-URL`
	 ```http
	 POST / HTTP/1.1
	 X-Original-URL: /admin/deleteUser
```
-  try several requests methods to perform unauthorized actions
- multi-steps don't always have the same access control 


