# AWS Networking

---

## CIDR (Classless Inter-Domain Routing)
- A method for allocating IP addresses and defining IP ranges.
**Components**
    - *Base IP* - Start of the range (eh: 10.0.0.0, 192.168.0.0)
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