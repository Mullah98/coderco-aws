## Security Groups (SGs)

### Purpose:
- Acts as a *virtual firewall* for EC2 Instances.
- Controls inbount and outbound traffic.
- Only *allow rules*, anything nott explicitly allowed is blocked by default.
- Essential for tight security, controlling who can connect, which ports are accessible, and preventing unwanted traffic.

### Inbound traffic:
- Only traffic matching SG rules is permitted.
- Unauthorized IPs or ports are automatically blocked.
- **EG:** Allow HTTP (Port 80) or SSH (Port 22) from trusted IPs.

### Outbound traffic:
- By default, **all outbound traffic is allowed**.
- Can be restricted by adding specific outbound rules if necessary.

### Best practice:
- Maintain *seperate SGs* for SSH and other services.
- Always limit sensitive access to trusted IPs.

### Referencing other SGs:
- Instead of allowing traffic from IP addresses, you can allow traffic from *other SGs*.
- **This means:**
    - Any instance in SG-A can talk to instances in SG-B on specified ports.
- Useful for:
    - **Auto-scaling**, where instance IPs change.

---

### Important Networks

| Service        | Port | Purpose / Notes                                                   |
| -------------- | ---- | ----------------------------------------------------------------- |
| **SSH**        | 22   | Secure server login; also used for SFTP; restrict to trusted IPs. |
| **FTP**        | 21   | Old file transfer; not secure.                                    |
| **SFTP**       | 22   | Secure file transfer over SSH; preferred over FTP.                |
| **HTTP**       | 80   | Unencrypted web traffic.                                          |
| **HTTPS**      | 443  | Encrypted web traffic (TLS); standard for secure sites.           |
| **DNS**        | 53   | Domain → IP lookups; essential for internet services.             |
| **RDP**        | 3389 | Remote access to Windows servers.                                 |
| **SMTP**       | 25   | Email sending.                                                    |
| **MySQL**      | 3306 | MySQL database access.                                            |
| **PostgreSQL** | 5432 | Postgres database access.                                         |

### Elastic IPs
- Assign a fixed public IPv4 to an EC2 instance to maintain a stable IP even after stopping/refreshing.
- **Free** when attached to a *running instance*; charged when allocated but not unused.
- Can be **remapped** between instances as needed.
- Only **5** Elastic IPs per WS account; can request more.
