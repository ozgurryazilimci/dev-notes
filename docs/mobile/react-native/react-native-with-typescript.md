# React Native CLI & TypeScript Tutorials

This documentation is a complete learning path for developers who want to master **React Native CLI with TypeScript**.
Each topic includes explanations, code examples, and best practices.

---

## Hello World App

Your first React Native CLI app.

**Example:**

```tsx
import React from "react";
import { Text, View } from "react-native";

const App = () => {
    return (
        <View>
            <Text>Hello World!</Text>
        </View>
    );
};

export default App;
```

---

## Using Text

The `Text` component is used to display strings.

**Example with styling:**

```tsx
<Text style={{fontSize: 20, fontWeight: "bold", color: "blue"}}>
    Welcome to React Native with TypeScript!
</Text>
```

---

## View Component

`View` is the container for layout and styling.

**Example:**

```tsx
<View style={{backgroundColor: "#eee", padding: 20}}>
    <Text>Inside a View</Text>
</View>
```

---

## Button Usage

`Button` is used for simple press actions.

**Example:**

```tsx
<Button title="Click Me" onPress={() => alert("Button pressed!")}/>
```

---

## Components

A component is a reusable piece of UI.

**Functional Component Example:**

```tsx
type Props = { name: string };
const User: React.FC<Props> = ({name}) => <Text>Hello {name}</Text>;
const LongUser: React.FunctionComponent<Props> = ({name}) => <Text>Hello {name}</Text>;
```

---

## Props

Props are inputs to components.

**Defining and using props in a component:**

```tsx
type GreetingProps = {
    name: string;
    age: number
};

const Greeting: React.FC<GreetingProps> = ({name, age}) => {
    return <Text>Hello {name}, you are {age} years old.</Text>;
};
```

**Passing props from a parent:**

```tsx
const App = () => {
    return <Greeting name="John" age={25}/>;
};
```

---

## State with useState

`useState` manages component state.

```tsx
const [count, setCount] = useState<number>(0);

<Button title="Increase" onPress={() => setCount(count + 1)}/>
<Text>{count}</Text>
```

---

## Children (with PropsWithChildren)

Components can accept child elements.

```tsx
import React, { PropsWithChildren } from "react";
import { View } from "react-native";

const Card: React.FC<PropsWithChildren> = ({children}) => (
    <View style={{padding: 10, borderWidth: 1}}>{children}</View>
);

<Card>
    <Text>Hello inside a Card!</Text>
</Card>
```

Components can accept child elements using `PropsWithChildren`.  
You can also extend your own props interface so that both `children` and custom props are supported.

**Example with extended Props:**

```tsx
import React, { PropsWithChildren } from "react";
import { View, Text } from "react-native";

type CardProps = PropsWithChildren<{
    title: string;
}>;

const Card: React.FC<CardProps> = ({title, children}) => (
    <View style={{padding: 10, borderWidth: 1}}>
        <Text style={{fontWeight: "bold"}}>{title}</Text>
        {children}
    </View>
);

const App = () => (
    <Card title="User Info">
        <Text>Name: John</Text>
        <Text>Age: 25</Text>
    </Card>
);
```

---

## Alert

Alerts interact with users.

```tsx
<Button
    title="Show Alert"
    onPress={() => Alert.alert("Warning", "This is an alert!")}
/>
```

---

## Flexbox

**Key points:**

- Default `flexDirection` = column
- `justifyContent` = vertical alignment
- `alignItems` = horizontal alignment

```tsx
<View style={{flex: 1, justifyContent: "center", alignItems: "center"}}>
    <Text>Centered Content</Text>
</View>
```

- `flexDirection` options: `column`, `column-reverse`, `row`, `row-reverse`.

```tsx
<View style={{flexDirection: 'row'}}>
    <Text>A</Text>
    <Text>B</Text>
</View>

<View style={{flexDirection: 'row-reverse'}}>
    <Text>A</Text>
    <Text>B</Text>
</View>
```

---

## Image

```tsx
<Image
    style={{width: 100, height: 100, borderRadius: 50}}
    source={{uri: "https://placekitten.com/200/200"}}
/>
```

---

## ImageBackground

