# AWS Services - Route 53

Amazon Route 53 is a highly available and scalable cloud Domain Name System (DNS) web service. It connects user requests
to internet applications running on AWS or external infrastructure and can be used for both domain registration and
intelligent traffic routing.

![](<images/aws_route53.png>)

---

## DNS Fundamentals

- **DNS (Domain Name System)**: Translates human-readable domain names (e.g., `example.com`) into machine-readable IP
  addresses (e.g., `192.0.2.1`).
- **Root Zone**: The top of the DNS hierarchy (`.`). Managed by IANA.
- **TLD (Top-Level Domain)**: The suffix of a domain name, such as `.com`, `.org`, `.net`.
- **gTLD (Generic Top-Level Domain)**: Non-country specific, e.g., `.com`, `.info`.
- **ccTLD (Country Code Top-Level Domain)**: Country-specific, e.g., `.us`, `.uk`, `.de`.
- **Authoritative Name Servers**: Provide the definitive DNS records for a domain.

### How DNS Works?

The Domain Name System (DNS) is a hierarchical and distributed naming system. When a user types `www.example.com` into a
browser, DNS resolution happens in a series of steps:

1. **Root DNS Servers (`.`)**
    - The process begins at the root, which directs the query to the correct **Top-Level Domain (TLD)** servers.
    - Example: For `www.example.com`, the root points to `.com` servers.

2. **TLD Servers (`.com`, `.org`, etc.)**
    - The TLD servers know which authoritative name servers are responsible for the requested domain.
    - Example: `.com` servers return the name servers for `example.com`.

3. **Authoritative Name Servers**
    - These servers hold the actual DNS records for the domain.
    - Example: The authoritative NS for `example.com` returns the A record for `www.example.com`.

4. **DNS Records**
    - Finally, the specific record (A, CNAME, etc.) is resolved to an IP address, allowing the browser to connect to the
      server.

**Hierarchy (fihrist model):**

- Root (`.`)
    - TLD (`.com`)
        - Domain (`example.com`)
            - Host/Subdomain (`www.example.com`)

This hierarchical model ensures scalability, redundancy, and distributed management of the DNS system.

---

## Route 53 Components

### Domain Registration

- Route 53 can register domain names directly or manage domains registered elsewhere by updating the name servers.
- Integrates with AWS Certificate Manager (ACM) for SSL/TLS certificates.

### Hosted Zones

- **Public Hosted Zone**: DNS records for a domain that is accessible on the public internet.
- **Private Hosted Zone**: DNS records that are only resolvable within an Amazon VPC. Useful for internal services (
  e.g., `db.internal.local`).

---

## DNS Records in Route 53

- **A (Address Record)**: Maps a domain to an IPv4 address.
- **AAAA (IPv6 Record)**: Maps a domain to an IPv6 address.
- **CNAME (Canonical Name)**: Alias for another domain (e.g., `www.example.com → example.com`).
- **MX (Mail Exchange)**: Defines mail servers for a domain.
- **TXT**: Stores text information, often used for verification (e.g., SPF, DKIM).
- **NS (Name Server)**: Specifies the authoritative name servers for the domain.
- **SRV (Service Locator)**: Defines location of services.
- **Alias Records** (Route 53-specific): Point a domain to AWS resources (S3, CloudFront, ELB) without needing an IP
  address. Alias records are free of charge.

---

## Routing Policies

Route 53 offers multiple routing policies for flexible traffic management:

- **Simple Routing**: Single resource, returns one record.
- **Weighted Routing**: Split traffic across multiple resources based on assigned weights (useful for A/B testing).
- **Latency-Based Routing**: Directs users to the AWS region with the lowest latency.
- **Failover Routing**: Primary/secondary setup for high availability. If health check fails, traffic is routed to the
  secondary.
- **Geolocation Routing**: Route traffic based on the user’s geographic location (continent, country, or state).
- **Geoproximity Routing** (with Traffic Flow): Route traffic based on location of resources and bias rules.
- **Multivalue Answer Routing**: Return multiple healthy records for simple load balancing.

---

## Health Checks & Monitoring

- **Health Checks**: Actively monitor endpoints (HTTP, HTTPS, or TCP) to determine availability.
    - Can be associated with DNS records for failover.
    - Supports CloudWatch alarms integration.
- **Failover with Health Checks**: If a resource becomes unhealthy, Route 53 redirects traffic to a healthy resource.

---

## Traffic Policies and Traffic Flow

- **Traffic Policy**: JSON-based configuration that defines complex routing rules (weighted, latency, failover, geo).
- **Traffic Flow**: Visual editor in the AWS Console to create advanced traffic routing rules without manually editing
  JSON.
- Policies can be versioned and reused across multiple hosted zones.

---

## Security & Access Control

- **IAM Policies**: Control who can manage Route 53 domains, hosted zones, and records.
- **DNSSEC (Domain Name System Security Extensions)**: Adds cryptographic signatures to DNS records for integrity and
  authenticity (available for domain registration).
- **Private DNS**: Control internal name resolution securely within VPCs.

---

## Integration with Other AWS Services

- **Elastic Load Balancing**: Alias records can point directly to load balancers.
- **CloudFront**: Use Route 53 for custom domain mappings with distributions.
- **S3 Static Websites**: Route 53 Alias can point to S3 website endpoints.
- **API Gateway / AppSync**: Use CNAME or Alias for custom API domains.
- **Global Accelerator**: Integrate with Route 53 for performance-optimized routing.

---

## Best Practices

- Always use **Alias Records** for AWS resources (no extra cost, automatic IP updates).
- Use **health checks** with failover routing for high availability.
- Organize infrastructure with **private hosted zones** for internal apps.
- Combine **latency-based routing** with **geolocation** for global applications.
- Apply **IAM least privilege** for DNS administration.
- Regularly monitor **CloudWatch metrics** for query volume and health check statuses.