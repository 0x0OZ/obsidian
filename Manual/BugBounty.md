 
### methodology
- check parsers logic
- manipulation checklist
	- url
	- parameters
	- Host headers
	- other headers
	- HTTP/HTTP2 implementation
- fuzzing checklist
	- hidden subdomains using ffuf
	- hidden headers
	- 



### Brief notes
- [bounty-targets-data](https://github.com/arkadiyt/bounty-targets-data) daily updated repository
-  [bbscope](https://github.com/sw33tLie/bbscope) target scope scanner
amass has viz feature to visualize results.
use free api keys for [amass,gau]
shodan has free [membership](https://help.shodan.io/the-basics/academic-upgrade) for academy students , has some limitations

## automation methodolodgy
run subfinder 
send results to subfinder again
store all results in subfinder's folder 
	send results to knockpy - currently commented out
send results to gau
store all results in gau's folder 
grep all domains/subdomains from gau and subfinder's folders/files
sort subdomains remove duplicates
1 
	grab ip addresses and remove duplicates
	send ip addresses to nmap, scan ALL TCP and UDP ports
2 
	grab the domains with web pages
	send them to nuclei project and WebHeckScanner
	Also
	# Done till here
	send them to fuzzers, fuzz for dirs and leaked files
	Also
	send them to web apps screenshooter 
	Also
	send them to web spiders
3 


## tools to use 
- use 
- nuclei
- [WebHeckScanner](https://github.com/grahamzemel/WebHeckScanner)
#### dns scan
- sub4finder - - discover valid subdomains??
- massdns - dns resolver 
- dnsrecon - dns resolver
- dnsscan - wordlist based
- knockpy - wordlist based
- gau
#### network scan
- masscan
- scanCanon
- rustscan
- nmap
> results go to dns reslover then dns scan
#### web apps screenshoter
- screenshoter
- witnessme
- depix - recovers passwords from pixelized screenshots
- httpscreenshot
### dir / files / urls fuzzer / finder
gobuster - dirs/files/dns and vhost busting tool
gospider - a fast web spider
LinksDumpster - extract links and possible endpoints
Urlgrab - go utility to spider for additional links
waybackurls - fetch all urls that the wayback knows about
gau - Fetch known URLs from AlienVault’s Open Threat Exchange, and much more.
FFUF - , find leaked files!
fuzzdb -  Dictionary of attack patterns and primitives for black box application fault injection and resource discovery.

### misc tools
**Httpx**
### browser extensions
**Webanalyze**
**Wappalyzer**

#### extra notes
 **Are there insecure requests being made?** **Are there possible login form vulnerabilities?** **Do you have more access or information than you need as a typical authenticated client?**
## Create A report
### Report Title
-   _Sql injection in xxx.xxx_
-   _XSS in xxx.xxx_
-   _Github leaked password_
-   _Admin account take over in xxx.xxx_
-   Sql injection in [xxx.xxx] parameter [xx=]
-   Non-self reflected XSS in [xxx.xxx] parameter [xx=]
-   Valid/internal data leaked via github for [xxx.xxx] domain
-   Default credentials/auth bypass led to admin account take over in [xxx.xxx]
### Description
-   _I found credentials leaked via github here https://github…… for domain xxx.xxx_
-   _I was testing xxx.xxx and I found sql injection on this panel_
-   _I found valid/internal credentials leaked via github here https://github…… for domain xxx.xxx this data was leaked in **1/6/2022** by user xxxx and user xxxx employee https://linkedin…….._
-   _I was testing xxx.xxx and I found sql injection on this panel as post/get request parameter x = -*-*-*-*-*-*-*-*-*–*_
### Steps to reproduce
-   _Visit this end point xxx.xxx/….._
-   _Try this payload [xxxxxxxx]_
-   _If not, you must add 2 steps to reproduce_
    -   1) steps to reproduce for triage team
    -   2) steps to reproduce for customer team
### POC: Proof of Concept
-   Sql injection example:
    -   Dbs name or host name is enough
    -   Not ethical to dig deeper
-   Leaked and/or default/weak credentials example:
    -   Screenshot and/or recorded video is enough
    -   Not ethical to dig deeper
### Impact
-   For example, in a normal web app page, sqli in this panel leads to:
    -   Critically sensitive data exposure about users
    -   Critically sensitive data exposure about admins
    -   Critically sensitive data exposure about systems
## another report Template
```bash
## Title:  
[Title of bug, i.e. “[bug type] on [domain] leading to [list possible consequences]]## Summary:  
[add summary of the vulnerability, what can it do to harm the company/website/app?]## Steps To Reproduce:  
[add details for how others can reproduce the issue. The better you do this, the sooner you can possibly get a reward & it shows professionalism]  
1. [add step]  
2. [add step]  
3. [add step]## Supporting Material/References:  
[list any additional material (e.g. screenshots, logs, Burpsuite request/responses, etc.)]  
* [attachment / reference]##IP Address:  
[IP Address for identifying your traffic]##Timestamp:  
[Date and time of testing]
```
### Markdown
```
# used for headers
`` `used for commands`
"`used for request/response leaked codes`"

```
- [bugcrowd report](https://www.bugcrowd.com/resources/levelup/how-to-write-excellent-reports-techniques-that-save-triagers-time-and-mistakes-that-should-be-avoided-in-reports/)


still vulnerable to prototype pollution:
jQuery BBQ 
jquery-deparam
MooTools More
yui3
phpjs


