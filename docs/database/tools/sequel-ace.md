# Database Tool: Sequel Ace

## Overview

**[Sequel Ace](https://github.com/Sequel-Ace/Sequel-Ace)** is a modern, open-source, and user-friendly macOS application
for managing MySQL and MariaDB databases.  
It is actively maintained as a successor to Sequel Pro, which is now deprecated.  
Sequel Ace supports local, socket, and SSH connections, making it easy to manage databases on macOS.

---

## Migration from Sequel Pro

Follow these steps to migrate your connections and settings from Sequel Pro to Sequel Ace:

### 1. Install Sequel Ace

You can install Sequel Ace via Homebrew:

```bash
brew install --cask sequel-ace
```

Or download it from the [Mac App Store](https://apps.apple.com/us/app/sequel-ace/id1518036000?mt=12).

### 2. Close Both Applications

Ensure both Sequel Pro and Sequel Ace are completely closed before migrating.

### 3. Migrate Preferences

Copy Sequel Pro preferences to Sequel Ace:

```bash
cp ~/Library/Preferences/com.sequelpro.SequelPro.plist ~/Library/Containers/com.sequel-ace.sequel-ace/Data/Library/Preferences/com.sequel-ace.sequel-ace.plist
```

### 4. Migrate Favorites

Copy your saved favorite connections:

```bash
cp ~/Library/Application\ Support/Sequel\ Pro/Data/Favorites.plist ~/Library/Containers/com.sequel-ace.sequel-ace/Data/Library/Application\ Support/Sequel\ Ace/Data/Favorites.plist
```

### 5. Migrate Passwords (Optional)

If you used the macOS Keychain to store passwords in Sequel Pro, you might need to re-enter them in Sequel Ace, as
Keychain entries are not automatically transferred.

For more information, refer to
the [Sequel Ace documentation](https://sequel-ace.com/get-started/migrating-from-sequel-pro.html).

### 6. Launch Sequel Ace

Open Sequel Ace. Your migrated connections and favorites should appear automatically.

---

## Creating a New Connection

1. Click **"Add Connection"** or **"+"**.
2. Choose connection type: **Standard**, **Socket**, or **SSH**.
3. Fill in details:
    - Hostname / IP
    - Port (default 3306)
    - Username & Password
    - Database (optional)
4. Test the connection and save.

---

## Key Features

- **Favorites**: Save frequently used connections.
- **Query Editor**: Write and run SQL queries with syntax highlighting.
- **Table Browser**: View and edit tables directly.
- **Import / Export**: CSV, SQL dump, or other formats.
- **SSH Tunneling**: Securely connect to remote databases.
- **Dark Mode Support**: Matches macOS system appearance.
- **Activity Log**: Monitor queries executed and execution times.

---

## Tips for Sequel Pro Users

- Keyboard shortcuts are similar, easing the transition.
- Most Sequel Pro plugins and workflows work out-of-the-box.
- Regularly check [Sequel Ace GitHub](https://github.com/Sequel-Ace/Sequel-Ace) for updates and bug fixes.
- Use **Homebrew updates** to keep Sequel Ace up-to-date:

```bash
brew upgrade --cask sequel-ace
```

---

## Useful Commands

- **List installed version**:

```bash
brew info --cask sequel-ace
```

- **Reinstall Sequel Ace** if needed:

```bash
brew reinstall --cask sequel-ace
```

---

## References

- [Sequel Ace Official Website](https://sequel-ace.com/)
- [GitHub Repository](https://github.com/Sequel-Ace/Sequel-Ace)
- [Migration Guide from Sequel Pro](https://sequel-ace.com/get-started/migrating-from-sequel-pro.html)
