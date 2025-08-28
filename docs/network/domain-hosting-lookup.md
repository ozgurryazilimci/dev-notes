# Domain & Hosting Lookup

This guide lists useful free tools to check domain ownership, hosting provider, and DNS records.

---

## 🔎 WHOIS Lookup (Who Owns a Domain?)

You can see who registered a domain, their contact details, registrar, and registration dates.

- [who.is](https://who.is)
- [whois.com](https://www.whois.com/whois)
- [whois.domaintools.com](https://whois.domaintools.com)
- [lookup.icann.org](https://lookup.icann.org)
- [namecheap.com/domains/whois](https://www.namecheap.com/domains/whois)
- [trabis.gov.tr](https://trabis.gov.tr) (for `.tr` domains)

Example domain: `example.com`

> Note: If the domain owner has enabled WHOIS Privacy/ID Protection, personal details will be hidden.

---

## 🏢 Hosting Lookup (Where Is a Site Hosted?)

You can check which hosting provider a website is using.

- [hostingchecker.com](https://hostingchecker.com)
- [whoishostingthis.com](https://www.whoishostingthis.com)

---

## 🌐 DNS Record Lookup

You can check DNS propagation and verify TXT, CNAME, MX, and other records.

- [dnschecker.org](https://dnschecker.org)

---

## 💻 Local DNS Testing with `nslookup`

On your computer, you can test DNS resolution directly:

```bash
nslookup example.com
nslookup sub.example.com
```

- If the record is set up correctly, it will return the target (IP for **A record**, hostname for **CNAME**,
  value for **TXT**, etc.).
- If you get `NXDOMAIN` or `Non-existent domain`, it means DNS is not yet set or propagated.

---

## ⏳ DNS Propagation Time

- DNS changes usually take **10–60 minutes** to propagate.
- In some cases, it can take up to **24–48 hours** globally.
- Caching by ISPs may cause different results in different regions during propagation.

---

## ✅ Usage Tips

1. Use **WHOIS lookup** to check who owns a domain.
2. Use **hosting lookup** tools to see which company hosts a site.
3. Use **DNS checker** to confirm if your DNS changes (TXT, CNAME, etc.) have propagated.
4. Use **nslookup** locally to troubleshoot if records are visible to your device.
