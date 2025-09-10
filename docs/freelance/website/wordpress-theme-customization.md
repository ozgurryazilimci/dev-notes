# WordPress Theme Customization & Block Editor

This document introduces the main customization options available in WordPress themes and provides an overview of
designing with the WordPress 5 Block Editor.

---

## Part 1: Theme Customization

### Introduction to Theme Customization

The **Theme Customizer** allows you to adjust your site’s appearance, including fonts, colors, layout, and structural
components like headers and footers. Access it from:

- **Appearance > Customize**

---

### General Settings

#### Fonts

- Choose global fonts for headings, body text, and navigation.
- Adjust font size, weight, and line height.
- Select fallback font families.

#### Buttons

- Customize button styles (background color, hover state, border radius).
- Define typography for button labels.
- Control padding and alignment.

#### Container

- Adjust maximum site width and content container width.
- Choose boxed or full-width layout.
- Manage spacing and margins.

#### Colors

- Set a global color palette.
- Define background, text, and link colors.
- Ensure accessibility with contrast checking.

---

### Header Builder

- Configure logo, site title, and tagline.
- Add navigation menus, search bars, or custom widgets.
- Adjust layout: sticky header, transparent header, or multiple rows.

---

### Breadcrumbs

- Enable/disable breadcrumbs (navigation path).
- Choose separator style (›, /, →).
- Customize font, size, and alignment.

---

### Blog Page Customization

- Choose layout: list, grid, or masonry.
- Enable/disable featured images, metadata, and excerpts.
- Adjust pagination style (numbers, load more, infinite scroll).

---

### Sidebar Management

- Add or remove sidebar widgets.
- Configure placement (left, right, or none).
- Customize styling and widget spacing.

---

### Footer Builder

- Create multiple footer sections (columns, rows).
- Add widgets: menus, social icons, text blocks.
- Adjust background color, typography, and spacing.

---

### Theme Widgets

- Common widgets include: recent posts, categories, archives, search, and custom HTML.
- Widgets can be placed in sidebars, footers, and widget-ready areas.

---

### Menus

- Create and assign navigation menus.
- Add pages, posts, categories, or custom links.
- Configure menu locations: header, footer, mobile menu.

---

### Site Icon

- Upload a **favicon** (site icon) to display in browser tabs and mobile bookmarks.
- Recommended size: **512x512 px**.

---

### Homepage Settings

- Choose **Latest Posts** or **A Static Page** as your homepage.
- Assign a separate **Posts Page** for the blog if using a static homepage.

---

## Part 2: WordPress 5 Block Editor (Gutenberg)

### Introduction to Block Editor

The **Block Editor** (Gutenberg) is the default WordPress editor for creating posts and pages. It is block-based,
meaning each element (paragraph, image, video, etc.) is treated as a block.

---

### Block Editor Settings

- Access editor options from the top-right menu (three dots).
- Switch between **Visual Editor** and **Code Editor**.
- Enable **Top Toolbar** and **Spotlight Mode** for focused editing.

#### Block Editor Display Options

- **Full Screen Mode**: Expands the editor to take up the entire screen, hiding the WordPress admin menu.
- **Top Toolbar**: Moves all block controls to a single fixed toolbar at the top of the screen for easier access.
- **Settings Sidebar**: Toggle the sidebar to adjust block-specific and page/post-wide settings.

You can enable/disable these options from the **Editor Options menu (three dots in the top-right corner)**.

---

### Common Blocks

- **Paragraph**: Add and style text.
- **Heading**: Create headings (H1–H6).
- **Image**: Insert and align images.
- **Gallery**: Display multiple images in a grid.
- **List**: Ordered and unordered lists.
- **Quote**: Add pull quotes or citations.

---

### Formatting Blocks

- **Code**: Display snippets of code.
- **Table**: Create simple tables.
- **Preformatted**: Fixed-width text styling.
- **Reusable Blocks**: Save and reuse custom-designed blocks across pages.

---

### Layout Blocks

- **Columns**: Divide content into multiple columns.
- **Group**: Group blocks for unified styling.
- **Spacer**: Add adjustable spacing between sections.
- **Cover**: Add background images with overlay text.

---

### Social Media Blocks

- **Social Icons**: Add links to social media profiles.
- **Embed Blocks**: Embed YouTube, Twitter, Instagram, or other external content.
- **Buttons**: Add calls-to-action with links.

#### Embedding Sound from SoundCloud

To add audio from SoundCloud:

1. Copy the URL of the SoundCloud track (e.g., `https://soundcloud.com/artist/track`).
2. In the Block Editor, click the **+** icon to add a new block.
3. Search for **SoundCloud** under the **Embed** section.
4. Paste the SoundCloud URL into the block.
5. WordPress will automatically fetch and display the embedded player.

**Example:**

If you embed: `https://soundcloud.com/artist/track-name`

The block will display the SoundCloud player, allowing visitors to play the track directly on your site.

---

## Summary

- **Theme Customizer**: Control global styles (fonts, colors, layout) and structural components (header, footer,
  sidebar).
- **Block Editor**: Build content with flexible, reusable blocks, enhancing design freedom.
- Together, these tools allow for both **site-wide customization** and **content-level creativity**.
