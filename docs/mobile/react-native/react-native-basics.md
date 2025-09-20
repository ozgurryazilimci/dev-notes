# React Native Basics

React Native enables building **cross-platform mobile apps** using **JavaScript and React**. With tools like **Expo CLI,
React Navigation, FlatList,** and **Flexbox**, you can quickly create robust, efficient, and beautiful apps for both
**iOS** and **Android**.

This document provides a step-by-step guide to setting up React Native on **Windows** and **macOS**, explains
fundamental concepts like **JSX**, **components**, **state**, **props**, **styling**, and covers advanced topics such as
**API requests**, **navigation**, **FlatList**, and **screen routing**.

---

## 🚀 React Native Setup on Windows

### Required Tools

- **Node.js** (LTS recommended)
- **npm** or **yarn**
- **React Native CLI** or **Expo CLI**
- **Android Studio** (with Android SDK & Emulator)
- **Visual Studio Code** (optional, but recommended)
- **Watchman**

### Installation Steps

- Install [Node.js](https://nodejs.org).
- Install `npm` (comes with Node.js) or `yarn`.
- Install React Native CLI globally:

```bash
npm install -g react-native-cli
```

- Initialize a new project:

```bash
react-native init project-name
```

- Run the app:

```bash
react-native run-ios
react-native run-android
```

- Download and install [Android Studio](https://developer.android.com/studio).
    - Set up the **Android SDK**.
    - Install the **Android Emulator**.
    - Configure **environment variables** (`ANDROID_HOME`).

### Running the Emulator

- Open **Android Studio** → `More Actions` → `Virtual Device Manager`.
- Create a new Android Virtual Device (AVD).
- Launch the emulator.
- Run the app:

```bash
npx react-native run-android
```

### Emulator Checklist

- Android Studio installed
- SDK tools configured
- AVD created
- Emulator successfully booted
- USB Debugging enabled (for real device testing)

### Expo CLI & Real Device Testing

- Install Expo CLI globally:

```bash
npm install -g expo-cli
```

- Initialize a new project:

```bash
npx expo-cli init project-name
```

- Start the project:

```bash
npm start
```

- Download the **Expo Go** app on your device.
- Scan the QR code to test directly on your physical device.

---

## 🍏 React Native Setup on macOS

### Required Tools

- **Node.js**
- **npm** or **yarn**
- **React Native CLI** or **Expo CLI**
- **Xcode** (with iOS Simulator)
- **Android Studio** (optional, for Android testing)
- **Watchman** (`brew install watchman`)

### Installation Steps

- Install Node.js and package managers.
- Install CLI tools (React Native CLI or Expo CLI).
- Install [Xcode](https://developer.apple.com/xcode/).
    - Includes iOS Simulator.
    - Accept license agreement: `sudo xcodebuild -license accept`
- (Optional) Install Android Studio for Android development.

### Running the iOS Simulator

- Open Xcode → Preferences → Locations → Command Line Tools.
- Launch Simulator:

```bash
npx react-native run-ios
```

### iOS Emulator Checklist

- Xcode installed
- iOS Simulator available
- Developer tools configured

### Expo CLI & Real Device Testing

- Same as Windows setup (use Expo Go app on physical device).

---

## ⚛️ React Native & JSX

### Starting a New App

```bash
npx react-native init MyApp
```

or with Expo:

```bash
npx create-expo-app MyApp
```

### React vs React Native

- **React**: Library for building UI for web apps.
- **React Native**: Uses React concepts but renders to **native mobile UI**.

### ES6 Syntax

- `let`, `const`, and `var` for variable declarations.
- `const` → immutable
- `let` and `const` → block-scoped
- Arrow functions (`()=>{}`) for cleaner syntax.
- Template literals (`` `Hello ${name}` ``).

### Helpful Tools

- [CodePen.io](https://codepen.io/pen) → test React code online.
- [Babel REPL](https://babeljs.io/repl) → see how JSX compiles to JS.

### JSX and Babel

- JSX is transformed to JavaScript using **Babel**.
- Example:

```jsx
const element = <Text>Hello JSX</Text>;
```

### First Component

```jsx
import React from 'react';
import { Text, View } from 'react-native';

const App = () => {
  return (<View>
    <Text>Hello React Native!</Text>
  </View>);
};

export default App;
```

### Rendering Components

- Root component is registered with:

```jsx
AppRegistry.registerComponent('appName', () => App);
```

- Components can be:
    - **App component** (root)
    - **Child components**

### Import/Export

```jsx
export default MyComponent;
import MyComponent from './MyComponent';
```

### Simulator Shortcuts

- **Command + R** → Refresh
- **Command + D** → Debug
- Debugger UI: [http://localhost:8081/debugger-ui/](http://localhost:8081/debugger-ui/)

---

## 🎨 Styling in React Native

### JSX Styling

```jsx
<Text style={{ color: 'blue', fontSize: 20 }}>Hello</Text>
```

### Using StyleSheet

```jsx
import { StyleSheet } from 'react-native';

const styles = StyleSheet.create({
                                   header: {
                                     fontSize: 24, fontWeight: 'bold', margin: 10,
                                   },
                                 });
```

### Props for Reusability

- Props are passed **parent → child**.

```jsx
const Header = ({ title }) => (<Text style={styles.header}>{title}</Text>);
```

### Boilerplate Shortcuts

- `rnfc` → React Native Functional Component template
- `nfn` → shorthand for arrow functions

---

## 🌐 API Requests with React Native

### Showing a List

- Use `FlatList` or `ScrollView`.

### Axios HTTP Request

```bash
npm install --save axios
```

```jsx
import axios from 'axios';
import React, { useEffect, useState } from 'react';
import { FlatList, Text } from 'react-native';

const Users = () => {
  const [data, setData] = useState([]);

  useEffect(() => {
    axios.get('https://jsonplaceholder.typicode.com/users')
      .then(response => setData(response.data));
  }, []);

  return (<FlatList
    data={data}
    keyExtractor={item => item.id.toString()}
    renderItem={({ item }) => <Text>{item.name}</Text>}
  />);
};
```

---

## 🔄 Lifecycle Methods

- **Mounting** → `componentDidMount`
- **Updating** → `componentDidUpdate`
- **Unmounting** → `componentWillUnmount`
- With hooks, use `useEffect` instead.
- In **class components**:
    - State exists by default.
    - State can only be updated via `setState`.
    - After first render, `componentDidMount` is called.
    - When state updates, component re-renders.

---

## ⚡ React Hooks: useState & useEffect

### What are Hooks?

Hooks are special functions in React that let you **use state and lifecycle features** in functional components without
writing class components.

### useState

- `useState` lets you add state to functional components.
- It returns a **state variable** and a **setter function**.

```jsx
import React, { useState } from 'react';
import { Button, Text, View } from 'react-native';

const Counter = () => {
  const [count, setCount] = useState(0);

  return (<View>
    <Text>{count}</Text>
    <Button title="Increase" onPress={() => setCount(count + 1)}/>
  </View>);
};
```

### useEffect

- `useEffect` allows you to perform **side effects** (like data fetching, subscriptions, or manual DOM changes).
- It replaces lifecycle methods (`componentDidMount`, `componentDidUpdate`, `componentWillUnmount`).

```jsx
import React, { useEffect, useState } from 'react';
import { Text } from 'react-native';

const Timer = () => {
  const [time, setTime] = useState(0);

  useEffect(() => {
    const interval = setInterval(() => setTime(t => t + 1), 1000);
    return () => clearInterval(interval); // cleanup
  }, []);

  return <Text>{time}</Text>;
};
```

### Why Hooks?

- Make functional components **stateful**.
- Avoid complex class-based code.
- Encourage **reusable and cleaner logic**.

---

## 🗂️ State in React Native

### Using State

```jsx
import React, { useState } from 'react';
import { Button, Text, View } from 'react-native';

const Counter = () => {
  const [count, setCount] = useState(0);
  return (<View>
    <Text>{count}</Text>
    <Button title="Increase" onPress={() => setCount(count + 1)}/>
  </View>);
};
```

### State Rules

- Only **setState** can update state in class components.
- Updating state triggers re-render.

### Props.Children

Reusable components can render children:

```jsx
const Card = ({ children }) => (<View style={{ padding: 20, backgroundColor: '#eee' }}>
  {children}
</View>);
```

---

## 📐 Flexbox in React Native

### Flexbox Basics

```jsx
<View style={{ flex: 1, justifyContent: 'center', alignItems: 'center' }}>
  <Text>Centered Text</Text>
</View>
```

- `flexDirection`: row or column (default is `column` in React Native)
- `justifyContent`: main axis alignment (for column, vertical alignment)
- `alignItems`: cross-axis alignment (for row, horizontal alignment)

### Scrollable Content

```jsx
import { ScrollView } from 'react-native';

<ScrollView>
  <Text>Item 1</Text>
  <Text>Item 2</Text>
</ScrollView>
```

---

## 🧭 Navigation with Expo Router

### What is a Router?

A router manages **screen navigation** inside a mobile app.

### Installing React Navigation

```bash
npm install react-navigation
npx expo-cli install react-native-gesture-handler react-native-reanimated react-navigation-stack react-native-screens @react-native-community/masked-view
```

### Navigators

- Stack Navigator
- Drawer Navigator
- Bottom Tab Navigator

### Example

```jsx
import { createStackNavigator } from '@react-navigation/stack';
import { NavigationContainer } from '@react-navigation/native';

const Stack = createStackNavigator();

const App = () => (<NavigationContainer>
  <Stack.Navigator>
    <Stack.Screen name="Home" component={HomeScreen}/>
    <Stack.Screen name="Details" component={DetailScreen}/>
  </Stack.Navigator>
</NavigationContainer>);
```

### Navigation Helpers

- `props.navigation.navigate('ScreenName')`
- `props.navigation.getParam('paramName')`
- `withNavigation` HOC from `react-navigation`

---

## 📋 FlatList

### Example

```jsx
<FlatList
  data={[{ id: '1', name: 'Apple' }, { id: '2', name: 'Banana' }]}
  keyExtractor={item => item.id}
  renderItem={({ item }) => <Text>{item.name}</Text>}
  horizontal
  showsHorizontalScrollIndicator={false}
/>
```

- **Props**: `data`, `renderItem`, `keyExtractor`, `horizontal`, `showsHorizontalScrollIndicator`
- **Key prop** ensures efficient re-rendering.

---

## 📱 Touchable Components

- **TouchableHighlight**
- **TouchableOpacity**

These provide user interaction capabilities (button-like behavior).

---

## 🔀 Routing Between Screens

### Example with Navigation

```jsx
const HomeScreen = ({ navigation }) => (<Button
  title="Go to Details"
  onPress={() => navigation.navigate('Details', { itemId: 42 })}
/>);

const DetailScreen = ({ route }) => (<Text>Item ID: {route.params.itemId}</Text>);
```

- **`navigate()`** → move to another screen
- **`params`** → pass data between screens

---

## 📚 References

- [React Native Official Documentation](https://reactnative.dev/docs/getting-started)
- [Expo Documentation](https://docs.expo.dev/)
- [React Navigation](https://reactnavigation.org/docs/getting-started)
- [Babel REPL](https://babeljs.io/repl)
- [CodePen](https://codepen.io/pen)
- [Axios GitHub](https://github.com/axios/axios)  
