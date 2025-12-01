## EC2 & Compute

---

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
