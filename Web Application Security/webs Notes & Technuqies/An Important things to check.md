```bash
curl -v -H 'Cookie: 0=1' https://automattic.com/?cb=123 | fgrep Cookie

```


#### random important things to check
- DNS cache poisoning: account takeover via [password reset](https://sec-consult.com/blog/detail/forgot-password-taking-over-user-accounts-kaminsky-style/)
- prototype pollution in `sequence` [sink](https://portswigger.net/research/widespread-prototype-pollution-gadgets) in chrome `__proto__[sequence]=alert()-`
- reverse proxies [cheat sheet](https://github.com/GrrrDog/weird_proxies)

- Amazon 'Magic' Ip to retrieve user data and instance metadata to a specific instance, require no auth if accessed from AWS server
```
169.254.169.254
```
- the WinAPI function FindFirstFile() note (php function vuln):
during the trace of call stack it was found out that the character > gets replaced with ?, character < transforms to \*, and " (double quote) is replaced by a . (dot). This bug has already been described in MSDN in 2007.
 **PHP** is a good victim to use this note to abuse it's file_get_content() and other functions.

