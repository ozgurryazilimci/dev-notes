# 🔒 Setting Up SSL (HTTPS) in cPanel with AutoSSL (Let's Encrypt)

This guide explains how to install a **free SSL certificate** using
**Let's Encrypt** in cPanel, and how to **force HTTPS** so all visitors
are redirected securely.

---

## 1. Log in to cPanel

- Open your browser and go to: `yourdomain.com/cpanel`

- Enter your username and password.

---

## 2. Enable SSL with AutoSSL (Let's Encrypt)

1. In the cPanel dashboard, scroll to the **Security** section.
2. Open **SSL/TLS Status** (or **Let's Encrypt™ SSL / AutoSSL**, depending on your host).
3. Select your domain(s).
4. Click **Run AutoSSL**.
5. Wait a few minutes, then refresh the page.

✅ If successful, you'll see a green padlock for your domain.

![](<images/ssl_in_cpanel.png>)

---

## 3. Verify SSL Installation

- Visit your site with: `https://yourdomain.com`

- The browser should show a **padlock icon** in the address bar.

---

## 4. Force HTTPS in cPanel (Easy Way)

1. Go to **Domains** in cPanel.
2. Find your domain.
3. Toggle **Force HTTPS Redirect** to ON.

If you see this option, you're done ✅.

![](<images/force_https_in_cpanel.png>)

---

## 5. Force HTTPS Manually with `.htaccess`

If the redirect option is not available, you can enforce HTTPS manually:

- Open **File Manager** in cPanel.

- Navigate to your website's root folder:

    - **Main domain** → `public_html/`
    - **Addon domain** → `public_html/yourwebsite.com/`

- Enable hidden files:

    - Click **Settings** (top right) → check **Show Hidden Files (dotfiles)**.

- Locate or create a file named **`.htaccess`**.

- Add this code:

```apache
 RewriteEngine On
 RewriteCond %{HTTPS} !=on
 RewriteRule ^(.*)$ https://%{HTTP_HOST}%{REQUEST_URI} [L,R=301]
```

- Save and close.

- Refresh your site → it should now always use HTTPS.

---

## 6. (Optional) Redirect to Non-WWW or WWW

To make sure your site always uses one format (with or without `www`),
use one of the following in `.htaccess`:

- **Redirect to www:**

```apache
RewriteEngine On
RewriteCond %{HTTP_HOST} !^www\. [NC]
RewriteRule ^(.*)$ https://www.%{HTTP_HOST}/$1 [L,R=301]
```

- **Redirect to non-www:**

```apache
RewriteEngine On
RewriteCond %{HTTP_HOST} ^www\.(.+)$ [NC]
RewriteRule ^(.*)$ https://%1/$1 [L,R=301]
```

---

## 7. Using Really Simple Security Plugin (WordPress Plugin)

If your website runs on **WordPress**, you can use the **Really Simple Security** plugin to make the process easier.
For more details, see the [Really Simple Security Plugin Guide](wordpress/plugins/really-simple-security-plugin.md).

---

## ✅ Summary

- Use **AutoSSL (Let’s Encrypt)** in cPanel to install a free SSL certificate.
- Verify that your domain is secured with a padlock.
- Force HTTPS either via the **Domains** section, `.htaccess`, or **Really Simple Security** (WordPress).
- Optionally redirect to **www** or **non-www** for consistency.  
