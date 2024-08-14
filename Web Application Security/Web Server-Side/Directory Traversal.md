# Directory Traversal


###### techniques: 
- check operating system
- Check source code for every fetch request  text,images,js,etc..
- use static path
- encode path, encode path multi times 
- double path `....//` 
```python
c = GET["q"].replace("../","")
```
- don't remove original file path like `/var/www/html/cats/`
- use valid file extension using nullbyte `%00.ext`