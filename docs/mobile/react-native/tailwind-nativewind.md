# Tailwind & NativeWind in React Native

## What Are They?

- **Tailwind CSS:** A utility-first CSS framework originally for web development. It allows you to style elements
  directly using class names like `bg-blue-500`, `p-4`, `flex`, etc., without writing custom CSS.
- **NativeWind:** A React Native adaptation of Tailwind. It brings Tailwind’s utility classes to React Native, allowing
  you to style components using `className` instead of `StyleSheet`.

**Benefits:**

1. Rapid UI prototyping.
2. Consistent styling across the app.
3. No need for StyleSheet or CSS-in-JS.

---

## Installation (Expo + React Native)

```bash
npx create-expo-app MyNativeWindApp --template
cd MyNativeWindApp

# Install NativeWind
npm install nativewind
# Initialize Tailwind config
npx tailwindcss init
```

`tailwind.config.js`:

```js
/** @type {import('tailwindcss').Config} */
module.exports = {
  content: ["./App.{js,jsx,ts,tsx}", "./src/**/*.{js,ts,jsx,tsx}"], theme: {
    extend: {},
  }, plugins: [],
};
```

---

## Basic Example (TSX)

```tsx
import React from "react";
import { View, Text, Pressable } from "react-native";
import { styled } from "nativewind";

const StyledView = styled(View);
const StyledText = styled(Text);
const StyledButton = styled(Pressable);

export default function App() {
    return (
        <StyledView className="flex-1 items-center justify-center bg-gray-100">
            <StyledText className="text-2xl font-bold text-blue-600 mb-4">
                Hello NativeWind!
            </StyledText>

            <StyledButton
                className="bg-blue-500 px-6 py-3 rounded-full"
                onPress={() => alert("Button pressed!")}
            >
                <StyledText className="text-white font-semibold text-lg">
                    Press Me
                </StyledText>
            </StyledButton>
        </StyledView>
    );
}
```

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

## Useful Resources

- **Tailwind CSS docs:** [https://tailwindcss.com/docs](https://tailwindcss.com/docs)
- **NativeWind docs:** [https://www.nativewind.dev](https://www.nativewind.dev)

> Tip: Most Tailwind classes are available in NativeWind, but pseudo-classes like `hover:` or `focus:` do not work in
> React Native.

- For more info about Tailwind CSS, visit the [Tailwind CSS](tailwind-css.md).

---

## Usage Scenarios

- Rapid prototyping
- Small-to-medium projects needing consistent design
- Cross-platform (iOS & Android) layouts