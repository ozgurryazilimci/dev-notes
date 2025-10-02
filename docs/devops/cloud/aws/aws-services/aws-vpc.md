# AWS Services - VPC (Virtual Private Cloud)

Amazon VPC (Virtual Private Cloud) is the fundamental networking layer for AWS. It enables developers to launch AWS
resources in a logically isolated virtual network that they define. With VPC, you have full control over your IP ranges,
subnets, route tables, gateways, and security.

![](<images/aws_vpc.gif>)

![](<images/aws_vpc_big.png>)

---

## VPC Overview

- **Isolated Network**: Each VPC is logically isolated from other VPCs in AWS.
- **Customizable IP Space**: You choose a CIDR block (e.g., 10.0.0.0/16).
- **Scalable and Flexible**: You can span multiple Availability Zones (AZs) for high availability.

---

## Subnets

- **Public Subnet**: Connected to the Internet Gateway; resources can receive inbound/outbound internet traffic.
- **Private Subnet**: No direct access to the internet; used for application servers, databases, and backend services.
- **Best Practices**:
    - Use multiple AZs for redundancy.
    - Separate layers (web, application, database) into different subnets.

---

## Route Tables

- Define how network traffic flows inside the VPC.
- Each subnet must be associated with a route table.
- Common routes:
    - **0.0.0.0/0 → Internet Gateway** (for public subnets).
    - **0.0.0.0/0 → NAT Gateway** (for private subnets).
    - **Local** routes (within the VPC CIDR) are automatically added.

---

## Internet Gateway (IGW)

- A gateway that enables communication between resources in your VPC and the internet.
- Must be explicitly attached to the VPC.
- Required for public subnets.

---

## NAT (Network Address Translation)

### NAT Gateway

- Fully managed, highly available AWS service.
- Allows instances in private subnets to access the internet while preventing inbound connections.
- Requires an Elastic IP.

### NAT Instance

- EC2 instance with a NAT AMI acting as a gateway.
- More manual management; less common today since NAT Gateway is preferred.

---

## Elastic IPs

- Static public IPv4 addresses that you can associate with resources.
- Common use cases: NAT Gateways, Bastion Hosts, fixed public endpoints.

---

## Bastion Host (Jump Box)

- EC2 instance in a public subnet used to securely connect to resources in private subnets.
- Best practices:
    - Restrict inbound SSH/RDP with Security Groups and IAM.
    - Use Session Manager (SSM) as an alternative to avoid public exposure.

---

## Security

### Security Groups (SGs)

- Instance-level virtual firewalls.
- **Stateful**: return traffic is automatically allowed.
- Define inbound and outbound rules (e.g., allow SSH on port 22).

### Network ACLs (NACLs)

- Subnet-level firewalls.
- **Stateless**: return traffic must be explicitly allowed.
- Useful for an extra layer of control (e.g., block IP ranges).

---

## VPC Endpoints

- Allow private connectivity between your VPC and AWS services without using the internet.
- Types:
    - **Interface Endpoints**: Elastic network interfaces for services like S3, DynamoDB.
    - **Gateway Endpoints**: For S3 and DynamoDB only, added to route tables.
- Improves security and reduces costs by avoiding public internet.

---

## VPC Peering

- Connects two VPCs so resources can communicate using private IPs.
- Useful for cross-team or cross-account setups.
- Limitations: no transitive peering (each connection is one-to-one).

---

## AWS Direct Connect

- Dedicated private network connection between your on-premises data center and AWS.
- Provides lower latency and higher bandwidth than VPN.
- Often combined with VPN for redundancy.

---

## Monitoring and Logging

- **VPC Flow Logs**: Capture IP traffic information for network interfaces, subnets, or VPCs.
- **CloudWatch Metrics and Alarms**: Monitor bandwidth, packet loss, errors.
- **CloudTrail**: Logs API activity for auditing.

---

## Best Practices

- Design VPCs with multiple AZs for fault tolerance.
- Separate environments (dev, test, prod) into different VPCs.
- Use Security Groups for application-level control, NACLs for subnet-level rules.
- Prefer NAT Gateways over NAT Instances for high availability.
- Use VPC Endpoints for private service access.
- Enable VPC Flow Logs for troubleshooting and auditing.

---

## References

- [Amazon VPC Documentation](https://docs.aws.amazon.com/vpc/latest/userguide/what-is-amazon-vpc.html)
- [IBB Tech Medium](https://hbayraktar.medium.com/)
- [IBB Tech Presentation](https://github.com/hakanbayraktar/ibb-tech/blob/main/sistem-network-atolyesi/30-eylul/30%20Eyl%C3%BCl.pdf)
- [IBB Tech Lab](https://github.com/hakanbayraktar/ibb-tech/blob/main/sistem-network-atolyesi/30-eylul/Custom-VPC-Bastion%20Host-Apache%20Web-Server.pdf)
