
###### upload reverse shell to page via Opache engine in PHP 7
##### this technique is to bypass write permissions to overwrite php file in uploads directory when caching is enabled

##### notes:
- ##### need `phpinfo()` file to perform the attack
- Opache stores data in memoy and offers to store it in filesystem such as `/tmp/opache`
- under `opache` folder data stored under `[system-id]` folder which is md5 hash comprised of PHP’s version, Zend’s extension build ID and the size of various data type.
- This data can be found in `phpinfo()` file 
- [This](https://github.com/GoSecure/php7-opcache-override) tool to calculate the md5 hash aka `system-id`
- [This](https://www.gosecure.net/blog/2016/05/26/detecting-hidden-backdoors-in-php-opcache/) article explain how to detect this attack for blue teaming
##### steps
- get `system-id` hash
- start local php page with cache enabled `php -S 127.0.0.1:8080` and the reverse shell file in it
- after opening the localhost page go to `opache` folder and edit hash in reverse shell `bin` file to the victim one's, and upload to target



source : [gesource.net](https://www.gosecure.net/blog/2016/04/27/binary-webshell-through-opcache-in-php-7/)