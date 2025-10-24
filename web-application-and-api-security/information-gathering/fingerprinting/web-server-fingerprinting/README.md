# Web Server Fingerprinting

## Online tools

* [Netcraft][1]: it can show the webserver version and historical OS information about the domain.

## CLI tools

### Nmap

```bash
nmap -Pn -n -vvv -sV --version-all -p80,433 10.10.10.10
```

### Ncat (for HTTP)

```bash
nc example.com 80
HEAD / HTTP/1.0
```

### OpenSSL (for HTTPS)

```bash
openssl s_client -connect example.com:443
HEAD / HTTP/1.0
```

### Httprint

```bash
httprint -P0 -h 10.10.10.10 -s /usr/share/httprint/signatures.txt
```

* Options used:
  * `-P0`: to avoid pinging the host.
  * `-h <target_hosts>`.
  * `-s`: set the signature file to use.

## Search for known vulnerabilities related to the web server

### SearchSploit

```
searchsploit apache 2.2
```

### Google search

```
site:cvedetails.com apache 2.2 vulnerabilities
```
  
## Further reading

* [Testing for Web Application Fingerprint][2]


[1]: https://searchdns.netcraft.com/
[2]: https://wiki.owasp.org/index.php/Testing_for_Web_Application_Fingerprint_(OWASP-IG-004)

