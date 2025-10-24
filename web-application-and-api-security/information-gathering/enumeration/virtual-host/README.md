# Virtual Host Enumeration

An IP address can be associated to different DNS names such as `example.com` and `domain.com`. This is 1-to-N relationship and it's possible due to the `Host` HTTP header.

```bash
curl http://10.10.10.10 -H "Host: example.com"
```

```bash
curl http://10.10.10.10 -H "Host: domain.com"
```

## wfuzz

```bash
wfuzz -u 10.10.10.10 -w local-domains.lst -H "Host: FUZZ" --hw 1402 -c | tee vhosts-10.10.10.10.wfuzz
```

```bash
wfuzz -u example.com -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-110000.txt -H "Host: FUZZ.example.com" --hw 1402 -c | tee vhosts-example.com.wfuzz
```

## gobuster

```bash
gobuster vhost -u example.com -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-110000.txt -o vhosts-example.com.gobuster
```
