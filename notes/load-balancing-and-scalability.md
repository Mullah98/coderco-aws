## Load Balancing & Scalability
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

### Load Balancers:
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

### ELB (Elastic Load Balancer)
- A managed load balancer.
- AWS guarantees that it will be working, takes care of upgrades, maintenance, high availability.
- Costs less to set up your *own* load balancer but more effort on your end.
- Less manual configuration compared to self-managed load balancers.
- You define the routing rules; AWS manages the complexity of traffic distribution.


### Health Checks
- Used to verify if an instance is alive and ready to handle traffic.
- LB sends a request (e.g., `GET /health` on a specific port).
- **200 OK → healthy**, LB keeps routing traffic to it. **Non 200 OK**, LB stops sending traffic to that instance.
- Continuous + automatic monitoring.
- Ensures users only hit working instances.
- If one instance fails, traffic is rerouted to healthy ones until recovery.

### Types Of Load Balancers
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

### Load Balancer Security Groups
- Allows incoming traffic from the internet (0.0.0.0/0) over *HTTP* and *HTTPS*.
- Standard cloud pattern fro secure, scalable traffic flow whilst protecting backend infrastructure.
- Acts as a protective front layer.
- **Application SG** - Restricts traffic so only the load balancer can communicate with the backend instance. Ensures instances are not exposed directly to the internet.
- *Think of load balancer like the front door of a building; users go throught he door but can't access rooms directly.

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