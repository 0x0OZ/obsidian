# Insecure Deserialization 

###### notes: 
- php serialized data format:
	- `O:4:"User":2:{s:4:"name":s:6:"carlos"; s:10:"isLoggedIn":b:1;}`
	-   `O:4:"User"` - An object with the 4-character class name `"User"`
	-   `2` - the object has 2 attributes
	-   `s:4:"name"` - The key of the first attribute is the 4-character string `"name"`
	-   `s:6:"carlos"` - The value of the first attribute is the 6-character string `"carlos"`
	-   `s:10:"isLoggedIn"` - The key of the second attribute is the 10-character string `"isLoggedIn"`
	-   `b:1` - The value of the second attribute is the boolean value `true`
- `unserialize()` method in php to deserialize data, and `readObject()` for java
- java serialized data format is binary and always begin with the same bytes encoded as `ac ed` in hexadecimal and `rO0` in Base64
- Magic methods is the use of constructors and methods to increase the impact.
- Gadget Chains is invoking a method from another to reach a critical method that cause the highest impact.
- 
- you can sometimes see the php source file by appending a tilde `~` to the filename - Forgetten backup filename
- tools like `ysoserial` can be used to choose gadget chains for a library to exploit insecure deserialization 
- burpsuite has extensions to generate full requests containing payloads to scan and exploit insecure deserialization
- tools to generate gadget chains in libraries:
```
ysoserial
PHPGGC
```
- java runtime exec payloads encode https://ares-x.com/tools/runtime-exec/
- rails gadget chain on [payloadAllTheThings](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Insecure%20Deserialization/Ruby.md)
- another [rails](https://forum.portswigger.net/thread/exploiting-ruby-deserialization-using-a-documented-gadget-chain-840f591f) gadget chain generator 

##### Not finished not continue reading [here](https://portswigger.net/web-security/deserialization/exploiting)
- insecure Deserialization in [JSON](https://www.youtube.com/watch?v=oUAeWhW5b8c), the [whitepaper](https://www.blackhat.com/docs/us-17/thursday/us-17-Munoz-Friday-The-13th-JSON-Attacks-wp.pdf)
- advanced insecure deserialization in php, [youtube](https://www.youtube.com/watch?v=GePBmsNJw6Y) video
