# Visual Studio Code

This guide provides a streamlined setup for **Visual Studio Code (VS Code)** with useful extensions, snippets, and
configuration files to improve productivity and maintain consistent formatting.

---

## 🔌 Useful Extensions

A code editor like VS Code becomes more powerful with the right extensions. Recommended ones:

- **Live Server**  
  Launch a local development server with live reload feature for static and dynamic pages.

- **PostCSS Language Support**  
  Adds syntax highlighting and IntelliSense for PostCSS.

- **Prettier - Code Formatter**  
  Opinionated code formatter to ensure consistent style.

- **Tailwind CSS IntelliSense**  
  Autocompletion, syntax highlighting, and linting for Tailwind CSS classes.

- **ES7+ React/Redux/React-Native snippets**  
  Provides JavaScript and React/React Native snippets with shortcuts.

- **VSCode Color Picker**  
  A color picker for CSS, SCSS, and other files.

---

## ⚡ Using ES7+ React/Redux/React-Native Snippets

This extension allows you to quickly scaffold common React and React Native components using simple prefixes.

### Common Shortcuts

- `rns` → React Native StyleSheet import
- `rnfe` → React Native Functional Component Export
- `rnfs` → React Native Functional Component with StyleSheet
- `rnfes` → React Native Functional Component Export with StyleSheet
- `nfn` → Named function
- `lorem3` → Generates 3 words of lorem ipsum text

### Example:

Typing `rnfe` + `Tab` expands into:

```javascript
import React from 'react';
import { View, Text } from 'react-native';

export default function MyComponent() {
  return (<View>
    <Text>MyComponent</Text>
  </View>);
}
```

### Settings

1. Ensure the extension is installed.
2. Confirm **Tab Completion** is enabled:
    - Go to **File → Preferences → Settings** (or `Ctrl+,`)
    - Search for `Tab Completion`
    - Set to **on**
3. To trigger manually, type a shortcut and press `Ctrl+Space`.

---

## 🎨 Prettier Configuration

Create a `.prettierrc.js` file in your project root for consistent formatting:

```javascript
module.exports = {
  semi: true,
  singleQuote: true,
  trailingComma: 'all',
  tabWidth: 2,
  printWidth: 100,
  arrowParens: 'always',
  bracketSpacing: true,
  jsxBracketSameLine: false,
  endOfLine: 'lf',
};
```

---

## ⚙️ VS Code Settings

Update your **`settings.json`** for optimal workflow:

```json
{
  "redhat.telemetry.enabled": true,
  "workbench.colorTheme": "Visual Studio Dark",
  "files.autoSave": "afterDelay",
  "[typescriptreact]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[json]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "editor.formatOnSave": true,
  "vscode-color-picker.languages": [
    "javascript",
    "javascriptreact",
    "typescript",
    "typescriptreact",
    "postcss",
    "xml",
    "html"
  ],
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": true,
    "source.organizeImports": true
  }
}
```

### How to Edit Settings

1. Open **Command Palette** (`Ctrl+Shift+P` / `Cmd+Shift+P` on Mac).
2. Search for **Preferences: Open Settings (JSON)**.
3. Paste the above configuration.

---

## ⌨️ Useful Keyboard Shortcuts in VS Code

Here are some handy shortcuts that improve workflow:

- **Command + .** → Quick suggestions / auto-fix options
- **Shift + Option + F** → Format document
- **Shift + Option + O** → Remove unused imports

---

## 🐞 Debugging in VS Code

### JavaScript Debugging

Debugging is straightforward in VS Code with the built-in debugger.

**Steps:**

1. Open the **Command Palette** (`Ctrl+Shift+P` / `Cmd+Shift+P` on Mac).
2. Select **JavaScript Debug Terminal**.
3. Run your development script (for example):

```bash
npm run dev
```

---

## ✅ Summary

With these extensions, snippets, and configurations:

- You’ll have **faster scaffolding** for React/React Native.
- **Consistent code formatting** across the team.
- **Improved developer experience** with auto-save, linting, and IntelliSense.

This is an all-in-one setup for a smoother development workflow in **VS Code**.
