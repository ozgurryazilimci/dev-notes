# Expo Router

Expo Router brings **file-based routing** to React Native apps. Instead of manually registering screens in a navigation
stack, your folder and file structure inside the `app/` directory **becomes your navigation tree**. This makes building
and scaling navigation easier, faster, and more predictable.

---

## 📦 Getting Started

### Installation

If you don’t already have it:

```bash
npx create-expo-app my-app
cd my-app
npx expo install expo-router
```

### Setup

In `package.json`, make sure your entry point is:

```json
"main": "expo-router/entry"
```

Then create the `app/` folder at the root of your project (Expo already generates one if you start with the template).

---

## 🗂️ Folder Structure and Routing

Expo Router maps your **file system** to navigation routes.

### Basic Example

```
app/
 ├─ index.tsx          → route: "/"
 ├─ about.tsx          → route: "/about"
 ├─ contact.tsx        → route: "/contact"
```

- `app/index.tsx` → Home screen (`/`)
- `app/about.tsx` → About screen (`/about`)
- `app/contact.tsx` → Contact screen (`/contact`)

### Nested Routes

```
app/
 ├─ settings/
 │   ├─ index.tsx      → "/settings"
 │   ├─ profile.tsx    → "/settings/profile"
 │   └─ security.tsx   → "/settings/security"
```

### Dynamic Routes

Use `[param].tsx` for dynamic paths:

```
app/
 ├─ users/
 │   ├─ [id].tsx       → "/users/:id"
 │   └─ [id]/
 │       └─ posts.tsx  → "/users/:id/posts"
```

- `/users/123` → renders `app/users/[id].tsx` with `id = "123"`
- `/users/123/posts` → renders `app/users/[id]/posts.tsx`

You can access params using `useLocalSearchParams`:

```tsx
import { useLocalSearchParams } from "expo-router";

export default function UserScreen() {
    const {id} = useLocalSearchParams();
    return <Text>User ID: {id}</Text>;
}
```

---

## 🧭 Navigating Between Screens

### `Link` Component

For declarative navigation:

```tsx
import { Link } from "expo-router";

export default function Home() {
    return <Link href="/about">Go to About</Link>;
}
```

### `useRouter` Hook

For imperative navigation:

```tsx
import { useRouter } from "expo-router";

export default function Home() {
    const router = useRouter();

    return (
        <Button
            title="Go to Profile"
            onPress={() => router.push("/settings/profile")}
        />
    );
}
```

---

## 📐 Layouts and Nested Navigation

You can define layouts with `_layout.tsx`.

### Example

```
app/
 ├─ _layout.tsx
 ├─ index.tsx
 ├─ settings/
 │   ├─ _layout.tsx
 │   ├─ index.tsx
 │   └─ profile.tsx
```

- `app/_layout.tsx` wraps **all routes** in the app.
- `app/settings/_layout.tsx` wraps only the `settings/` routes.

```tsx
// app/_layout.tsx
import { Stack } from "expo-router";

export default function RootLayout() {
    return <Stack screenOptions={{headerShown: false}}/>;
}
```

```tsx
// app/settings/_layout.tsx
import { Stack } from "expo-router";

export default function SettingsLayout() {
    return <Stack/>;
}
```

This gives you automatic **nested navigators**.

---

## ⚡ Route Groups

Sometimes you need folders for organization without affecting the URL. Prefix the folder with `(` and `)`:

```
app/
 ├─ (auth)/
 │   ├─ login.tsx      → "/login"
 │   ├─ register.tsx   → "/register"
 ├─ (tabs)/
 │   ├─ index.tsx      → "/"
 │   ├─ feed.tsx       → "/feed"
 │   ├─ profile.tsx    → "/profile"
```

- `(auth)` and `(tabs)` do not appear in the URL.
- Useful for grouping by navigation type.

---

## 🔄 Navigation Patterns

- `router.push("/path")` → go to route, add to stack
- `router.replace("/path")` → replace current route
- `router.back()` → go back
- `router.prefetch("/path")` → prefetch screen

---

## 🛠️ Advanced Features

### Modals

You can create modal routes with `+modal.tsx`:

```
app/
 ├─ _layout.tsx
 ├─ index.tsx
 ├─ settings/
 │   └─ +modal.tsx   → shows as modal
```

### Tabs and Drawers

Layouts can be configured as **tab navigators** or **drawer navigators**:

```tsx
// app/_layout.tsx
import { Tabs } from "expo-router";

export default function TabLayout() {
    return (
        <Tabs>
            <Tabs.Screen name="index" options={{title: "Home"}}/>
            <Tabs.Screen name="about" options={{title: "About"}}/>
        </Tabs>
    );
}
```

### Middleware

You can use `middleware.ts` at the root of `app/` to run logic on navigation (e.g., auth guards).

```ts
// app/middleware.ts
import { NextResponse } from "expo-router/server";

export function middleware(request) {
    const isLoggedIn = false; // replace with real auth logic
    if (!isLoggedIn && request.url.pathname.startsWith("/settings")) {
        return NextResponse.redirect("/login");
    }
}
```

---

## 🏗️ Example Project Structure

```
my-app/
 ├─ app/
 │   ├─ _layout.tsx
 │   ├─ index.tsx
 │   ├─ about.tsx
 │   ├─ (auth)/
 │   │   ├─ login.tsx
 │   │   └─ register.tsx
 │   ├─ users/
 │   │   └─ [id].tsx
 │   └─ settings/
 │       ├─ _layout.tsx
 │       ├─ index.tsx
 │       └─ profile.tsx
 ├─ src/
 │   ├─ components/
 │   ├─ hooks/
 │   ├─ services/
 │   └─ utils/
 ├─ assets/
 ├─ global.css
 ├─ tailwind.config.js
 ├─ tsconfig.json
 └─ babel.config.js
```

---

## ✅ Key Takeaways

- `app/` is your navigation tree.
- Use `_layout.tsx` for shared layouts (Stacks, Tabs, Drawers).
- Use `[id].tsx` for dynamic routes.
- Use `(group)` folders to organize without affecting URLs.
- Use `+modal.tsx` for modal routes.
- Navigation APIs (`Link`, `useRouter`) replace `react-navigation` boilerplate.

Expo Router provides a **zero-config, scalable, and declarative navigation system** that feels like Next.js for React
Native.

---

## 📚 References

- [Expo Router Documentation](https://docs.expo.dev/router/introduction/)
- [React Navigation Docs](https://reactnavigation.org/) – useful for understanding the underlying navigation concepts
- [Expo GitHub Repository](https://github.com/expo/expo)