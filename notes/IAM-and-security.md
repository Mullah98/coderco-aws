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
