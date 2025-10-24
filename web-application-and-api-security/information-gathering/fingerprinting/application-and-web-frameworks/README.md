# Application and Web Frameworks Fingerprinting

## Manual search

* HTTP response headers.
* Footer credits.
* META tags in HTML pages.

## Browser tools

* [Wappalyzer][2] extension.
* Cookies may also reveal information about the technology stack (e.g. `PHPSESSID` for `PHP`, `ASPSESSIONID` for `.NET`, `JSESSION` for `JAVA`, etc).

## CLI tools

### [WhatWeb][1]

```
whatweb example.com -v
```

## Search for known vulnerabilities related to the technology stack of the website

### Google search

```
site:cvedetails.com php 5.5.23 vulnerabilities
```

[1]: https://github.com/urbanadventurer/WhatWeb
[2]: https://www.wappalyzer.com/