```tsx
<ImageBackground
    source={require("./bg.png")}
    style={{width: "100%", height: 200}}
    imageStyle={{borderRadius: 20}}
>
    <Text>Overlay text</Text>
</ImageBackground>
```

---

## ScrollView

```tsx
<ScrollView contentContainerStyle={{padding: 20}}>
    <Text>Item 1</Text>
    <Text>Item 2</Text>
    <Text>Item 3</Text>
</ScrollView>
```

**ScrollView with Object Mapping:**

```tsx
<ScrollView>
    {users.map(u => (
        <Text key={u.id}>{u.name}</Text>
    ))}
</ScrollView>
```

---

## TouchableOpacity

```tsx
<TouchableOpacity onPress={() => alert("Pressed!")}>
    <Text>Press Me</Text>
</TouchableOpacity>
```

---

## Custom Hook

```tsx
function useCounter(initial: number) {
    const [count, setCount] = useState<number>(initial);
    const increment = () => setCount(prev => prev + 1);
    return {count, increment};
}
```

---

## Switch

```tsx
const [enabled, setEnabled] = useState(false);

<Switch
    value={enabled}
    onValueChange={() => setEnabled(prev => !prev)}
/>
```

---

## TextInput

```tsx
<TextInput
    placeholder="Enter your name"
    value={name}
    onChangeText={setName}
    style={{borderWidth: 1, borderRadius: 5, padding: 10}}
/>
```

---

## Modal

```tsx
<Modal visible={true} transparent animationType="slide">
    <View style={{flex: 1, justifyContent: "flex-end"}}>
        <View style={{height: 200, backgroundColor: "white"}}>
            <Text>Modal Content</Text>
        </View>
    </View>
</Modal>
```

---

## Class Component

```tsx
class Counter extends React.Component<{}, { count: number }> {
    state = {count: 0};

    render() {
        return (
            <View>
                <Text>{this.state.count}</Text>
                <Button
                    title="Add"
                    onPress={() => this.setState({count: this.state.count + 1})}
                />
            </View>
        );
    }
}
```

**Lifecycle:**

- `componentDidMount` → after first render
- `componentDidUpdate` → after state/prop change
- `componentWillUnmount` → cleanup

---

## Stack Navigation

To use stack navigation, first install the required packages:

```bash
npm install @react-navigation/native @react-navigation/native-stack
```

Then configure your stack:

```tsx
import * as React from 'react';
import { Button, Text, View } from 'react-native';
import { NavigationContainer } from '@react-navigation/native';
import { createNativeStackNavigator } from '@react-navigation/native-stack';

// 1. Create the stack
type RootStackParamList = {
    Home: undefined;
    Profile: { userId: string };
};

const Stack = createNativeStackNavigator<RootStackParamList>();

// 2. Define screens
const HomeScreen = ({navigation}) => (
    <View>
        <Text>Home Screen</Text>
        <Button
            title="Go to Profile"
            onPress={() => navigation.navigate('Profile', {userId: '123'})}
        />
    </View>
);

const ProfileScreen = ({route}) => (
    <View>
        <Text>Profile Screen</Text>
        <Text>User ID: {route.params.userId}</Text>
    </View>
);

// 3. Use the stack in NavigationContainer
const App = () => (
    <NavigationContainer>
        <Stack.Navigator initialRouteName="Home">
            <Stack.Screen name="Home" component={HomeScreen}/>
            <Stack.Screen name="Profile" component={ProfileScreen}/>
        </Stack.Navigator>
    </NavigationContainer>
);

export default App;
```

---

## FlatList

```tsx
<FlatList
    data={users}
    renderItem={({item}) => <Text>{item.name}</Text>}
    keyExtractor={item => item.id.toString()}
/>
```

---

## Activity Indicator

```tsx
<ActivityIndicator size="large" color="blue"/>
```

---

## Fetch API

```tsx
useEffect(() => {
    fetch("https://dummyjson.com/users")
        .then(res => res.json())
        .then(data => setUsers(data.users));
}, []);
```

---

## Context API with useContext

```tsx
const ThemeContext = React.createContext("light");

const Child = () => {
    const theme = React.useContext(ThemeContext);
    return <Text>Theme: {theme}</Text>;
};

const App = () => (
    <ThemeContext.Provider value="dark">
        <Child/>
    </ThemeContext.Provider>
);
```

