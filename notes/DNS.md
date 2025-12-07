# DNS (Route 53)

---

- Translates human-friendly domain names into IP addresses.
- *Hierarchy:* Root -> TLD -> SLD -> Subdomain -> Browser.
- **Terminologies:**
    - Domain Registrar: Where you buy domains (e.g., AWS Route 53, GoDaddy).
    - DNS Records: Instructions for routing traffic (A, AAAA, CNAME, MX, NS).
    - Nameserver: Resolves DNS queries; authoritative source.
    - TLD: Top-level domain (e.g., .com, .org).
    - SLD: Second-level domain, the registered name (e.g., example).
    - FQDN: Fully qualified domain name (e.g., api.www.example.com).
- **DNS Process:** Browser asks local DNS -> Root (Managed by ICANN) -> TLD -> SLD -> Returns IP -> Browser connects.

---


## Amazon Route 53
- Managed DNS service; control + update DNS records for domains.
- *Authorative* - full control over DNS entries.
- Monitors resource health + can reroute traffic automatically.
- Can buy and manage domains like a registar.

## Hosted Zones
- Container for records that define how to route traffic to a domain and its subdomains.
- **Public Hosted Zones:** 
    - Contains records that specify how to route traffic on the internet
    - Manages DNS for domains accessible over the public internet resolving to public IPs, NLBs, S3 buckets etc
- **Private Hosted Zones:**
    - Contains records that specify how you route traffic within 1 or more VPCs
    - Pay $0.50 per month per hosted zone
    - Used within VPCs for internal DNS resolution; Keeps traffic internal + secure

*Route 53 supports both public + private hosted zones, making DNS flexible, scalable, and integrated with AWS architecture.*

---

## Route 53 records
- Directs traffic for a domain or subdomain.
- *Components:* Name, Type, Value, Routing Policy, TTL.

- **Common Record Types:**
    - **A:** Maps hostname → IPv4
    - **AAAA:** Maps hostname → IPv6
    - **CNAME:** Maps hostname → another hostname (subdomains only)
    - **NS:** Nameserver record, defines which servers handle DNS for the domain
    - **Other types:** MX, TXT, SPF, CAA (email, security, advanced routing)

**TTL (Time To Live)**
- How long a DNS record is cached.
- **High TTL** - Fewer queries - lower cost, changes propagate slowly
- **Low TTL** - Changes propagate quickly, higher costs, more traffic
- **ALIAS records** - TTL not required, managed automatically by AWS.

*Purpose: Ensures users are routed correctly to websites, APIs, or other resources.*