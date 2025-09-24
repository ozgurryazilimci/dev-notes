# Nativewind

This guide walks you through integrating Nativewind (Tailwind CSS for React Native) into an Expo Router project using
TypeScript.

---

## Prerequisites

- **Node.js**: Ensure you're using Node.js version 20. If you're using [nvm](https://github.com/nvm-sh/nvm), you can
  switch to Node.js 20 with:

```bash
nvm use 20
```

---

### Step 1: Create a New Expo Project with TypeScript and Expo Router

```bash
npx create-expo-app@latest my-app --template expo-router --typescript
cd my-app
```

This command sets up a new Expo project with Expo Router and TypeScript.

---

### Step 2: Install Dependencies

Install Nativewind and its peer dependencies:

- npm install:

```bash
npm install nativewind react-native-reanimated@~3.17.4 react-native-safe-area-context@5.4.0
```

- expo install:

```bash
npx expo install nativewind react-native-reanimated@~3.17.4 react-native-safe-area-context@5.4.0
```

Install development dependencies:

- npm install:

```bash
npm install --save-dev tailwindcss@^3.4.17 prettier-plugin-tailwindcss@^0.5.11
```

- expo install:

```bash
npx expo install --dev tailwindcss@^3.4.17 prettier-plugin-tailwindcss@^0.5.11
```

---

### Step 3: Initialize Tailwind CSS

Generate the Tailwind configuration file:

```bash
npx tailwindcss init
```

Update `tailwind.config.js` to include the paths to your component files:

```js
/** @type {import('tailwindcss').Config} */
module.exports = {
  // NOTE: Update this to include the paths to all files that contain Nativewind classes.
  content: ["./app/**/*.{js,jsx,ts,tsx}"], presets: [require("nativewind/preset")], theme: {
    extend: {},
  }, plugins: [],
}
```

---

### Step 4: Create Global CSS File

Create a `global.css` file inside the `app` folder:

```css
@tailwind base;
@tailwind components;
@tailwind utilities;
```

This file ensures Tailwind utilities and styles are available throughout your app.

---

### Step 5: Configure Babel

Create or update the `babel.config.js` file in the root of your project:

```js
module.exports = function (api) {
  api.cache(true);
  return {
    presets: [["babel-preset-expo", { jsxImportSource: "nativewind" }], "nativewind/babel",],
  };
};
```

This configuration ensures Expo's Babel setup works seamlessly with Nativewind.

---

### Step 6: Configure Metro Bundler

Create or update the `metro.config.js` file in the root of your project:

```js
const { getDefaultConfig } = require("expo/metro-config");
const { withNativeWind } = require('nativewind/metro');

const config = getDefaultConfig(__dirname)

module.exports = withNativeWind(config, { input: './app/global.css' })
```

This configuration integrates Nativewind with Expo's Metro bundler.

---

### Step 7: Import Global CSS in Your App

In your `app/_layout.tsx` file, import the global CSS file:

```tsx
import { Stack } from 'expo-router';
import './global.css';

export default function RootLayout() {
    return <Stack screenOptions={{headerShown: false}}/>;
}
```

This ensures that Tailwind's styles are applied globally across your app.

---

### Step 8: Modify your `app.json`

Switch the bundler to use the Metro bundler:

```json
{
  "expo": {
    "web": {
      "bundler": "metro"
    }
  }
}
```

---

### Step 9: TypeScript Setup (Optional)

If you're using TypeScript, create a `nativewind-env.d.ts` file in the root of your project with the following content:

```ts
/// <reference types="nativewind/types" />
```

This step enables TypeScript support for Nativewind's utility types.

---

### Step 10: Verify the Setup

To verify that Nativewind is working correctly, modify your `app/index.tsx` file as follows:

```tsx
import { Text, View } from 'react-native';

export default function App() {
    return (
        <View className="flex-1 items-center justify-center bg-white">
            <Text className="text-xl font-bold text-blue-500">
                Welcome to Nativewind!
            </Text>
        </View>
    );
}
```

Run your project:

```bash
npx expo start
```

If you see the styled text centered on a white background, Nativewind is working correctly.

---

## Additional Notes

- **Editor Setup**: To enhance your development experience, consider setting up your editor to support Nativewind's
  IntelliSense and linting features.
- **Troubleshooting**: If you encounter issues, ensure that all paths in your `tailwind.config.js` are correct and that
  all necessary files are properly configured.

---

## References

- [Nativewind Official Docs](https://www.nativewind.dev/docs)
- [Nativewind Installation](https://www.nativewind.dev/docs/getting-started/installation)
- [Youtube Installation Tutorial](https://youtu.be/WcumWxicmao?si=f7xr0nz_tFv9T8sJ)