---

## Ref & memo

```tsx
const inputRef = useRef<TextInput>(null);

<TextInput ref={inputRef}/>

const Memoized = React.memo(MyComponent);
```

---

# Styling & Tools

## StyleSheet

```tsx
const styles = StyleSheet.create({
    text: {fontSize: 20, color: "red"},
});
```

---

## Formik & Yup

```tsx
<Formik
    initialValues={{email: ""}}
    validationSchema={Yup.object({
        email: Yup.string().email().required(),
    })}
    onSubmit={values => console.log(values)}
>
    {({handleChange, handleSubmit, values}) => (
        <TextInput value={values.email} onChangeText={handleChange("email")}/>
    )}
</Formik>
```

---

## Redux Toolkit Example

### Slice (counterSlice.ts)

```tsx
import { createSlice } from '@reduxjs/toolkit';

export const counterSlice = createSlice({
    name: 'counter',
    initialState: {
        count: 0,
        author: {
            name: 'John',
            surname: 'Doe'
        }
    },
    reducers: {
        increment(state) {
            state.count++;
        },
        decrement(state) {
            state.count--;
        },
        refresh(state) {
            state.count = 0;
        },
        setAuthor(state, action) {
            const name = action.payload.name;
            const surname = action.payload.surname;

            state.author.name = name;
            state.author.surname = surname;
        }
    },
});

export const {increment, decrement, setAuthor} = counterSlice.actions;
export default counterSlice.reducer;
```

### Store (store.ts)

```tsx
import { configureStore } from '@reduxjs/toolkit';
import counterReducer from './counterSlice';

export const store = configureStore({
    reducer: {counter: counterReducer},
});

export type RootState = ReturnType<typeof store.getState>;
export type AppDispatch = typeof store.dispatch;
```

### App with Provider (App.tsx)

```tsx
import React from 'react';
import { Provider } from 'react-redux';
import { store } from './store';
import Main from './Main';

const App = () => (
    <Provider store={store}>
        <Main/>
    </Provider>
);

export default App;
```

### Using useSelector (Main.tsx)

```tsx
import React from 'react';
import { Text, View } from 'react-native';
import { useSelector } from 'react-redux';
import { RootState } from './store';

const Main = () => {
    const count = useSelector((state: RootState) => state.counter.count);
    const author = useSelector((state: RootState) => state.counter.author);

    return (
        <View>
            <Text>Count: {count}</Text>
            <Text>Author: {author.name} {author.surname}</Text>
        </View>
    );
};

export default Main;
```

### Using useDispatch (CounterButtons.tsx)

```tsx
import React from 'react';
import { Button, View } from 'react-native';
import { useDispatch } from 'react-redux';
import { decrement, increment, refresh, setAuthor } from './counterSlice';

const CounterButtons = () => {
    const dispatch = useDispatch();

    return (
        <View>
            <Button title="Increment" onPress={() => dispatch(increment())}/>
            <Button title="Decrement" onPress={() => dispatch(decrement())}/>
            <Button title="Reset" onPress={() => dispatch(refresh())}/>
            <Button
                title="Set Author"
                onPress={() => dispatch(setAuthor({name: 'Alex', surname: 'Smith'}))}
            />
        </View>
    );
};

export default CounterButtons;

```

---

## AsyncStorage

```tsx
await AsyncStorage.setItem("token", "123");
const token = await AsyncStorage.getItem("token");
await AsyncStorage.removeItem("token");
```

---

# References

- [React Native](https://reactnative.dev)
- [React](https://react.dev)
- [Expo](https://docs.expo.dev)
- [Redux Toolkit](https://redux-toolkit.js.org)
- [Formik](https://formik.org)
- [Yup](https://github.com/jquense/yup)
- [Async Storage](https://react-native-async-storage.github.io/async-storage/docs/usage/)
- [React Navigation](https://reactnavigation.org/docs/getting-started)
- [Dummy Json](https://dummyjson.com/docs/)
- [Youtube Tutorial Videos](https://www.youtube.com/watch?v=CZfU-falD_s&list=PLkcIcaxfjelbSrGLKY4bKh4ppHC7IusKI)
