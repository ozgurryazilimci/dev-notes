# WordPress Plugin - Really Simple Security (Really Simple SSL)

When you switch your website to **HTTPS**, you need to ensure all resources load securely. The
[Really Simple SSL](https://wordpress.org/plugins/really-simple-ssl/) plugin automates this process for WordPress sites.

## 🔎 What the Plugin Does

- Detects if an SSL certificate is active on your server.
- Updates WordPress settings (`siteurl` and `homeurl`) to use `https://`.
- Automatically redirects all traffic from **HTTP → HTTPS**.
- Fixes **mixed content issues** (when some resources still load via HTTP).

## ✅ How to Install

1. Log in to your WordPress Admin Dashboard.
2. Go to **Plugins → Add New**.
3. Search for **Really Simple Security**.
4. Click **Install Now** → then **Activate**.

## ⚡ Activate SSL

- After activation, a notice will appear:  
  **“Go ahead, activate SSL!”**
- Click the button → your site will switch to HTTPS automatically.

## ⚠️ Notes

- You **must already have an SSL certificate installed** via cPanel (e.g., Let’s Encrypt).
- Some hosting providers already handle HTTPS redirects. In that case, the plugin mainly helps clean up mixed content
  issues.
- The free version is enough for most users. The Pro version adds extra security features like **HSTS and advanced
  headers**.