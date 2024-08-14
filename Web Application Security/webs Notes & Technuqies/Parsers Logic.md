
- 2018 conference - Orange Tsai
`http://example.com/foo;name=orange/bar/`
```cs
|          | behaviour             |
|----------|-----------------------|
| apache   | /foo;name=orange/bar/ |
| Nginx    | /foo;name=orange/bar/ |
| IIS      | /foo;name=orange/bar/ |
| Tomcat   | /foo/bar/             |
| Jetty    | /foo/bar/             |
| WildFly  | /foo                  |
| WebLogic | /foo                  |
```