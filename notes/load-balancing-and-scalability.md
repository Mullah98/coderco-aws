# Load Balancing & Scalability
- Scalability means that an app/system can handle greated loads by adapting.
- 2 types = *Vertical Scalability* and *Horizontal Scalability*.

---

### Vertical Scalability:
- Adding more power to the machine such as increasing CPU/RAM on the same machine.
- Commonly used in *non-distributed* systems like databases.
- Hardware limit.

### Horizontal Scalability:
- Adding more machines to distribute load.
- If one instance fails, another continues to handle the traffic.
- *Auto Scaling Groups* help automate horizontal scaling.

### High Availability
- Goes hand in hand with *horizontal scaling*.
- Means running your app/system in at least *2* data centers.
- Goal is to survive a data center loss.
- **Active HA** - Workloads spread across multiple AZs; traffic auto-routed on failure.
- **Passive HA** - Primary instance in one AZ, standby in another; failover if primary fails.

### Combing Scalability & HA
- *Vertical Scaling* first, then *Horizontal Scaling* as traffic grows.
- *Load balancers + Multi AZ + ASG* = Highly available, resilient, and scalable architecture.
- Ensures apps remain accessible and performant during spikes or failures.

---

## Load Balancers:
- Distribute traffic across multiple instances.
- Prevents any single instance from being overloaded.
- Works best with *horizontal scaling*.
- **ELB (Elastic Load Balancer)** in AWS forwards requests to healthy instances automatically.

**Why use a Load Balancer?** 
- Spread load across multiple downstream instances.
- Seamlessly handle failures of downstream instances.
- Do regular health checks to your instances.
- Provide **SSL Termination (HTTPS)** for your websites.
- High availability across zones.

### Reverse Proxy
- Sits between users and servers.
- Can also route traffic based on request content.
- Direct requests to different servers or microservices.

**Benefits** = maintain app availability even if some instances fail, ensure scalable, resilient applications, improve user exp during spikes.

---

## ELB (Elastic Load Balancer)
- A managed load balancer.
- AWS guarantees that it will be working, takes care of upgrades, maintenance, high availability.
- Costs less to set up your *own* load balancer but more effort on your end.
- Less manual configuration compared to self-managed load balancers.
- You define the routing rules; AWS manages the complexity of traffic distribution.

---

## Health Checks
- Used to verify if an instance is alive and ready to handle traffic.
- LB sends a request (e.g., `GET /health` on a specific port).
- **200 OK → healthy**, LB keeps routing traffic to it. **Non 200 OK**, LB stops sending traffic to that instance.
- Continuous + automatic monitoring.
- Ensures users only hit working instances.
- If one instance fails, traffic is rerouted to healthy ones until recovery.

---

## Types Of Load Balancers
- **Classic Load Balancer (CLB):**
    - Old generation (2009).
    - Supports HTTP, HTTPS, TCP, SSL.
    - Simple, fewer features than newer options.
- **Application Load Balancer (ALB):**
    - Layer 7 (Application layer).
    - Handles HTTP, HTTPS, WebSockets.
    - Content-based routing (URLs, headers, cookies).
    - Ideal for microservices and modern web apps.
- **Network Load Balancer (NLB):**
    - Layer 4 (Transport layer).
    - High-performance, low-latency TCP and HTTP traffic.
    - Suitable for real-time gaming, high-frequency trading.
- **Gateway Load Balancer (GWLB):**
    - Layer 3 (Network layer).
    - Manages third-party network appliances (firewalls, IDS, traffic analyzers).
    - Useful for complex networking setups.
- **Internal vs External Load Balancers:**
    - Internal: routes traffic within a VPC.
    - External: handles traffic from the public internet.
- **AWS Managed ELBs (ALB/NLB/GWLB):**
    - AWS handles maintenance, scaling, HA, and integration with services like:
        - EC2 & Auto Scaling Groups
        - ACM for SSL termination
        - CloudWatch for monitoring
        - Route53, WAF, Global Accelerator

- **Best practices:**
    - Use **ALB** for traditional web apps and content-based routing.
    - Use **NLB** for high-throughput, low-latency TCP traffic.
    - Use **GWLB** for deploying scalable network appliances.
    - Avoid using CLB unless legacy support is needed.

---

## Load Balancer Security Groups
- Allows incoming traffic from the internet (0.0.0.0/0) over *HTTP* and *HTTPS*.
- Standard cloud pattern fro secure, scalable traffic flow whilst protecting backend infrastructure.
- Acts as a protective front layer.
- **Application SG** - Restricts traffic so only the load balancer can communicate with the backend instance. Ensures instances are not exposed directly to the internet.
- *Think of load balancer like the front door of a building; users go throught he door but can't access rooms directly.

