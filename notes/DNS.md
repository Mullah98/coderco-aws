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

---

## CNAME vs Alias Records
- **CNAME:**
    - Points to a hostname to another hostname
    - *Cannot be used for root domains*
    - Suitable for *subdomains*
- **Alias:**
    - AWS-specific record
    - Points hostname to AWS resource (ALB, NLB, Cloudfront, API gateway, S3 etc)
    - *Can be used for root domains*
    - Automatically handles changes to the IP of AWS resources
    - Does *NOT* require *TTL* - AWS manages it
- Record types are always **A** (IPv4), or **AAAA** (IPv6).

**Best Use:** Simplifies DNS management for AWS resources while providing flexibility and cost-effectiveness.

---

## Route 53 Routing Policies
- Routing policies don't route traffic themselves.
- Help DNS respond with the correct IP.
- Provide flexibility, control, and improved performance across different use cases.

| Routing Policy | Purpose / Description | Key Features | Use Cases |
| --- | --- | --- | --- |
| **Simple** | Return same IP for every query | Single/multiple values randomly, no health checks | Basic setups, no failure handling needed |
| **Weighted** | Distribute traffic based on weights | Assign proportional weights, supports health checks | Load balancing, gradual deployment, traffic control |
| **Failover** | Route traffic to backup if primary fails | Requires health checks, automatic rerouting | High availability, disaster recovery |
| **Latency-Based** | Send users to server with lowest latency | Optimizes global performance, can use health checks | Global apps, performance optimization |
| **Geolocation** | Route based on user location | Continent/country/state, default fallback | Localization, content distribution, regulatory compliance |
| **Geoproximity** | Adjust traffic distribution based on proximity | Uses bias values to expand/reduce traffic, requires Traffic Flow | Prioritize regions, fine-tune global traffic |
| **Multi-Value Answer** | Return multiple healthy resources per query | Up to 8 healthy records, tied to health checks | Redundancy, simple load distribution (not a replacement for ALB) |

---

## Domain Registrar vs DNS Service
- **Domain Registrar**
    - Service where you buy and register a domain
    - Requires an annual fee to maintain ownership
    - Manages legal ownership of your domain
    - *Sells/owns your domain*
- **DNS Serice**
    - Manages DNS records that map your domain to IP address and servers
    - Can be provided by your registrar or a seperate service
    - Offers flexibility, performance, and advanced features
    - *Manages traffic routing*

- *You can mix and match; buy a domain from one (GoDaddy), manage DNS with another service (Route 53).*

- **Using a 3rd party registrar with Amazon Route 53:**
    1. Created a hosted zone in Route 53 (typically public)
    2. Set up DNS records for your domain within Route 53
    3. Update name servers at your registrar to point to the Route 53 name servers

---