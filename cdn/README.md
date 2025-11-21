# Content Delivery Network (CDN)

## Tasks

* Use a Content Delivery Network (CDN) whenever possible to add availability and security to you applications.

## Introduction

* A `Content Delivery Network (CDN)` is a geographically distributed network of proxy servers and their data centers that work together to provide fast delivery of Internet content.
* With CDNs, content providers need only to supply an `origin server` and the CDN distributes the content to end users through a set of `edge servers` it has deployed in the network. Ideally, this reduces response time and latencies and improves user experience, due to the fact that the data is served from strategic geographic locations around the world that are closer to the end user.

![CDN graphical representation][1]

|Source: [*"Implementing a CDN to optimize video streaming quality and scalability" from StriveCast*][2].|
|:--:|

### DNS resolution

* When the browser makes a DNS request for a domain name that is handled by a CDN, the server handling DNS requests for the domain name looks at the incoming request to determine the best set of servers to handle it. At it's simplest, the DNS server does a geographic lookup based on the DNS resolver's IP address and then returns an IP address for an edge server that is physically closest to that area. This means that the CDN dynamically chooses a server to route the request to.
* To enable fast reaction to dynamic resource changes, the answer returned by the CDN's DNS server has a small TTL. This approach is transparent to the client.
* Keep in mind that companies may optimize their CDNs in other ways as well, for instance, redirecting to a server that is cheaper to run or one that is sitting idle while another is almost at capacity. In any case, the CDN smartly returns the best possible IP address to handle the request.

### Accessing content

* Edge servers are *proxy caches* that work in a manner similar to the browser caches. When a request comes into an edge server, it first checks the cache to see if the content is present. The cache key is the entire URL including query string (just like in a browser). If the content is in cache and the cache entry hasn't expired, then the content is served directly from the edge server.
* If, on the other hand, the content is not in the cache or the cache entry has expired, then the edge server makes a request to the origin server to retrieve the information. The origin server is the source of truth for content and is capable of serving all of the content that is available on the CDN. When the edge server receives the response from the origin server, it stores the content in cache based on the HTTP headers of the response.
* An edge server is a proxy, the origin server is the one that tells the edge server exactly what content should be returned for a particular request. The origin server may be running Java, Ruby, Node.js, or any other type of web server and, therefore, can do anything it wants. The edge server does nothing but make requests and serve content.

### Static files

* The reason JavaScript, CSS, images, Flash, audio, and video are frequently served from CDNs is precisely because they don't change that frequently. Once the cache is primed with content, all users benefit.

### Caching

* Caching is at the heart of CDN services.
* Web developers use HTTP cache headers to mark cacheable web content and set cache durations. Using cache headers, you can control your caching strategy by establishing optimum cache policies that ensure the freshness of your content (e.g. `Cache-Control: max-age=3600` means that the resource can be cached for no longer than an hour before it must be refetched from the origin content.
* Meticulously tagging each resource, or even groups of resource, can be overwhelming and prone to inefficiencies. Modern-day CDNs allow you to forgo the practice by employing intelligent mechanisms able to override cache header directives when they are discovered to be suboptimal. Most commonly, these mechanisms enable the caching of dynamic content marked as uncacheable by default, even when freshness is not an issue.
* A browser will cache the resources for a while and the CDN will cache the resources too. This means a resource may be cached in at least two places and users will receive the cached version instead of a new one for quite a while. There are several ways to work around this. The [YUI][5] library uses directories containing the version number of the library to differentiate file versions. It's also common to append identifiers to the end of a filename, such as an MD5 hash or source control revision. Any of these techniques ensures that users are receiving the most up-to-date version of the file while maintaining far-future `Expires` headers on all requests.
* HTTP HEAD requests are commonly used by proxies or CDN's to efficiently determine whether a page has changed without downloading the entire body. Also, the `If-Modified-Since` HTTP header indicates the time for which a browser first downloaded a resource from the server. This helps determine whether or not the resource has changed since the last time it was accessed. If the HTTP response status of a particular resource is `304 Not Modified`, this means that the file has not changed and there is no need to download it again:
  ```
  If-Modified-Since: Fri, 10 Jun 2022 09:00:00 GMT
  ```

### Limitations

* Although DNS-based server selection is transparent and general, it has two inherent limitations. First, it is based on the implicit assumption that clients are close to their local DNS servers. The CDN DNS server performing dynamic request routing only has access to the client's local DNS server's IP address, it actually does not know the client's own IP address. However, the assumption that clients are close to their local DNS server may not be valid.
* The second inherent limitation of DNS-based server selection is that a single request from a local DNS server can represent differing numbers of Web clients - this is called the *hidden load factor*. The *hidden load factor* has implications on a CDN's load balancing algorithm. For example, a DNS request from a local DNS server of a large ISP may result in many more web requests than a DNS request from a local DNS server of a small site. CDNs need to be able to properly weigh individual DNS requests to distribute web requests among its CDN servers. If the *hidden load factors* are known, load balancing algorithms can be easily deployed to achieve better load distribution. On the other hand, if the *hidden load factors* are not known, fine-grained request distribution may be difficult.

### Further reading

* [What is a CDN?][7]

|Sources: [*"DevSecOps Playbook" from Paul McCarty*][10], [*"How content delivery networks (CDNs) work" from Human Who Codes*][3], [*"A Precise and Efficient Evaluation of the Proximity between Web Clients and their Local DNS Servers" from USENIX*][4], [*"CDN Caching" from Imperva*][8] and [*"If-Modified-Since HTTP Header" from keycdn*][9].|
|:--:|

[1]: ./images/cdn-graphical-representation.png
[2]: https://strivecast.com/what-is-a-cdn-for-video-streaming/
[3]: https://humanwhocodes.com/blog/2011/11/29/how-content-delivery-networks-cdns-work/
[4]: https://www.usenix.org/legacy/publications/library/proceedings/usenix02/full_papers/mao/mao_html/node2.html
[5]: http://developer.yahoo.com/performance/rules.html
[7]: https://www.cloudflare.com/learning/cdn/what-is-a-cdn/
[8]: https://www.imperva.com/learn/performance/cdn-caching/
[9]: https://www.keycdn.com/support/if-modified-since-http-header
[10]: https://github.com/6mile/DevSecOps-Playbook
