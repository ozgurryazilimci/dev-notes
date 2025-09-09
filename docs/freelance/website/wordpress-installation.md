# WordPress Installation

This document provides a detailed guide for installing WordPress on your server. It covers **cPanel automatic
installation**, **manual installation**, database configuration, HTTPS setup, file structure, and post-installation
administration.

---

## Installing WordPress via cPanel

cPanel provides an easy way to install WordPress using Softaculous or a similar installer.

**Steps:**

1. **Login to cPanel**  
   Navigate to `https://yourdomain.com:2083` or `https://yourdomain.com/cpanel` and log in with your cPanel credentials.

2. **Locate WordPress Management or Installer**  
   In the **Software** section, click on **WordPress** (usually powered by Softaculous).

3. **Install WordPress**
    - Click **Install Now**.
    - **Choose Protocol**:
        - `https://` if your domain already has SSL configured.
        - `http://` otherwise (SSL can be configured later).
    - **Choose Domain**: Select the domain or subdomain where WordPress will be installed.
    - **Directory**: Leave blank to install at the root (`yourdomain.com`) or specify a folder like `blog` (
      `yourdomain.com/blog`).

4. **Site Settings**
    - **Site Name** and **Site Description**: Enter your preferred values.
    - **Admin Username & Password**: Set credentials for logging into `yourdomain.com/wp-admin`.
    - **Admin Email**: Enter a valid email for notifications.

5. **Database Settings**
    - cPanel usually auto-generates a database name and user. You can also customize it if needed.

6. **Advanced Options** (Optional)
    - Auto-upgrade WordPress core, plugins, and themes.
    - Backup options.

7. **Install**
    - Click **Install**.
    - Wait until the installation completes.
    - Access your site at `https://yourdomain.com` and admin at `https://yourdomain.com/wp-admin`.

![](<images/wordpress_management_in_cpanel.png>)

---

## Manual WordPress Installation

Sometimes you may want full control over your WordPress setup. This involves downloading WordPress, uploading files, and
connecting a database.

**Steps:**

1. **Download WordPress**
    - Go to [WordPress.org Download](https://wordpress.org/download/) and download the latest `.zip` file.

2. **Upload to Server**
    - Login to cPanel → **File Manager** → navigate to your domain folder (`public_html` or `yourdomain.com`).
    - Upload the `.zip` file and extract it.
    - If necessary, move the contents of the `wordpress` folder to the desired location.

3. **Create a Database**
    - In cPanel → **MySQL Databases** → create a new database.
    - Create a database user and assign it to the database with **All Privileges**.
    - Note the **database name, username, and password** for later.

4. **Configure wp-config.php**
    - Rename `wp-config-sample.php` to `wp-config.php`.
    - Edit the file:
      ```php
      define('DB_NAME', 'your_database_name');
      define('DB_USER', 'your_database_user');
      define('DB_PASSWORD', 'your_database_password');
      define('DB_HOST', 'localhost');
      ```

5. **Run the Installation Script**
    - Navigate to `https://yourdomain.com/wp-admin/install.php`.
    - Fill in **Site Title**, **Admin Username**, **Password**, and **Email**.
    - Click **Install WordPress**.

### Drag-and-Drop Installers

- Some hosting providers allow uploading `.zip` files via File Manager and using **one-click extract + database setup**.
- This method requires you to connect the extracted WordPress folder to a database manually as described above.

---

## HTTPS/Domain Considerations

- Use **SSL** for HTTPS: Most cPanel setups provide **AutoSSL**.
- Ensure `https://` is selected during installation.
- If WordPress is installed manually, update the **Site URL** in wp-admin → Settings → General.

---

## Post-Installation Access

- Admin dashboard: `https://yourdomain.com/wp-admin`.
- Use the admin credentials created during setup.
- From here you can:
    - Install themes and plugins
    - Configure site settings
    - Add users and manage content

---

## File Structure Overview

When installed in cPanel, your files typically reside in `public_html` or `yourdomain.com` folder:

| File/Folder     | Purpose                                      |
|-----------------|----------------------------------------------|
| `wp-admin/`     | WordPress admin panel backend                |
| `wp-includes/`  | Core WordPress files and libraries           |
| `wp-content/`   | User content: themes, plugins, uploads       |
| `wp-config.php` | Contains database connection and credentials |
| `.htaccess`     | URL rewriting, security, and server rules    |

---

## Removing WordPress

If you need to remove WordPress:

1. **Delete Files**
    - In cPanel → File Manager → delete WordPress files/folders (`wp-admin`, `wp-content`, `wp-includes`, and
      `wp-config.php`).

2. **Drop Database**
    - In cPanel → MySQL Databases → drop the database created for WordPress.

3. **Remove Database User**
    - Delete the user associated with the database to remove credentials.

> Or you can just delete the entire folders and database from WordPress Management in cPanel if available.

---

## Summary

- **Automatic (cPanel)**: Fast, minimal setup.
- **Manual**: Full control over files, database, and folder structure.
- **HTTPS**: Always recommended.
- **Admin**: Manage the site from `/wp-admin`.
- **Removal**: Delete files + database + users.

This guide covers all essential steps a developer needs to install and manage WordPress.

