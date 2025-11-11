# Tailwind CSS

## Introduction

Tailwind CSS is a utility-first CSS framework that provides low-level utility classes to build modern, responsive UIs
quickly. Unlike traditional CSS frameworks like Bootstrap, Tailwind doesn’t provide pre-designed components but instead
gives you building blocks to create custom designs.

### Why Tailwind?

- Faster development with utility classes
- Highly customizable via configuration
- Responsive design support out of the box
- Encourages consistent design system
- No need to leave HTML when styling

---

## Installation & Setup

### Prerequisites

- [Node.js](https://nodejs.org/) installed (`brew install node` on macOS)
- Git installed
- A code editor like [VS Code](../../tools/editors/vscode.md)

### Installation Steps

- Install Tailwind via npm:

```bash
npm init -y
npm install tailwindcss @tailwindcss/postcss postcss
npm i postcss-cli
```

- Initialize Tailwind:

```bash
npx tailwindcss init
```

- Configure `tailwind.config.js`:

```jsx
module.exports = {
  content: ["./src/**/*.{html,js,jsx,ts,tsx}"], theme: { extend: {} }, plugins: [],
}
```

- Add Tailwind to your CSS:

```css
@tailwind base;
@tailwind components;
@tailwind utilities;
```

- Run build process:

```bash
npx tailwindcss -i ./src/input.css -o ./dist/output.css --watch
```

---

## Core Concepts

### Utility-First

Tailwind provides classes like `text-center`, `mt-4`, `bg-blue-500` that can be composed to create any design.

### Responsive Design

Utilities can be applied per breakpoint:

- `sm:` for small devices
- `md:` for medium devices
- `lg:` for large devices
- `xl:` for extra-large devices
- `2xl:` for very large devices

Example:

```html

<div class="w-full md:w-1/2 lg:w-1/3">Responsive box</div>
```

### Customization

Edit `tailwind.config.js` to override or extend the default theme (colors, fonts, spacing, etc.).

---

## Utilities

### Colors

- Text: `text-red-500`
- Background: `bg-blue-200`
- Border: `border-green-400`
- Shadow: `shadow-lg`
- Accent color: `accent-*`
- Custom color variables: `style="background-color: var(--color-midnight);"`

Custom colors can be added in `tailwind.config.js`.

### Typography

- Font families: `font-sans`, `font-serif`, `font-mono`
- Font sizes: `text-xs`, `text-base`, `text-2xl`
- Font weights: `font-light`, `font-normal`, `font-medium`, `font-semibold`, `font-bold`
- Letter spacing: `tracking-tight`, `tracking-normal`, `tracking-wide`
- Text alignment: `text-left`, `text-center`, `text-right`
- Text transform: `uppercase`, `capitalize`
- Google Fonts (e.g., Roboto)

### Sizing

- Widths: `w-96`, `w-full`, `w-screen`, `w-1/2`, `w-[300px]`
- Heights: `h-24`, `h-screen`
- Max/Min sizes: `max-w-xl`, `min-h-screen`

### Spacing

- Padding: `p-4`, `px-8`, `pt-2`
- Margin: `m-2`, `mx-auto`, `mb-6`
- Space between: `space-x-4`, `space-y-2`, `-space-x-5`
- Pixel-level padding: `p-px`
- Logical properties: `ps-8`, `pe-8`, `me-8`

### Position

- `relative`, `absolute`, `fixed`
- `top-0`, `left-0`, `inset-0`, `z-50`

### Borders & Radius

- Borders: `border`, `border-2`, `border-t-4`
- Colors: `border-gray-400`
- Radius: `rounded`, `rounded-lg`, `rounded-full`, `rounded-s-lg`, `rounded-e-lg`

### Backgrounds & Shadows

- Colors: `bg-gray-100`
- Background Positions: `bg-top-right`, `bg-bottom-left`
- Fixed background: `bg-fixed`
- Gradients: `bg-gradient-to-r from-cyan-500 to-blue-500`, `bg-linear-to-l`, `bg-linear-to-bl`
- Shadows: `shadow`, `shadow-xl`, `shadow-lg`
- Outline: `outline-2`, `outline-white`

---

### Flexbox & Grid

- Flex utilities: `flex`, `flex-col`, `items-center`, `justify-between`
- Flex order: `order-1`, `order-2`
- Flex behavior: `flex-none`, `flex-initial`, `flex-auto`, `flex-1`
- Grid utilities: `grid grid-cols-3 gap-4`, `col-span-2`
- Grid row-span: `row-span-2`, column-span: `col-span-2`

#### Flex Utilities Explained

Tailwind provides several classes to control the **flex item behavior** inside a flex container. These determine how
items grow, shrink, and what initial size they take.

##### flex-none

- CSS equivalent: `flex: none;`
- Behavior:
    - The item **does not grow** (`flex-grow: 0`)
    - The item **does not shrink** (`flex-shrink: 0`)
    - Its size is fixed to the width/height you set (or content size if not set)

##### flex-initial

- CSS equivalent: `flex: 0 1 auto;`
- Behavior:
    - The item **does not grow** (`flex-grow: 0`)
    - The item **can shrink** (`flex-shrink: 1`) if there isn’t enough space
    - The **initial size** is based on its content or explicit width/height

##### flex-auto

- CSS equivalent: `flex: 1 1 auto;`
- Behavior:
    - The item **can grow** to fill available space (`flex-grow: 1`)
    - The item **can shrink** if space is limited (`flex-shrink: 1`)
    - The **initial size** is based on content

##### flex-1

- CSS equivalent: `flex: 1 1 0%;`
- Behavior:
    - The item **can grow** (`flex-grow: 1`)
    - The item **can shrink** (`flex-shrink: 1`)
    - The **initial size is ignored** (`flex-basis: 0%`), so space is distributed evenly among items

### Summary Table

| Class        | Grow | Shrink | Initial Size Considered |
|--------------|------|--------|-------------------------|
| flex-none    | No   | No     | Yes                     |
| flex-initial | No   | Yes    | Yes                     |
| flex-auto    | Yes  | Yes    | Yes                     |
| flex-1       | Yes  | Yes    | No (0% basis)           |

> These classes allow you to finely control how flex items behave, especially in **responsive layouts** or when mixing
> items of different sizes.

### Transitions & Animations

- Transition: `transition`, `duration-300`, `ease-in-out`
- Animations: `animate-bounce`, `animate-spin`

### Transforms

- Rotation: `rotate-45`
- Scale: `scale-110`
- Translate: `translate-x-4`
- 3D transforms: `hover:rotate-x-90`, `hover:rotate-y-90`
- Hover scale & translate: `hover:translate-x-full`, `hover:scale-125`

### Buttons

- Advanced states: `focus:ring-4`, `hover:bg-gray-100`
- Display: `inline-flex`, `shadow-md`

### Tables

- Striped: `odd:bg-white`, `even:bg-gray-50`
- Hover effects: `hover:bg-gray-50`, `hover:underline`

### Miscellaneous Utilities

- Cursor: `cursor-pointer`
- Group hover: `group-hover:*`
- Dividers: `divide-x-2`, `divide-gray-500`

---

## Components

### Buttons

```html

<button class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded">
  Click me
</button>
```

### Navbar

```html

<nav class="flex items-center justify-between p-4 bg-gray-800 text-white">
  <div class="text-lg font-bold">Logo</div>
  <ul class="flex space-x-4">
    <li><a href="#">Home</a></li>
    <li><a href="#">About</a></li>
    <li><a href="#">Contact</a></li>
  </ul>
</nav>
```

### Cards

```html

<div class="max-w-sm rounded overflow-hidden shadow-lg">
  <img class="w-full" src="image.jpg" alt="Sample">
  <div class="px-6 py-4">
    <div class="font-bold text-xl mb-2">Card Title</div>
    <p class="text-gray-700 text-base">Card description here.</p>
  </div>
</div>
```

### Forms

```html

<form class="space-y-4">
  <input type="text" placeholder="Name" class="w-full p-2 border rounded">
  <input type="email" placeholder="Email" class="w-full p-2 border rounded">
  <button type="submit" class="bg-green-500 text-white px-4 py-2 rounded">Submit</button>
</form>
```

### Accordion

Use [Flowbite](https://flowbite.com/) or build custom accordions with Tailwind utilities.

### Tabs

Tabs can be styled with flexbox and conditional classes for active/inactive states.

---

## Advanced Features

### Container Queries

Use `container` class and `@container` rules for more granular responsive design.

### Columns

```html

<div class="columns-2 md:columns-3 lg:columns-4 gap-4">
  <p>Content</p>
  <p>Content</p>
  <p>Content</p>
</div>
```

### Responsive Breakpoints

- Mobile-first by default
- Apply styles at specific sizes with `sm:`, `md:`, `lg:`, `xl:`, `2xl:`

---

## Common Classes

| Class            | Meaning                                                            |
|------------------|--------------------------------------------------------------------|
| `flex-1`         | Takes all available space (`flex: 1`)                              |
| `items-center`   | Align items vertically in the center (`align-items: center`)       |
| `justify-center` | Align items horizontally in the center (`justify-content: center`) |
| `bg-gray-100`    | Light gray background                                              |
| `text-xl`        | Large text                                                         |
| `font-bold`      | Bold font                                                          |
| `p-4`            | Padding 16px (depends on Tailwind scale)                           |
| `m-2`            | Margin 8px                                                         |
| `rounded-full`   | Fully rounded corners                                              |

---

## References

- [Tailwind CSS Official Docs](https://tailwindcss.com/docs)
- [Tailwind From Scratch](https://tailwindfromscratch.com/)
- [Bootstrap vs Tailwind](https://getbootstrap.com)
- [Flowbite Components](https://flowbite.com)
- [Vite](https://vite.dev/)
- [Google Fonts](https://fonts.google.com/selection/embed)
- [GitHub Tailwind Lessons](https://github.com/sadikturan/tailwind-css-dersleri)
