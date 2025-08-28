# DNS Records

This guide explains all common DNS record types, their purposes, usage scenarios, and examples, including IPv4 and IPv6
considerations.

---

## A Record (Address Record)

- **Purpose:** Maps a domain or subdomain to an **IPv4 address**.
- **Use case:** Your server has an IPv4 address and you want your domain to resolve to it.
- **Format:** `123.45.67.89`
- **Example:**

| Host | Type | Value        |
|------|------|--------------|
| @    | A    | 123.45.67.89 |
| www  | A    | 123.45.67.89 |

- **Notes:** Most websites still rely on A records since IPv4 is universally supported.

---

## AAAA Record (IPv6 Address Record)

- **Purpose:** Maps a domain or subdomain to an **IPv6 address**.
- **Use case:** Your server supports IPv6 and you want your site accessible on IPv6 networks.
- **Format:** `2001:0db8:85a3::8a2e:0370:7334`
- **Example:**

| Host | Type | Value          |
|------|------|----------------|
| @    | AAAA | 2001:db8::1234 |

- **Notes:** Modern browsers prefer IPv6 if available. If both A and AAAA exist, IPv6 is tried first. Then fallbacks to
  IPv4.

---

## CNAME Record (Canonical Name)

- **Purpose:** Points a subdomain to **another hostname**, not an IP.
- **Use case:** You want `blog.yourdomain.com` to resolve to `yourdomain.com` or `username.github.io`.
- **Format:** hostname
- **Example:**

| Host  | Type  | Value              |
|-------|-------|--------------------|
| www   | CNAME | yourdomain.com     |
| blog  | CNAME | yourdomain.com     |
| notes | CNAME | username.github.io |

- **Notes:** Cannot be used for the root domain (`@`). Useful for subdomains that should follow another hostname’s IP
  automatically.
- Alternatively you can use IP addresses as a value instead of hostnames. But it is preferable to use hostnames because
  if the IP address changes, you only need to update it in one place.

---

## MX Record (Mail Exchange)

- **Purpose:** Directs email to your mail server.
- **Use case:** You want your domain to receive emails.
- **Format:** priority + mail server hostname
- **Example:**

| Host | Type | Value                  |
|------|------|------------------------|
| @    | MX   | 10 mail.yourdomain.com |

- **Notes:** Lower priority numbers have higher priority. Multiple MX records provide redundancy.

---

## TXT Record (Text Record)

- **Purpose:** Store arbitrary text for verification, SPF, DKIM, or DMARC.
- **Use case:** Email authentication, domain verification.
- **Example:**

| Host                              | Type | Value                                 |
|-----------------------------------|------|---------------------------------------|
| @                                 | TXT  | "v=spf1 include:_spf.google.com ~all" |
| @                                 | TXT  | "google-site-verification=abc123"     |
| _github-pages-challenge-john.blog | TXT  | 11233122                              |

- **Notes:** Important for preventing email spoofing and verifying domain ownership.

---

## NS Record (Name Server Record)

- **Purpose:** Specifies which nameservers are authoritative for the domain.
- **Use case:** Point your domain to a hosting provider’s DNS service.
- **Format:** hostname
- **Example:**

| Host | Type | Value           |
|------|------|-----------------|
| @    | NS   | ns1.hosting.com |
| @    | NS   | ns2.hosting.com |

- **Notes:** Typically set at the registrar. Changing NS changes who controls DNS records for your domain.

---

## Relationship Between IPv4, IPv6, and DNS

- **A record → IPv4**
- **AAAA record → IPv6**
- If a domain has **both A and AAAA**, modern clients try IPv6 first and fall back to IPv4 if IPv6 fails.
- If A and AAAA point to different servers, users may see different content depending on their network.
- **IPv6 adoption** depends on OS, network, and ISP support. Older devices/networks may only support IPv4.

---

## Example Combined DNS Setup

| Host     | Type  | Value                                 | Notes                                      |
|----------|-------|---------------------------------------|--------------------------------------------|
| @        | A     | 123.45.67.89                          | Main website IPv4                          |
| @        | AAAA  | 2001:db8::1234                        | Main website IPv6                          |
| www      | CNAME | @                                     | Follows root domain                        |
| internal | CNAME | yourdomain.com                        | Internal subdomain pointing to main server |
| notes    | CNAME | username.github.io                    | GitHub Pages subdomain                     |
| @        | MX    | 10 mail.yourdomain.com                | Email server                               |
| @        | TXT   | "v=spf1 include:_spf.google.com ~all" | Email SPF                                  |
| @        | NS    | ns1.hosting.com                       | Primary nameserver                         |
| @        | NS    | ns2.hosting.com                       | Secondary nameserver                       |

---

This setup ensures:

- Your website works on IPv4 and IPv6.
- Subdomains correctly point to GitHub Pages or internal servers.
- Email is authenticated and reliable.
- DNS is managed via your chosen authoritative nameservers.

