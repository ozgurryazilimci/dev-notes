# DNS & Nameservers

This document explains domains, nameservers, hosts, DNS records, and how DNS resolution works.

```
[Domain Name]
       |
       v
[Nameserver (NS)]
       |
       v
[DNS Records]
       |-- A Record → Points to Hosting Server IP
       |-- CNAME Record → Alias for subdomains
       |-- MX Record → Points to Mail Server
       |-- TXT Record → SPF, DKIM, Verification
       |-- Other Records (AAAA, SRV, etc.)
       |
       v
[DNS Resolver] (Queries the NS to get IPs)
       |
       v
[Hosting Server]      [Mail Server]
       |                    |
       v                    v
[Website Files]       [Email Delivery & Storage]
```

**Explanation**:

**Domain Name**: Your website address (e.g., `example.com`)

**Nameserver (NS)**: Determines which DNS service manages the domain

**DNS Records**: Tell the world how to connect to your domain

- A Record: Maps domain to server IP (for hosting)
- CNAME: Alias or subdomain mapping
- MX: Points to email server
- TXT: SPF, DKIM, or verification records

**DNS Resolver**: The client-side or ISP-side resolver that queries nameservers to translate the domain into an IP
address

**Hosting Server**: Stores your website files

**Mail Server**: Handles sending/receiving emails

💡 **Key point**: The DNS Resolver sits **between the DNS Records and the Hosting/Mail Servers**, because it’s what
actually asks
the nameserver for the IP to connect.

**Example**:

1. You type a domain in your browser (e.g., `example.com`).
2. The **DNS Resolver** (usually your ISP or a public resolver like Google DNS) receives this request.
3. The resolver **queries the Nameserver** for the domain.
4. The **Nameserver responds** with the appropriate DNS records (A, CNAME, MX, etc.).
5. The **resolver returns the IP address** to your browser.
6. Your browser then connects to the **Hosting Server** for the website or the **Mail Server** for email.

---

## Domain

A **domain** is a human-readable address used to access websites on the internet. It maps to an IP address, which
identifies the server hosting the website.

**Example:**

- Domain: `example.com`
- IP Address: `93.184.216.34`

**Parts of a Domain:**

- **Top-Level Domain (TLD):** `.com`, `.org`, `.net`
- **Second-Level Domain (SLD):** `example` in `example.com`
- **Subdomain:** Optional prefix like `blog.example.com`
- **Host:** Specific machine or service within a domain. Often used interchangeably with subdomain in some contexts.
  Example: `www` in `www.example.com`.

---

## Nameserver

A **nameserver** is a server responsible for translating domain names into IP addresses. It is part of the **Domain Name
System (DNS)**.

**Types of Nameservers:**

1. **Authoritative Nameserver:**
    - Stores DNS records for a domain.
    - Provides definitive answers for DNS queries.

2. **Recursive Resolver (DNS Resolver):**
    - Queries multiple nameservers to find the IP address for a domain.
    - Usually provided by ISPs or third-party services like Google DNS (`8.8.8.8`) or Cloudflare (`1.1.1.1`).

---

## DNS Resolver vs Nameserver

![](<images/dns_resolver_vs_namespace.png>)

While both **DNS resolvers** and **nameservers** are involved in translating domain names to IP addresses, they serve
different roles:

| Term                                  | Role                                                                                      | Example / Notes                                                                                                                                              |
|---------------------------------------|-------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **DNS Resolver (Recursive Resolver)** | Queries multiple nameservers on behalf of the client to find the IP address for a domain. | Provided by ISPs or third-party services like Google DNS (`8.8.8.8`) or Cloudflare (`1.1.1.1`). It doesn’t store authoritative records, only caches results. |
| **Nameserver**                        | Stores DNS records and provides authoritative answers for a domain.                       | Authoritative nameservers like `ns1.example.com` contain the actual DNS records (`A`, `MX`, `CNAME`, etc.).                                                  |

**Key Differences:**

1. **Purpose:**
    - Resolver: Finds the IP for a domain.
    - Nameserver: Tells the resolver what the IP is.

2. **Authority:**
    - Resolver: Not authoritative; relies on nameservers for correct answers.
    - Nameserver: Authoritative; stores and serves the definitive records for a domain.

3. **Interaction Flow:**
    - Client → DNS Resolver → Nameserver(s) → Resolver → Client

**Simplified Analogy:**

- Think of a **DNS resolver** as a librarian helping you find a book.
- A **nameserver** is the actual bookshelf with the book on it.

---

## DNS Records

DNS records are stored on authoritative nameservers and tell the internet how to route traffic for a domain.

**Common DNS Record Types:**

| Record Type | Purpose                                                          |
|-------------|------------------------------------------------------------------|
| **A**       | Maps a domain or host to an IPv4 address                         |
| **AAAA**    | Maps a domain or host to an IPv6 address                         |
| **CNAME**   | Maps a domain or subdomain to another domain (alias)             |
| **MX**      | Directs email to mail servers                                    |
| **TXT**     | Stores text information for verification or security (SPF, DKIM) |
| **NS**      | Specifies the authoritative nameservers for a domain             |
| **PTR**     | Maps IP addresses back to domain names (reverse DNS)             |
| **SRV**     | Specifies services for a domain (like VoIP or chat)              |

**Other Concepts:**

- **Host:** Individual device or service within a domain, e.g., `mail.example.com`.
- **Zone File:** A file on an authoritative nameserver containing all DNS records for a domain.

---

## DNS Resolution Flow

When a user types a domain into a browser, DNS resolution occurs in multiple steps:

1. **Browser Cache Check:** Browser looks for cached IP addresses.
2. **Operating System Cache Check:** OS cache is checked if the browser doesn’t have it.
3. **Recursive Resolver Query:** Sent to the DNS resolver (ISP or third-party).
4. **Root Nameserver Query:** Resolver asks root nameserver to find TLD server.
5. **TLD Nameserver Query:** Resolver asks the TLD server (e.g., `.com`) for authoritative nameserver.
6. **Authoritative Nameserver Query:** Authoritative server provides the IP address for the domain or host.
7. **Response Returned:** Resolver returns IP to browser, which connects to the website.

---

## Summary

- **Domain:** Human-readable website address.
- **Subdomain / Host:** Subsection or service under a domain.
- **Nameserver:** Translates domain names to IP addresses.
- **DNS Resolver:** Queries nameservers to find the IP for a domain.
- **DNS Records:** Instructions defining traffic routing.
- **Zone File:** Storage of all DNS records for a domain.
- **Resolution Flow:** Step-by-step process of converting domain names to IP addresses.
