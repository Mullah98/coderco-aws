# AWS Moodule Notes
---

### What is AWS?
- Cloud platform: storage, compute, networking, databases.
- Scalable, cost-effective, IT infrastructure.
- Eg: Netflix, Activision, McDonalds.

### Regions & Availability Zones
- **Region**: Geographic area with multiple AZs.
- **AZ**: Isolated data center; deploy across multiple AZs for fault tolerance.
- Choosing regions: Compliance, proximity, service availability, pricing.
- **Edge locations**: Speed up content delivery (CloudFront).

### Global vs Region-Scoped Services
- **Global**: IAM, Route 53, CloudFront.
- **Region-scoped**: EC2, Elastic Beanstalk, Lambda.

---

## IAM & Security

### Users, Groups & Roles
- **Root account**: Full access; use sparingly.
- **IAM Users**: Individual accounts.
- **IAM Groups**: Assign permissions to multiple users.
- **Roles**: Temporary permissions for services (EC2, Lambda).

### Permissions & Policies
- **Policies**: JSON documents defining actions & resources.
- **Least privilege principle**: Only grant necessary permissions.
- **Inheritance**: Users inherit group policies + inline policies.

### Authentication & Access
- **Console**: Password + MFA.
- **CLI/SDK**: Access keys.
- Always secure credentials and audit regularly.

### Security Tools & Best Practices
- MFA for all accounts.
- Strong password policies.
- Use IAM roles for services, never hard-coded credentials.
- Audit with **Credentials Report** & **Access Advisor**.

---

## EC2 & Compute

### EC2 Basics
- Virtual machines (IaaS): full control of OS & software.
- User Data scripts: Run at launch for bootstrapping.

### Compute Options
- **Containers**: ECS, EKS.
- **Serverless**: Lambda.
- High availability & scalability via ELB, Auto Scaling, multiple AZs.

### Instance Types & Naming
- General Purpose, Compute Optimized, Memory Optimized, Storage Optimized, Accelerated, HPC.
- Naming convention: Class + Generation + Size (e.g., M5.2xlarge).

### Key Takeaways
- Plan regions & AZs for latency, compliance, and cost.
- Follow IAM security best practices.
- Use CLI/SDK for automation.
- Right-size EC2 instances for workload & cost efficiency.
- Use roles for service-to-service permissions.

---