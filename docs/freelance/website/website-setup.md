# Website Setup

This document provides a step-by-step guide to setting up a website from scratch, including domain registration,
hosting, WordPress installation, plugins, pages, and email configuration.

---

## 1. Domain Registration

1. **Choose a domain registrar**
    - [Güzel Hosting](https://guzel.net.tr)
    - [Natro](https://natro.com)
    - [isimtescil](https://isimtescil.net)
    - [GoDaddy](https://godaddy.com)
    - [Namecheap](https://namecheap.com)
    - [Google Domains](https://domains.google)
    - [Odeaweb](https://odeaweb.com)
    - [Metunic](https://metunic.com.tr) (for .tr domains)
    - [OVH](https://www.ovhcloud.com) (Europe-based sites)
2. **Search for available domains** using the registrar's search tool.
3. **Purchase your desired domain**.
4. **Configure DNS settings**:
    - Access the domain's DNS management panel.
    - Update the **Nameservers** to point to your hosting provider (your host will provide these, e.g., `ns1.host.com`,
      `ns2.host.com`).
    - Optionally, configure **A Records** or **CNAME** if using custom setups. For more details about DNS records,
      please follow [DNS Records](../../network/dns-records.md) page.

> Note: For .tr domains, [trabis.gov.tr](https://www.trabis.gov.tr/page/2) is the official registry site.

---

## 2. Hosting Setup

1. **Choose a hosting provider**:
    - [FastComet](https://fastcomet.com)
    - [Güzel Hosting](https://guzel.net.tr)
    - [Natro](https://natro.com)
    - [GoDaddy](https://godaddy.com)
    - [Odeaweb](https://odeaweb.com)
    - [Bluehost](https://bluehost.com)
    - [SiteGround](https://world.siteground.com)
    - [HostGator](https://hostgator.com)
    - [Hostinger](https://hostinger.web.tr)
2. **Select a hosting plan**:
    - Shared hosting (for small websites)
    - VPS hosting (for medium to large websites)
    - Dedicated hosting (for high traffic)
3. **Purchase hosting** and link it with your domain:
    - Enter your domain name during setup.
    - Update your domain's nameservers to match your hosting provider (if not done already).

### Reseller / Multiple Sites Hosting

- Use **WHM (Web Host Manager)** for multiple websites.
- Access WHM via your host-provided URL, e.g., `xxxx.fcomet.com:9999`.
- Configure each website individually through cPanel accounts.

---

## 3. Access cPanel

Most hosts provide cPanel for managing your website:

- Log in to cPanel using credentials from your host.
- Key sections in cPanel:
    - **File Manager**: Upload files
    - **Databases (MySQL/MariaDB)**: Create databases for WordPress
    - **Email Accounts**: Setup email addresses
    - **Domain Manager**: Add/remove domains or subdomains
    - **Softaculous/App Installer**: One-click WordPress installation

---

## 4. WordPress Installation

### Automatic Installation (via cPanel)

- Navigate to **Softaculous / WordPress Installer**.
- Select your domain.
- Fill in:
    - **Site Name**
    - **Admin Username**
    - **Admin Password**
    - **Email**
- Click **Install**.
- Access your WordPress dashboard: `yourdomain.com/wp-admin`.

### Manual Installation

1. Download WordPress from [wordpress.org](https://wordpress.org/download/).
2. Upload WordPress files to your host via **File Manager** or **FTP**.
3. Create a **database** in cPanel:
    - Go to **MySQL Databases**
    - Create a database, user, and assign user to database
4. Rename `wp-config-sample.php` to `wp-config.php` and add database details:
    - Database name
    - Username
    - Password
    - Host (`localhost`)
5. Access your domain in a browser and follow the WordPress setup wizard.

---

## 5. WordPress Configuration

- Log in to the WordPress admin dashboard.
- Set **General Settings**: Site Title, Tagline, Timezone.
- Set **Permalinks** (Settings > Permalinks) to `Post name` for SEO-friendly URLs.

### Plugins

- Navigate to **Plugins > Add New**.
- Install and activate the necessary plugins.

**Essential plugins:**

- Security (e.g., Wordfence)
- SEO (e.g., Yoast SEO)
- Cache (e.g., W3 Total Cache)
- Backup (e.g., UpdraftPlus)
- WooCommerce (e.g., for e-commerce sites)
- Classic Editor & Widgets
- Jetpack (site stats)
- WP Reset (reset/testing)
- Yoast SEO / Rank Math SEO (basic SEO)
- BuddyPress (for user management)
- Site Kit by Google
- iThemes Security
- ShortPixel / Imagify (image optimization)

### Themes

- Go to **Appearance > Themes > Add New**.
- Choose a free or premium theme.
- Customize via **Appearance > Customize**.

**Essential themes:**

- **Sources**:
    - [ThemeForest](https://themeforest.net/) / Envato Market: premium, update-enabled themes
    - [Envato Elements](https://elements.envato.com/): personal use, no updates
- **Builders**:
    - [Elementor](https://elementor.com/), WPBakery for page editing
- **Notes**:
    - Prefer plugins over editing theme code to preserve changes during updates
    - Free themes: Storefront (simple setup)

---

## 6. Adding Pages and Content

1. **Pages**: Go to **Pages > Add New** for Home, About, Contact, etc.
2. **Posts**: Go to **Posts > Add New** for blog entries.
3. **Menus**: Go to **Appearance > Menus** to organize navigation.
4. **Widgets**: Go to **Appearance > Widgets** to add sidebars, footers, and other elements.

---

## 7. Email Setup

1. In cPanel, go to **Email Accounts**.
2. Create an email (e.g., `info@yourdomain.com`).
3. Configure **Email Client** (Outlook, Gmail, etc.) or use **Webmail ((`webmail.yourdomain.com`))**.
4. For external email services (Google Workspace, Microsoft 365):
    - Update **MX Records** in your domain DNS settings.

> Note: For temporary/temp/fake emails, use services like `tempail.com`.

---

## 8. Website Launch

1. Test all pages, links, and forms.
2. Enable **SSL Certificate** (usually via `cPanel > SSL/TLS` or via `Let’s Encrypt`). Ensure HTTPS is enforced.
3. Verify site speed and mobile responsiveness.
4. Backup your site before launch.
5. Make your website public by publishing.

---

## 9. Maintenance

- Regularly update WordPress core, themes, and plugins.
- Monitor site performance and security.
- Regularly backup your website and database.

---

## 10. Security

- Enable **iThemes Security** plugin
- Use [Google PageSpeed](https://pagespeed.web.dev/) Insights for performance check
- GDPR compliance: KVKK regulations
- Password resets via PHP/MySQL using MD5 if necessary

---

## 11. Analytics & SEO

- Track visitor data:
    - Google Site Kit
    - Yandex Metrica (`metrica.yandex.com.tr`)
- Optimize images and pages for speed
- Keep SEO plugins updated and configure titles, meta, sitemap
