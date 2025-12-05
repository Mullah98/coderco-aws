# AWS Networking

---

## CIDR (Classless Inter-Domain Routing)
- A method for allocating IP addresses and defining IP ranges.
**Components**
    - *Base IP* - Start of the range (eg: 10.0.0.0, 192.168.0.0)
    - *Subnet Mask* - Number after the slash determines how many bits can change
**IP Range examples**
- `/32` -> single IP
- `/26` -> 64 IPs
- `/0` -> all IPs
**Key rule:** Smaller numbers after the slash; larger IP range. Larger numbers; smaller range.
**Use cases:** Defining ranges for VPCs, security groups, networking configs in cloud platforms like AWS.

---

## How to Calculate Number of IPs From CIDR (Simple Method)

**Formula:**
`Number of IPs = 2^(32 - CIDR)`
*Because IPv4 has 32 bits total.*

**Examples**
/32 → 2^(32-32) = 2^0 = **1 IP**
/31 → 2^(32-31) = 2^1 = **2 IPs**
/30 → 2^(32-30) = 2^2 = **4 IPs**
/29 → 2^(32-29) = 2^3 = **8 IPs**
/24 → 2^(32-24) = 2^8 = **256 IPs**
/16 → 2^(32-16) = 2^16 = **65,536 IPs**

*Every time the CIDR decreases by 1, the IP count doubles.*

---

## How to Know How Many Octets Can Change

*Just look at the CIDR:*

`/0` → all **4 octets** change
`/8` → last **3 octets** change
`/16` → last **2 octets** change
`/24` → last **1 octet** changes
`/32` → **none** change

Rule of thumb:
**Every 8 bits = 1 octet**

---

## VPC (Virtual Private Cloud)
- Automatically created with every new AWS account.
- Allows immediate use of AWS services without manual VPC setup.
- If no VPC/subnet is specified, EC2 instance go into the default VPC.
- Instances receive *automatic public Ipv4* addresses, enabling internet access.
- Default VPC comes with internet connectivity by default.

## VPC Subnets
- *Subnets:* Small networks within a VPC.
- Divide a VPC into multiple subnets for segmentation, high availability, and redundancy.
- Can be public or private, depending on internet access. 
- **Reserved IPs (AWS):**
    - AWS reserves 5 IPs per subnet, not usable by resources.
    - Reserved IPs in a subnet (e.g., 10.0.0.0/24):
        - `.0` → Network address
        - `.1` → VPC router
        - `.2` → Amazon-provided DNS
        - `.3` → AWS future use
        - `.255` → Broadcast address (reserved, not usable)

**Design Tip:**
- When choosing subnet CIDR, account for 5 reserved IPs.
- Example: To host 29 instances, /27 gives 32 IPs minus 5 → 27 usable (not enough). Use /26 → 64 IPs minus 5 → 59 usable (sufficient).

**Key Idea:** Always calculate usable IPs after AWS reservation when planning subnets.

---

## Internet Gateway (IGW)
- Allow *VPC* resources to connect to the internet.
**Features**
    - *Horizontally scalable* - handles increasing traffic easily
    - *Highly available and redundant* - no single point of failure
- Not created by default with a VPC.
- *Must create and attach manually.*
- Only *1 IGW per VPC* & *1 VPC per IGW*.
- Attaching an IGW alone *does not grant access to the internet*.
- Must update route tables to direct traffic to/from the IGW.

---

## Bastion Hosts
- Used to securely access private EC2 instances in private subnets.
- Private instances *cannot be reached directly* from the internet.
- A bastion host sits in a public subnet and can be accessed from the internet (via SSH).
- You *SSH into the bastion*, then *SSH into private instances*.

**Security group rules:** 
- *Bastion*: allow SSH only from trusted IP ranges (e.g., office IP).
- *Private instances:* allow SSH from the bastion (private IP or SG).

**Purpose:** manage private resources safely without exposing them to the public internet.

---

## NAT Gateway
- Gives outbound internet to private subnets only.
- No inbound allowed.
- Fully managed, auto-scales, no SGs, no patching.
- AZ-specific, uses an Elastic IP, needs an IGW.
- Pay per hour + data.
- Must be in a different subnet from the private instances.
- Flow: Private Subnet → NAT GW → IGW → Internet.

## High Availability
- One NAT gateway = one AZ.
- If that AZ fails, all dependent private subnets lose outbound access.
- **Fix:** deploy a NAT gateway in every AZ and adjust route tables.

### **NAT Gateway vs NAT Instance**

| Feature | NAT Gateway | NAT Instance |
| --- | --- | --- |
| **Availability** | HA within one AZ; create one per AZ for redundancy | No built-in HA; needs failover scripts |
| **Bandwidth** | Auto-scales up to ~100 Gbps | Limited by EC2 instance size |
| **Maintenance** | Fully managed by AWS | You manage OS, patches, updates |
| **Cost Model** | Hourly cost + data processed | EC2 instance cost + network charges |
| **Security Groups** | No SGs | Supports SGs |
| **Elastic IP** | Requires EIP | Can use EIP |
| **Acts as Bastion Host** | ❌ No | ✔️ Yes |
| **Scalability** | Automatic | Manual (change instance type) |
| **Use Case** | High availability, high bandwidth, minimal admin | More control, lower cost for small setups |

---

## NACL (Network Access Control List)
- Controls traffic at *subnet* level.
- *Stateless* - Inbount and outbound rules must be defined seperately.
- *1 NACL per subnet*
- *Lower numbered rules* -> *Higher priority*; First match wins.
- Denies unmatched traffic by default.
- Useful for blocking IPs or adding extra security before traffic reaches instances.

## SGs & NACLs
- **SGs:** 
    - Operates at instance level
    - Is *stateful*, if inbount traffic is allowed, then return outbound traffic is automatically allowed
    - Control traffic to/from EC2 Instances
- **NACL:**
    - Operated at subnet level.
    - Is *stateless*, inbount and outbound rules must be defined
    - Control traffic entering or leaving the entire subnet

- **Key Differences**
| Feature | Security Group | NACL |
| --- | --- | --- |
| Level | Instance | Subnet |
| Stateful? | Yes | No |
| Inbound/Outbound | Outbound auto allowed for responses | Must define both explicitly |
| Scope | Specific instances | Entire subnet |
| Use | Firewall for instances | Subnet-level traffic filtering |

---

## VPC Peering
- Privately connect 2 VPCs without using the internet.
- Requires *Non-overlapping CIDRs.
- Each VPS needs its own connection.
- Must update route tables in *both* VPC subnets to allow communication.

**Advanced features & use cases:**
- *Cross-account pairing* - VPCs in different AWS accounts can communicate privately.
- *Cross-region pairing* - VPCs in different regions can be peered.
- *Reference SGs accross accounts* - SGs in 1 VPC can be reference and peered by a VPC in the *same* region.
- Useful for large orgs with multiple AWS accounts.
- Secure dev access to prod resources without exposing the network.
- Flexible, secure traffic management across isolated environments.
- Private, secure, scalable connectivity without using the public internet.