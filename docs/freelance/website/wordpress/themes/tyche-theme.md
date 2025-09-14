# WordPress Theme: Tyche

Tyche is a free WordPress theme designed for **eCommerce and online stores**. It is WooCommerce-ready and comes with
customizable layouts for product showcases.

![](<images/tyche_theme.png>)

---

## Install Tyche

1. In **Appearance → Themes → Add New**, search for **Tyche**.
2. Click **Install**, then **Activate**.

---

## WooCommerce Setup

1. After activation, install and activate the **WooCommerce plugin** if not already installed.
2. Complete the setup wizard:
    - Store address
    - Currency
    - Shipping
    - Payment methods

---

## Customize the Theme

- Go to **Appearance → Customize**.
- Configure homepage sections such as featured products, banners, and categories.
- Adjust typography, colors, and header layout.

---

## Demo Import (Optional)

Tyche supports demo content import to help you get started quickly.

- **Demo XML** → `tyche-demo.xml`
- **Widgets Import File** → `tyche-widgets.wie`

You can import these files using:

- **One Click Demo Import** plugin
- Or via **Tools → Import** in the WordPress dashboard

---

## Fixing Homepage Slider

You may need to install additional plugins to fix the homepage slider and widgets after disabling the
default homepage slider on Tyche theme:

- **Smart Slider** → For homepage sliders and banners.
- **Classic Widgets** → To manage traditional sidebar/footer widgets.

---

## Fixing Top Search for Products

By default, Tyche’s top search bar does not always filter WooCommerce products correctly.  
To fix this:

- Navigate to **Appearance → Theme File Editor**.
- Open the file: `page-templates/template-parts/top-header.php`
- Find the class:

```html
<input type="search" class="search-field-top-bar" ...>
```

- After this line, insert the following hidden input:

```html
<input type="hidden" name="post_type" value="product">
```

This ensures that the top search bar only searches for WooCommerce products.

---

## Adding Products

1. Go to **Products → Add New**.
2. Fill in product details:
    - Title
    - Description
    - Price
    - Images
    - Categories
3. Publish to display it in the shop.

---

## Launch the Store

- Preview your site with demo content and products.
- Adjust sliders, widgets, and theme settings.
- Publish your store and start selling online.
