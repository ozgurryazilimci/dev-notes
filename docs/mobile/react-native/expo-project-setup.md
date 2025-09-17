# Expo React Native Project Setup Guide (Hello World)

## Prerequisites

- **Node.js** (v18+ recommended) → [Download Node.js](https://nodejs.org/)
- **npm** (comes with Node.js) → Update if needed:

```bash
# install
nvm install 20
nvm use 20

# check
node -v
npm -v
```

- **Expo CLI** (optional, can use npx)
- **Git** (optional, for version control)

---

## Create a new Expo Project

- Open terminal / command prompt.
- Run the following command to create a new project:

```bash
npx create-expo-app my-hello-world-app
```

This is equivalent to:

```bash
npm install -g expo-cli
expo init my-hello-world-app
```

- Choose **TypeScript** or **JavaScript** when prompted.
- Navigate into the project folder:

```bash
cd my-hello-world-app
```

---

## Project Structure

After creation, your project will look like:

```
my-hello-world-app/
├─ app/
│  ├─ _layout.tsx        # Navigation layout (Expo Router)
│  ├─ (tabs)/            # Optional tab navigation
│  └─ index.tsx          # Entry screen
├─ assets/              # Images, fonts
├─ components/          # Reusable components
├─ package.json
├─ tsconfig.json        # TypeScript config
└─ node_modules/
```

---

## Install Dependencies

(Optional, for UI components and navigation)

```bash
npm install react-native-paper
npm install @react-navigation/native
npm install expo-router
```

---

## Create a Simple Hello World Screen

Edit `app/index.tsx`:

```tsx
import React from 'react'
import { View, Text, StyleSheet } from 'react-native'

export default function HomeScreen() {
    return (
        <View style={styles.container}>
            <Text style={styles.title}>Hello World!</Text>
        </View>
    )
}

const styles = StyleSheet.create({
    container: {
        flex: 1,
        justifyContent: 'center',
        alignItems: 'center',
        padding: 20,
    },
    title: {
        fontSize: 24,
        fontWeight: 'bold',
    },
})
```

---

## Run the Project

- Start the development server:

```bash
npx expo start
```

- Terminal will show a QR code.

- Start the development server for Android (Android Studio Emulator or connected device):

```bash
npx expo start --android
```

---

## Mobile Device Testing

### Android / iOS

1. Install **Expo Go** app on your mobile device:
    - [Android - Google Play Store](https://play.google.com/store/apps/details?id=host.exp.exponent)
    - [iOS - App Store](https://apps.apple.com/app/expo-go/id982107779)
2. Open Expo Go → scan the QR code from terminal.
3. The app should open and display "Hello World!".

### Optional: Emulator

- **Android Studio Emulator** → Run the project directly from `npx expo start` → Click "Run on Android device/emulator"
- **iOS Simulator** (Mac only) → Run directly if Xcode installed

---

## Notes

- Any changes in `app/index.tsx` or components will **hot reload** automatically in Expo Go.
- For TypeScript, `.tsx` files are used.
- You can later add navigation, state management, API calls, and other features as your app grows.

---

## Next Steps

- Add additional screens (e.g., About, Settings)
- Add navigation (Stack or Tabs)
- Connect to backend (e.g., Supabase or Firebase)