---

### ALB (Application Load Balancer)
- Operates at Layer 7 (HTTP Traffic).
- Load balancing to multiple HTTP applications across machines + multiple applications on same machine.
- *Smart Routing* - Reads headers, cookies, URLs, query strings.
- Support redirects (eg: HTTP to HTTPS).
- Can handle *multiple applications with one load balancer*, reducing costs and simplifying management.

- **Routing Features:**
    - Routing tables to different target groups.
    - Routing based on path in URL.
    - Routing based on Query String, Headers.

### ALB Target Groups
- **What are target groups?** 
    - Groups of resources (EC2 instances, Lambda functions) that ALB routes traffic to.
    - Act as destinations for incoming requests handled by *Load balancer*.
    - Allows independant scaling of different system parts.

**Types of Target Group Resources**
1. **EC2 Instances**
    - Can be managed by autoscaling groups
    - Automatically add/remove instances based on load
    - ALB balances HTTP traffic between instances
2. **ECS Tasks**
    - Containers running your applications
    - Ideal for microservices and containerized applications
    - ALB routes traffic directly to ECS tasks
3. **Lambda Functions**
    - ALB translates HTTP requests into JSON events
    - Lambda functions process the JSON events
    - Perfect for serverless applications without infrastructure management
4. **Private IP Addresses**
    - Can route traffic directly to IP addresses
    - Must be private IPs (not public)

**Multiple Target Groups**
- Single ALB can route traffic to multiple target groups.
- Enables handling several services/applications with one load balancer.
- Example: One group for user requests, another for search services.

**Health Checks**
- Performed at the target group level.
- ALB routinely checks health of each resource (instances, ECS tasks, Lambda functions).
- If resource is unhealthy, ALB stops routing traffic to it.
- Traffic resumes when resource is back online.
- Ensures users always directed to healthy instances.

---

## NLB (Network Load Balancer)
- Optimized for extreme performance and high traffic with low latency.
- Operated at *Layer 4 (Transport Layer) of OSI model.
- Handles TCP & UDCP traffic.
- Manages millions of request per second.

**Key benefits:**
- Low latency; ~100ms latency (ALB has ~400ms)
- High throughput; Ideal for high-demand apps
- Statis IP addressing; Assigns one static IP per AZ

**Traffic handling:**
- Does NOT inspect HTTP headers or handle SSL termination
- Routes raw TCP or UDP traffic to instances.

---

## Application Load Balancer (ALB) vs Network Load Balancer (NLB)

| Feature | Application Load Balancer (ALB) | Network Load Balancer (NLB) |
|---------|----------------------------------|------------------------------|
| **OSI Layer** | Layer 7 (Application Layer) | Layer 4 (Transport Layer) |
| **Protocol Support** | HTTP, HTTPS | TCP, UDP |
| **Latency** | ~400ms | ~100ms |
| **Performance** | Moderate throughput | Millions of requests per second |
| **Routing Basis** | Path, host, headers, query strings | Port and protocol |
| **HTTP Header Inspection** | Yes | No |
| **SSL/TLS Termination** | Yes | No |
| **Static IP** | No (uses DNS name) | Yes - One static IP per AZ |
| **Elastic IP Support** | No | Yes |
| **Target Types** | EC2 instances, ECS tasks, Lambda functions, private IPs | EC2 instances, ECS tasks, private IPs |
| **Health Checks** | At target group level (HTTP/HTTPS) | At target group level (TCP/UDP) |
| **Best Use Cases** | Web applications, microservices, content-based routing | Gaming servers, financial services, real-time communication, VoIP, DNS |
| **AWS Free Tier** | Included (with limits) | NOT included |
| **Traffic Modification** | Can inspect and modify requests | Forwards traffic without modification |
| **Ideal For** | Applications needing intelligent routing and Layer 7 features | Applications requiring extreme performance and low latency |

- Choose **ALB** when: You need HTTP/HTTPS routing, content-based routing, SSL termination, or Lambda integration
- Choose **NLB** when: You need ultra-low latency, static IPs, millions of requests/second, or TCP/UDP protocols

---

## Sticky Sessions
- Ensure clients consistently connect to the same instance behind a *Load Balancer*.
- Client *sticks* to a specific server for the duration of their session.
- Works across *Classic Load Balancer, Application Load Balancer, and Network Load Balancer*.
- Load Balancer uses *cookies* to track which instance a client is connected to.

---

## SSL (Secure Socket Layer) / TLS (Transport Layer Security) Certificates
- Ensures traffic between clients (browsers) and servers are *encrypted* in transit.
- This is called **in-flight encryption**.
- Data is scrambled as it travels over the internet, preventing snooping.

