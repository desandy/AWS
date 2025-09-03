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

## Example: 3-Tier VPC Architecture

```mermaid
flowchart TB

  IGW["Internet Gateway"]:::gateway

  subgraph VPC["AWS VPC (10.0.0.0/16)"]
    direction TB

    subgraph PublicSubnet["Public Subnet (10.0.1.0/24)"]
      EC2Web["Web Servers (EC2)"]:::public
      NAT["NAT Gateway"]:::gateway
    end

    subgraph PrivateSubnet["Private Subnet (10.0.2.0/24)"]
      EC2App["App Servers (EC2/ECS)"]:::private
    end

    subgraph IsolatedSubnet["Isolated Subnet (10.0.3.0/24)"]
      RDS["Database (RDS)"]:::db
    end
  end

  %% Connections
  IGW --- EC2Web
  EC2Web --- EC2App
  EC2App --- RDS
  EC2App --- NAT
  NAT --- IGW

  classDef gateway fill=#f4d03f,stroke=#7d6608,stroke-width=2px,color=#000
  classDef public fill=#58d68d,stroke=#1d8348,stroke-width=2px,color=#000
  classDef private fill=#5dade2,stroke=#154360,stroke-width=2px,color=#000
  classDef db fill=#bb8fce,stroke=#512e5f,stroke-width=2px,color=#000
