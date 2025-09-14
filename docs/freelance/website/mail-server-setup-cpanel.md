# Mail Server Setup in cPanel

This document explains how to properly set up email accounts such as `info@yourdomain.com` or `contact@yourdomain.com`
when:

- The **domain** is registered at **Guzel.net**
- **DNS** is managed through **Guzel.net DirectAdmin panel**
- **Hosting and Mail server** are on **FastComet (cPanel)**

---

## Creating Email Accounts in FastComet cPanel

1. Log in to FastComet cPanel.
2. Go to **Email Accounts**.
3. Create a new account (e.g., `info@yourdomain.com`) with a secure password.
4. You can access this inbox from cPanel directly (**Check Email → Open**) or from webmail (
   `https://webmail.yourdomain.com`).

---

## Email Deliverability (SPF, DKIM, DMARC)

- In cPanel, go to **Email Deliverability**.
- Here you will see recommendations for **SPF, DKIM, and DMARC**.
- Copy the suggested DNS records and add them in **Guzel.net DirectAdmin DNS panel**.
- Once correctly added, cPanel will show a green “Valid” status.

---

## Required DNS Records

| Host / Name          | Type  | Value / Example                                          | Purpose                                         |
|----------------------|-------|----------------------------------------------------------|-------------------------------------------------|
| `@` (yourdomain.com) | A     | `203.0.113.10` (FastComet IP)                            | Points the root domain to FastComet server      |
| `www`                | CNAME | `yourdomain.com`                                         | Redirects www → root domain                     |
| `mail`               | A     | `203.0.113.10` (FastComet IP)                            | Mail server hostname                            |
| `webmail`            | A     | `203.0.113.10` (FastComet IP)                            | Webmail access (https://webmail.yourdomain.com) |
| `@`                  | MX    | `mail.yourdomain.com` (priority 10)                      | Mail routing record                             |
| `@`                  | TXT   | `v=spf1 a mx ip4:203.0.113.10 ~all`                      | SPF record for sender validation                |
| `default._domainkey` | TXT   | `v=DKIM1; k=rsa; p=MIGfMA0GCSq...`                       | DKIM record from FastComet cPanel               |
| `_dmarc`             | TXT   | `v=DMARC1; p=none; rua=mailto:postmaster@yourdomain.com` | DMARC policy                                    |

---

## Why A and MX Records Are Both Needed

- **A Record (mail.yourdomain.com → IP):**  
  Tells the internet what IP address your mail server lives on.
- **MX Record (points to mail.yourdomain.com):**  
  Tells other mail servers *where to deliver emails* for your domain.

⚠️ Without an **A record**, the MX record has no IP to resolve to. That’s why both must exist.

---

## Webmail Access

There are two ways to access your mailbox:

1. **Direct cPanel Webmail URL:**  
   `https://yourdomain.com:2096`
   or  
   `https://server123.fcomet.com:2096`

2. **Custom Webmail Subdomain (recommended):**  
   `https://webmail.yourdomain.com`
    - Requires the `webmail` A record pointing to FastComet IP + SSL certificate enabled in cPanel.
    - Run AutoSSL for webmail on SSL/TLS Status page in cPanel.

---

## Mail Clients (IMAP/POP3/SMTP)

When setting up mail in Outlook, Thunderbird, or a mobile device:

- **Incoming Server (IMAP):** `mail.yourdomain.com`
    - Port 993 (IMAPS, SSL)
- **Incoming Server (POP3):** `mail.yourdomain.com`
    - Port 995 (POP3S, SSL)
- **Outgoing Server (SMTP):** `mail.yourdomain.com`
    - Port 465 (SMTPS, SSL) or 587 (STARTTLS)
- **Username:** Full email address (e.g., `info@yourdomain.com`)
- **Password:** The one you set in FastComet cPanel

---

## Why SPF, DKIM, DMARC Are Needed

- **SPF:** Authorizes FastComet servers to send mail for your domain (reduces spoofing).
- **DKIM:** Adds a cryptographic signature so recipients can verify authenticity.
- **DMARC:** Defines policy for handling failed SPF/DKIM checks (e.g., monitor, quarantine, or reject).

Without these, your emails may land in **spam** or be rejected by services like Gmail/Outlook.

---

## Notes

- DNS propagation can take **1–24 hours** after changes.
- Always verify records with [dnschecker.org](https://dnschecker.org).
- SSL must be enabled for `mail.yourdomain.com` and `webmail.yourdomain.com` in cPanel → AutoSSL.
- FTP access is unrelated to mail. It is only used to upload website files into `public_html` on FastComet.

---

## Troubleshooting

- If mail isn’t delivering → check MX and SPF records.
- If webmail doesn’t load → verify the `webmail` A record and SSL certificate.
- If Outlook/phone mail doesn’t connect → make sure you’re using the correct ports and `mail.yourdomain.com` as the
  server.

---

✅ With this setup, your mailboxes like `info@yourdomain.com` or `contact@yourdomain.com` will be fully functional with
FastComet hosting and DNS managed on Guzel.net DirectAdmin.
