# WordPress Plugin: Loco Translate

This guide explains how to install, configure, and use the **Loco Translate** plugin to translate WordPress themes and
plugins directly from the WordPress admin dashboard.

---

## Introduction

Loco Translate is a popular WordPress plugin that provides in-browser editing of translation files (`.po` / `.mo`).  
It is especially useful for customizing the language of themes and plugins without editing code manually.

---

## Installation

1. Log in to your **WordPress Admin Dashboard**.
2. Navigate to **Plugins → Add New**.
3. In the search bar, type: `Loco Translate`
4. Find the plugin **Loco Translate by Tim Whitlock**.
5. Click **Install Now**, then **Activate**.

---

## Translating a Theme

1. Go to **Loco Translate → Themes**.
2. Select the theme you want to translate (e.g., Astra, Storefront, Cenote, Tyche).
3. Click **New Language**.
4. Choose your **language** and **location** for saving translation files:
    - Recommended: `Custom (languages/loco/themes/)` or **System directory** to prevent overwriting during updates.
5. Use the built-in editor to translate text strings.
6. Click **Save** when finished.

---

## Translating a Plugin

1. Go to **Loco Translate → Plugins**.
2. Select the plugin you want to translate (e.g., WooCommerce, Join.chat).
3. Click **New Language**.
4. Select your target language and save location.
5. Translate the text strings using the Loco Translate editor.
6. Save your translations.

---

## Editing Existing Translations

- Navigate to the theme or plugin inside **Loco Translate**.
- Click on the existing language file.
- Update strings as needed and save changes.
- Compile `.mo` files automatically using the editor.

---

## Best Practices

- Always **back up translations** before updating themes/plugins.
- Store translations in the **Custom directory** (`languages/loco/`) or **System directory** to avoid losing them after
  updates.
- Regularly check for **new strings** after theme/plugin updates.
- Use **search and filter** options to find strings quickly.

---

## Example Use Cases

- Translate **WooCommerce** buttons (e.g., “Add to Cart” → “Sepete Ekle”).
- Localize **themes** for multilingual websites.
- Customize wording in plugins like **Join.chat** or **Loco Translate** itself.

---

## Resources

- [Loco Translate Plugin Page](https://wordpress.org/plugins/loco-translate/)
- [Loco Translate Documentation](https://localise.biz/wordpress/plugin/manual)
- [WordPress Language Packs](https://make.wordpress.org/polyglots/teams/)
