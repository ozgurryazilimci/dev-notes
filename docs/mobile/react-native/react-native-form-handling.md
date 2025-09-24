# React Native Form Handling

This guide explains two popular ways to handle forms in a React Native TypeScript project:

1. **Formik + Yup**
2. **React Hook Form + Zod**

Both provide form state management and validation, but with different approaches.

---

## 1. Project Setup

Create a new Expo TypeScript project:

```bash
npm install -g expo-cli  
expo init MyFormApp --template expo-template-blank-typescript  
cd MyFormApp
```

---

## 2. Formik + Yup (Legacy Approach)

**Formik** is a popular form library for React and React Native.  
**Yup** is a schema validation library that integrates seamlessly with Formik.

### Installation

- npm install:

```bash
npm install formik yup @types/yup
```

- expo install:

```bash
npx expo install formik yup @types/yup
```

### Example Usage

```tsx

import React from 'react'
import { Button, Text, TextInput, View } from 'react-native'
import { Formik } from 'formik'
import * as Yup from 'yup'

const SignupSchema = Yup.object().shape({
    email: Yup.string().email('Invalid email').required('Required'),
    password: Yup.string().min(6, 'Too Short!').required('Required'),
})

export default function FormikFormExample() {
    return (
        <Formik
            initialValues={{email: '', password: ''}}
            validationSchema={SignupSchema}
            onSubmit={values => console.log(values)}
        >
            {({handleChange, handleBlur, handleSubmit, values, errors, touched}) => (
                <View style={styles.container}>
                    <TextInput
                        placeholder="Email"
                        onChangeText={handleChange('email')}
                        onBlur={handleBlur('email')}
                        value={values.email}
                        style={styles.input}
                    />
                    {errors.email && touched.email ? <Text style={styles.error}>{errors.email}</Text> : null}

                    <TextInput
                        placeholder="Password"
                        secureTextEntry
                        onChangeText={handleChange('password')}
                        onBlur={handleBlur('password')}
                        value={values.password}
                        style={styles.input}
                    />
                    {errors.password && touched.password ? <Text style={styles.error}>{errors.password}</Text> : null}

                    <Button onPress={handleSubmit as any} title="Submit"/>
                </View>
            )}
        </Formik>
    )
}

const styles = StyleSheet.create({
    container: {padding: 20},
    input: {height: 40, borderColor: 'gray', borderWidth: 1, marginBottom: 10, paddingHorizontal: 10},
    error: {color: 'red'}
})
```

### References

- [Formik](https://formik.org)
- [Yup](https://github.com/jquense/yup)

---

## 3. React Hook Form + Zod (Modern Approach)

**React Hook Form** is a lightweight form library using uncontrolled components.  
**Zod** is a TypeScript-first schema validation library that works well with React Hook Form.

### Installation

- npm install:

```bash
npm install react-hook-form zod @hookform/resolvers
```

- expo install:

```bash
npx expo install react-hook-form zod @hookform/resolvers
```

### Example Usage

```tsx
import React from 'react'
import { View, TextInput, Button, Text, StyleSheet } from 'react-native'
import { useForm, Controller } from 'react-hook-form'
import { zodResolver } from '@hookform/resolvers/zod'
import * as z from 'zod'

const schema = z.object({
    email: z.string().email('Invalid email'),
    password: z.string().min(6, 'Password too short'),
})

type FormData = z.infer<typeof schema>

export default function HookFormExample() {
    const {control, handleSubmit, formState: {errors}} = useForm<FormData>({
        resolver: zodResolver(schema)
    })

    const onSubmit = (data: FormData) => console.log(data)

    return (
        <View style={styles.container}>
            <Controller
                control={control}
                name="email"
                render={({field: {onChange, onBlur, value}}) => (
                    <>
                        <TextInput
                            placeholder="Email"
                            onChangeText={onChange}
                            onBlur={onBlur}
                            value={value}
                            style={styles.input}
                        />
                        {errors.email && <Text style={styles.error}>{errors.email.message}</Text>}
                    </>
                )}
            />

            <Controller
                control={control}
                name="password"
                render={({field: {onChange, onBlur, value}}) => (
                    <>
                        <TextInput
                            placeholder="Password"
                            secureTextEntry
                            onChangeText={onChange}
                            onBlur={onBlur}
                            value={value}
                            style={styles.input}
                        />
                        {errors.password && <Text style={styles.error}>{errors.password.message}</Text>}
                    </>
                )}
            />

            <Button onPress={handleSubmit(onSubmit)} title="Submit"/>
        </View>
    )
}

const styles = StyleSheet.create({
    container: {padding: 20},
    input: {height: 40, borderColor: 'gray', borderWidth: 1, marginBottom: 10, paddingHorizontal: 10},
    error: {color: 'red'}
})
```

### References

- [React Hook Form](https://react-hook-form.com)
- [Zod](https://github.com/colinhacks/zod)
- [Resolver Integration](https://react-hook-form.com/get-started#SchemaValidation)

---

## 4. Summary

| Feature            | Formik + Yup                   | React Hook Form + Zod       |
|--------------------|--------------------------------|-----------------------------|
| Approach           | Controlled components          | Uncontrolled components     |
| TypeScript support | Good (requires some typing)    | Excellent (Zod is TS-first) |
| Validation         | Yup schemas                    | Zod schemas via resolver    |
| Performance        | Slightly slower on large forms | Lightweight and fast        |
| More Modern        | Legacy                         | Modern                      |

Both libraries are excellent choices; it depends on your project needs and preference.

