# AWS Security Groups vs. Network ACLs (NACLs)

When architecting secure VPCs in AWS, **Security Groups** and **Network ACLs (NACLs)** are two key layers of network control. While they seem similar, their scope, behavior, and defaults differ significantly.

---

## Security Groups (SGs)

- **Scope:** Attached to **ENIs (Elastic Network Interfaces)** on EC2 instances (instance-level).
- **Type:** **Stateful**  
  - If you allow inbound traffic, the response traffic is automatically allowed outbound.
- **Default behavior:**
  - By default, **all inbound traffic is denied**.
  - By default, **all outbound traffic is allowed**.
- **Rules:**  
  - Evaluated as an **allow list only** (no explicit deny rules).
  - Rules are permissive; if no rule matches, traffic is denied.
- **Granularity:**  
  - You can allow traffic based on protocol, port, and source (CIDR or another SG).

---

## Network ACLs (NACLs)

- **Scope:** Attached to **subnets** (subnet-level).
- **Type:** **Stateless**  
  - Responses to allowed inbound traffic are **not automatically allowed** outbound—you must explicitly permit them.
- **Default behavior:**
  - **Default VPC NACL:** allows **all inbound and outbound traffic**.
  - **Custom NACL:** denies **all inbound and outbound traffic** until rules are added.
- **Rules:**  
  - Contain both **allow** and **deny** rules.
  - Rules are numbered and evaluated in order, starting from the lowest number.
  - First matching rule is applied; if no rules match, traffic is denied.
- **Granularity:**  
  - Operates at the subnet boundary.
  - Applies to all resources within the subnet.

---

## Key Differences

| Feature                     | Security Groups                          | Network ACLs                          |
|------------------------------|------------------------------------------|---------------------------------------|
| **Scope**                   | Instance-level (ENI)                     | Subnet-level                          |
| **Type**                    | Stateful (automatic return traffic)      | Stateless (explicit rules required)   |
| **Rule Types**              | Allow only                               | Allow and Deny                        |
| **Default Behavior**        | Deny all inbound, allow all outbound      | Default NACL: allow all; Custom NACL: deny all |
| **Evaluation**              | All rules evaluated (union of allows)    | Rules evaluated in order, first match wins |
| **Traffic Within Subnet**   | N/A (applies to ENIs only)               | Packets **within the same subnet do not cross the NACL** |

---

## Practical Notes

- **Security Groups** are the primary method for controlling instance-level access.  
- **NACLs** are optional for coarse-grained, subnet-level network filtering (e.g., blocking an IP range).  
- Since **same-subnet traffic does not traverse NACLs**, you cannot use NACLs to filter intra-subnet communication—Security Groups are required for that.  

---
