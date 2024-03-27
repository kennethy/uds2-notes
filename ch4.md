## Chapter 4. Discovery

Domain name system (DNS) is a distributed, hierarchical, and eventually consitent key-value store that maps domain to the corresponding IP address.


## Resolution Steps
1. The browser checks its local cache to see whether the address has been resolved into an IP.
2. If not, it routes the request to a DNS resolver that is typically hosted by our internet service provider (ISP). The resolver also checks it own cache.
3. With a cache miss, the resolver routes the request to a root resolver. The root resolver maps the top-level-domain (TLD) like `.com` to the address of the name server responsible for it.
4. The request is routed to the TLD name server.
5. The TLD name server maps the address to the adderss of the authoritative name server responsible for the domain.
6. The resolver queries the authoritative name server for the subdomain, like `www` for example.

Every DNS record has a time-to-live (TTL).

Static stability means the server remain functional even if a dependency is impaired.