# Running SQLMap on an HTTP Request

This document provides a quick guide reference to perform SQLMap on HTTP requests and the tools available to streamline this.

---
## Curl Commands

Modern web browsers come packaged with the ability to inspect network traffic using the dev tools.

`Development Tools`

You can copy network requests in several different formats. ```CURL``` is one of them.

Utilizing ChatGPT normalize the curl request for SQLMap.

`GET` parameters are provided with the usage of option `-u/--url`. 

```bash
sqlmap 'http://www.example.com/?id=1' -H 'User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:80.0) Gecko/20100101 Firefox/80.0' -H 'Accept: image/webp,*/*' -H 'Accept-Language: en-US,en;q=0.5' --compressed -H 'Connection: keep-alive' -H 'DNT: 1'
```

`Post` parameters are provided with the `--data` flag

```bash
sqlmap 'http://www.example.com/' --data 'uid=1&name=test'
```
In this example POST parameters `uid` and `name` will be tested for SQLi vulnerability. 

We can narrow this down the parameter we want to test by using the `-p` argument.

```bash
sqlmap 'http://www.example.com/' --data 'uid=1&name=test -p uid'
```

Optionally you can use a special marker `*` as follows:

```bash
sqlmap 'http://www.example.com/' --data 'uid=1*&name=test'
```

## Utilizing Full HTTP Request using Text Files

This is a request captured using Burp Suite.
```html
GET /?id=1 HTTP/1.1
Host: www.example.com
User-Agent: Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:80.0) Gecko/20100101 Firefox/80.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Connection: close
Upgrade-Insecure-Requests: 1
DNT: 1
If-Modified-Since: Thu, 17 Oct 2019 07:18:26 GMT
If-None-Match: "3147526947"
Cache-Control: max-age=0
```
We can pass it to SQLMap using the `-r` argument after saving it to a text file.
```bash
sqlmap -r req.txt
```
You can either pass the `--data` argument or pass the wildcard (`*`) in the request text file to specify which parameter you want to test for vulnerabilities

```
Tip: You can use the --random-agent flag to randomize your user agent. By default sql map will disclose itself which can result in being blocked immediately.
```

All fields are testable for SQLi Injection. In the module we tested Cookies, JSON, and plain old parameters passed in a Post request.
