# Information Disclosure 




###### techniques: 
- crawl site, check sitemap, robots.txt
- fuzz hidden directories,files, backup files 
- read pages sources, requests and responses headers
	- look for idor
- use custome http methods to find secret headers like `TRACE`
	- `TRACE` : if enabled it let the server reflects the request which is generaly good for enumeration especially if there is reverse proxy  
- *extra: learn more to use git.*




