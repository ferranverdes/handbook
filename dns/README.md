# 🐝 DNS

* A DNS record starts with a domain name, usually a FQDN. If anything other than a FQDN is used, the name of the zone the record is in will automatically be appended to the end of the name.

|Source: [*"Web Application Penetration Testing" from INE*.][1]|
|:--:|

## How DNS works

![How DNS works][8]

|Source: [*"What is DNS and How it works" from FoxuTech*.][9]|
|:--:|

## DNS zone

* The DNS is broken up into many different zones. A DNS zone is a portion of the DNS namespace that is managed by a specific organization or administrator.
* It can be a primary or secondary zone, also called master o slave zone. The primary zone is the master record and is where the administrator makes changes. When the administrator updates it, all changes will be replicated to the slave zone through a process called Zone Transfer.
* While a domain is a logical division of the DNS namespace, a DNS zone is a physical division where DNS records are stored.
* Each DNS zone has a file called zone file that contains all the records for every domain within the zone. A zone file is a plain text that must always start with a Start of Authority (SOA) record, which contains important information including contact information for the zone administrator.
* A DNS zone starts at a domain within the DNS tree and can also extend down into subdomains so that multiple subdomains can be managed by one entity.
* A common mistake is to associate a DNS zone with a domain name or a single DNS server. In fact, a DNS zone can contain multiple subdomains and multiple zones can exist on the same server. DNS zones are not necessarily physically separated from one another, zones are strictly used for delegating control.

|Source: [*"What is a DNS zone?" from Cloudflare*.][5]|
|:--:|

![DNS zones representation][3]

|Source: [*"MCSA Windows Server 2016 Certification Guide: Exam 70-741" from Packt*.][4]|
|:--:|

## Zone transfer

* Zone transfer is the process of copying the contents of the zone file on a primary DNS server to a secondary DNS server.

|Source: [*"Layer 7: The Application Layer" from ScienceDirect*.][2]|
|:--:|

## DNS records

### SOA

* The `Start Of Authority (SOA)` record indicates the beginning of a zone and it should occur first in a zone file.
* There can be only one SOA record per zone.
* Defines important information about a domain or zone such as the email address of the administrator, when the domain was last updated, and how long the server should wait between refreshes.

|Source: [*"What is a DNS SOA record?" from Cloudflare*][6] and [*"Web Application Penetration Testing" from INE*.][1]|
|:--:|

### PTR

* The `Pointer (PTR)` record is the reverse of an A or AAAA record. A PTR record resolves IPv4 or IPv6 addresses to domain names. Reverse DNS lookups use PTR records.
* Zones with PTR records are called *reverse zones*.

|Source: [*"Know the eight most common DNS records" from BlueCat*][7] and [*"Web Application Penetration Testing" from INE*.][1]|
|:--:|

### CNAME

* The `Canonical Name (CNAME)` record maps an alias hostname to an A record hostname.

|Source: [*"Web Application Penetration Testing" from INE*.][1]|
|:--:|


[1]: https://my.ine.com/INE/courses/38316560/web-application-penetration-testing
[2]: https://www.sciencedirect.com/topics/computer-science/zone-transfer
[3]: ./images/dns-zones-representation.png
[4]: https://subscription.packtpub.com/book/cloud_and_networking/9781789535600/2/ch02lvl1sec14/configuring-dns-zones
[5]: https://www.cloudflare.com/en-gb/learning/dns/glossary/dns-zone/
[6]: https://www.cloudflare.com/en-gb/learning/dns/dns-records/dns-soa-record/
[7]: https://bluecatnetworks.com/blog/know-the-eight-most-common-dns-records/
[8]: ./images/how-dns-works.png
[9]: https://foxutech.com/what-is-dns-and-how-it-works/

