# IntelliJ IDEA

This guide collects useful shortcuts, debugging tips, and workflow improvements for **IntelliJ IDEA**, especially for
TypeScript/JavaScript projects.

---

## ✍️ Editing & Multi-Cursor

- **Select all occurrences in a file:**  
  `Command + Control + G`  
  → Selects all instances of the highlighted word for bulk editing.

- **Select next occurrence:**  
  `Control + G`  
  → Adds the next occurrence of the selected word.

- **Deselect last occurrence:**  
  `Control + Shift + G`  
  → Removes the last selected occurrence.

[JetBrains Guide: Select All Occurrences](https://www.jetbrains.com/go/guide/tips/select-all-occurrences-in-a-file/)  
[JetBrains Guide: Multiple Occurrences](https://www.jetbrains.com/idea/guide/tips/find-select-multiple-occurrences/)

---

## 🖥️ UI Scaling

- **Zoom in IDE:** `Control + Option + =`
- **Zoom out IDE:** `Control + Option + -`

---

## 📂 Code Folding

- **Collapse all methods:** `Command + Shift + -`
- **Uncollapse all methods:** `Command + Shift + =`

- **Collapse one method:** `Command + -`
- **Uncollapse one method:** `Command + =`

---

## 🐞 Debugging

- **TypeScript Debugging:**  
  Right-click on a script inside `package.json` and select **Debug**.

---

## 🧹 Code Quality & Formatting

- **Fix ESLint Problems:**  
  Assign a custom shortcut in IntelliJ IDEA to automatically fix linting issues.

- **Recommended Order:**
    1. **Optimize Imports**
    2. **Fix ESLint Problems**

This avoids ESLint undoing optimizations.

![](<images/intellij_eslint_keymap.png>)
![](<images/intellij_eslint_preferences.png>)

---

## 📝 Patch Management

- **Create Patch** from local changes (Menu: `Git -> Patch -> Create Patch from Local Changes`)

- **Apply Patch** to update codebase (Menu: `Git -> Patch -> Apply Changes`)

Patches can be copied to clipboard or saved as files.

## ⚡ Most Used Actions & Shortcuts (IntelliJ IDEA)

Here are some of the most frequently used commands to speed up your workflow:

| Action                             | Shortcut (Mac)        | Shortcut (Windows/Linux) |
|------------------------------------|-----------------------|--------------------------|
| **Format Code**                    | Option + Command + L  | Ctrl + Alt + L           |
| **Optimize Imports**               | Control + Option + O  | Ctrl + Alt + O           |
| **Quick Suggestions / Intentions** | Option + Enter        | Alt + Enter              |
| **Find & Replace All Occurrences** | Command + Control + G | Ctrl + Alt + Shift + J   |
| **Select Next Occurrence**         | Control + G           | Ctrl + G                 |
| **Deselect Last Occurrence**       | Control + Shift + G   | Ctrl + Shift + G         |
| **Collapse All Methods**           | Command + Shift + -   | Ctrl + Shift + -         |
| **Uncollapse All Methods**         | Command + Shift + =   | Ctrl + Shift + +         |
| **Collapse Current Method**        | Command + -           | Ctrl + -                 |
| **Uncollapse Current Method**      | Command + =           | Ctrl + +                 |
| **Zoom In IDE**                    | Control + Option + =  | Ctrl + Mouse Wheel Up    |
| **Zoom Out IDE**                   | Control + Option + -  | Ctrl + Mouse Wheel Down  |

You can download the official IntelliJ IDEA keymap reference here:

[IntelliJ Keymap (PDF)](files/intellij_reference_card.pdf)