# Custom Domain Setup for GitHub Pages

This guide explains how to connect a custom domain (e.g. `example.com` or `blog.example.com`) to a GitHub Pages site.  
No extra hosting is required — GitHub Pages serves your site for free.

---

## Step 1: Buy a Domain

Purchase a domain name from any registrar (e.g., guzel.net.tr, GoDaddy, Cloudflare, Namecheap).  
After purchase, you will be able to manage its **DNS records**.

---

## Step 2: Verify Your Domain in GitHub Profile Settings

Before you can use your domain with GitHub Pages, you must verify that you own it.

1. Go to **GitHub → Profile → Settings → Pages**.
2. Enter your domain (e.g., `example.com`) in the **Custom domain** field.
3. GitHub will show you a verification message like:

    - Create a TXT record in your DNS configuration for the following hostname:  
      `_github-pages-challenge-<username>.<subdomain-prefix>`
    - Use this code for the value of the TXT record: `<github-provided-code>`
    - Wait until your DNS configuration changes. This could take up to 24 hours to propagate.

4. Go to your domain registrar’s DNS panel and add the TXT record. **(Explained in Step 3)**
    - **Type**: TXT
    - **Host/Name**: `_github-pages-challenge-<username>.<subdomain-prefix>`, e.g.
      `_github-pages-challenge-john.blog`
    - **Value**: `<github-provided-code>`, e.g. `11233122`

5. Wait until the DNS change propagates. This can take from a few minutes up to 24 hours.
6. Return to **Profile → Settings → Pages** page. Once verified, your domain will be marked as **Verified**.

---

## Step 3: Domain Registrar DNS Configuration

**There are two types of GitHub Pages sites:**

- **User/Organization site** → from repo named `username.github.io`
    - Default URL: `https://username.github.io/`
    - Can be mapped directly to a root domain (e.g. `example.com`)

- **Project site** → from any other repo (e.g. `blog`)
    - Default URL: `https://username.github.io/repo-name/`
    - Can be mapped to a subdomain (e.g. `blog.example.com`)

### Case A: Root Domain (`example.com`)

Add **A records** pointing to GitHub’s IP addresses in your DNS settings in your registrar:

    A   @   185.199.108.153
    A   @   185.199.109.153
    A   @   185.199.110.153
    A   @   185.199.111.153

### Case B: Subdomain (`blog.example.com`)

Add a **CNAME record** in your DNS settings in your registrar:

    CNAME   blog   username.github.io

### Domain Verification (TXT Record)

GitHub often requires domain ownership verification. Add a TXT record in your DNS settings in your registrar:

    TXT   _github-pages-challenge-<username>.<subdomain-prefix>   github-provided-code

Example:

    TXT   _github-pages-challenge-john.blog   11233122

---

## Step 4: Wait for DNS Propagation

- DNS changes usually propagate within minutes but may take up to **24–48 hours**.
- You can check propagation with:
    - [dnschecker.org](https://dnschecker.org)
    - `nslookup` or `dig` commands from your terminal.

Example with `nslookup`:

```bash
nslookup blog.example.com
```

If DNS has propagated, it should return GitHub’s IPs or `username.github.io`.

---

## Step 5: Configure Repository Pages with Custom Domain

Now that your domain is verified at the profile level, you can assign it to a repository.

1. Open the repository on GitHub.
2. Go to **Repo → Settings → Pages**.
3. In the **Custom domain** field, enter your chosen domain or subdomain:
    - For a full domain: `example.com`
    - For a subdomain: `blog.example.com`
4. Click **Save**. GitHub will create a `CNAME` file in your repository automatically.

---

## Step 6: Enforce HTTPS

- Once the domain is verified, GitHub Pages provides free HTTPS via Let’s Encrypt.
- Go to **Settings → Pages → Enforce HTTPS** and enable it.

---

## Step 7: Test Your Website

- Visit your custom domain (`example.com` or `blog.example.com`).
- If everything is set up correctly, it should display your GitHub Pages site.

---

## ✅ Summary

1. Buy a domain from a registrar.
2. Verify it in **GitHub Profile Settings → Pages** (TXT record).
3. Add DNS records on your registrar:
    - **Root domain** → A records to GitHub IPs
    - **Subdomain** → CNAME to `username.github.io`
    - **TXT record** → GitHub verification code
4. Wait for DNS propagation (up to 24h).
5. Add the custom domain in **Repo → Settings → Pages**.
6. Enable **Enforce HTTPS**.
7. Test your website.

> You can now use GitHub Pages with your custom domain successfully.

---

## 🔧 Troubleshooting

- Use `nslookup example.com` or `nslookup blog.example.com` to confirm DNS.
- Use [dnschecker.org](https://dnschecker.org) to check propagation globally.
- If GitHub still fails to verify after 24h, double-check that:
    - The TXT record matches exactly (no extra spaces).
    - CNAME or A records are pointing to GitHub’s servers.  