**TLS**
- The modern, more secure version of *SSL*

**Certificate Authoraties**
- Trusted 3rd parties that verify the identity of websites.
- Issue SSL/TLS certifcates.
- *Must* be renewed before expiration data.
- Duration options include 1 year, 2 years, 3 years, or even 10 years.

## Load Balancer SSL Certifcates
- Load balancer uses an X.509 certificate
- Can manage certificates using **ACM (AWS Certificate Manager)**.
- Can create + upload your own certificates

**Traffic Flow:**
1. User connects via HTTPS (encrypted over the internet)
2. Load balancer handles the encrypted connection
3. Inside the VPC, load balancer forwards request via HTTP (unencrypted)
4. Private traffic within VPC doesn't need encryption
5. Request reaches EC2 instance

---

## SNI (Server Name Indication)
- *Before SNI* - Hosting multiple websites with differnet SSL certs on the same server required a seperate IP address for each one.
- *After SNI* - Multiple SSL certs can be loaded on the same web server without needing multiple IP addresses.

**How SNI Works**
1. Browser/client makes initial connection to web server
2. **Browser sends the hostname** of the website it's trying to reach as part of the SSL handshake
3. Server uses this hostname information to find the right SSL certificate for that specific website
4. If no match is found, server uses the **default certificate**

**Why SNI is Important**
- Perfect for running multiple websites on the same server.
- Essential for ALB and NLB managing multiple domains.
- Each website gets the correct SSL certificate automatically.
- Saves resources by eliminating the need for multiple IP addresses.

**AWS Support for SNI**
- ✅ **Supported:** ALB (Application Load Balancer), NLB (Network Load Balancer), CloudFront
- ❌ **NOT Supported:** CLB (Classic Load Balancer)

---

Here’s a clean, concise summary you can paste directly into GitHub or Notion:

---

## Elastic Load Balancers & SSL (AWS)

**Classic Load Balancer (CLB)**
- Only supports **one SSL certificate**.
- Serving multiple domains requires **multiple CLBs** → more cost & complexity.

**Application Load Balancer (ALB / ELB)**
- Supports **multiple SSL certificates** on multiple listeners.
- Uses **SNI (Server Name Indication)** to serve different domains from one load balancer.
- More flexible and cost-efficient.

**Network Load Balancer (NLB)**
- Also supports **multiple SSL certificates** with SNI.
- Ideal for **high-performance TCP/UDP** workloads that still need SSL.

**Key point:**
Modern ELBs (ALB & NLB) offer far better SSL flexibility thanks to **SNI**.
If you're still on CLB, upgrading is recommended.

---

## Connection Draining / Deregistration Delay (AWS ELB)
- A feature that lets existing requests finish before an instance is removed from a load balancer.
- Ensures smooth scaling and avoids user interruptions during instance removal.

**How it works:**
1. The load balancer stops sending new requests to the instance.
2. In-flight requests are allowed to complete before the instance is fully removed.

---

## ASG (Auto Scaling Groups)
- Manages instances automatically based on traffic/load changes.
- Useful for websites and apps with variable traffic.
- Leverages cloud's ability to scale resources up or down quickly.

**Scaling Operations:**
- **Scale Out** - Add more instances to handle increased demand. Occurs when traffic increases, upto max capacity limit.
- **Scale In** - Reduces number of running instances to save costs. Occurs when traffic decreases, never goes below min capacity.

**3 Key Capacity Settings:**
- **Minimum Capacity** - Least num of instances always running, maintained even during low traffic times.
- **Desired Capacity** - Target number of instances for normal load. ASG tries to maintain this number unless conditions change.
- **Maximum Capacity** - Most instances ASG can scale up to during high traffic

**Automatic Load Balancer Integration**
- New instances automatically regiester with your load balancer.
- If instances become unhealthy or terminate, ASG automatically recreates it.
- ASG are *free*. You only pay for the EC2 instances they manage.

---

## CloutWatch Alarms + ASG
- Moniter metrics like CPU, memory, or custom metrics.
- Alarm triggers an action.

## ASG Dynamic Scaling Policies

1. **Target Tracking**
    - Easiest setup.
    - You define a target metric (e.g., keep CPU at 40%).
    - ASG adds/removes instances to maintain that target.
2. **Simple / Step Scaling**
    - Triggered by CloudWatch alarms.
    - Example: CPU > 70% → add 2 instances; CPU < 30% → remove 1 instance.
    - Offers fine-grained control.
3. **Scheduled Scaling**
    - Pre-planned scaling for predictable traffic patterns.
    - Example: boost capacity every Monday at 10 a.m.
    - Perfect for known spikes (sales events, weekly peaks).

---