# Subdomain Enumeration

## Active enumeration

### Crawling/Brute force

* [HostileSubBruteforcer][6]
* [amass][1]
* dnsenum:

  ```bash
  dnsenum -p 20 -s 100 --threads 5 example.com
  ```
* [SubBrute][10]
* [Gobuster][14]:

  ```bash
  gobuster dns -d example.com -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-110000.txt
  ```

#### Common dictionaries

* [SecLists/Discovery/DNS][13]

### DNS Zone transfer

Allowed zone transfer processes are usually the result of misconfiguration of the remote DNS server. They should be enabled (if required) only for trusted IP addresses. When zone transfers are available, we can enumerate all the DNS records for that zone.

* dig:

  ```bash
  dig axfr example.com @ns1.example.com
  ```

* host:

  ```bash
  host -t axfr example.com ns1.example.com
  ```

## Passive enumeration

* [Google search][11]:

  ```
  site: example.com -site:www.example.com
  ```

* [dnsdumpster][2]
* [Netcraft][9]: use the *"subdomain matches"* option.
* [Sublist3r][3]
* [VirusTotal][4]
* [crt.sh][5] or [Certificate Transparency][7]: extracting subdomains from HTTPS certificates.

## Further reading

* Use [httpstatus.io][8] to easily check status codes, response headers and redirect chains for multiple subdomains.

[1]: https://github.com/1522402210/amass-subdomain-enumeration-In-depth-
[2]: https://dnsdumpster.com/
[3]: https://github.com/aboul3la/Sublist3r
[4]: https://www.virustotal.com/gui/home/search
[5]: https://crt.sh/
[6]: https://github.com/nahamsec/HostileSubBruteforcer
[7]: https://developers.facebook.com/tools/ct/search/
[8]: https://httpstatus.io/
[9]: https://searchdns.netcraft.com/
[10]: https://github.com/TheRook/subbrute
[11]: https://www.google.com/
[13]: https://github.com/danielmiessler/SecLists/tree/master/Discovery/DNS
[14]: https://www.kali.org/tools/gobuster/
