# Amazon VPC Overview

An **Amazon Virtual Private Cloud (VPC)** is a **logically isolated network** within AWS where you can launch and manage resources (EC2, RDS, ECS, etc.).  
It gives you control over:

- **IP addressing** (CIDR block, e.g., `10.0.0.0/16`)  
- **Subnets** (public, private, isolated)  
- **Routing** (via route tables)  
- **Access control** (Security Groups, NACLs)  
- **Connectivity** (VPN, Direct Connect, Transit Gateway, VPC Peering, etc.)  

---

## Subnets in a VPC

- A **VPC can contain multiple subnets**, each mapped to a specific Availability Zone (AZ).  
- Subnets can be:  
  - **Public** → Connected to an Internet Gateway (IGW).  
  - **Private** → Outbound internet via NAT Gateway.  
  - **Isolated** → No internet access (commonly for databases).  

**High Availability:** Best practice is to spread subnets across multiple AZs.  

---

## Multiple Applications in the Same Subnet?

✅ Yes — a subnet is just an IP range, so multiple applications/resources can coexist in one subnet as long as they each have unique private IP addresses.  

⚠️ However, **best practice** is to segment workloads by tier or role for security and manageability.  
For example:  
- Web tier → public subnet  
- Application tier → private subnet  
- Database tier → isolated subnet  

---
