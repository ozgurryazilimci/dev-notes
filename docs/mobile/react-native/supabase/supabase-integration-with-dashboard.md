# Supabase Integration With Dashboard

## Prerequisites

- Node.js installed (v18+ recommended)
- npm updated
- Expo project already created (see Hello World guide)
- Supabase account and project setup → [https://supabase.com](https://supabase.com)

---

## Configure Supabase Project

1. Go to [Supabase Dashboard](https://app.supabase.com/).
2. Create a new project (choose organization, project name, database password).
3. In the project **Settings → API**:
    - Copy your **Project URL** (supabaseUrl)
    - Copy your **anon public key** (supabaseKey)
4. Enable **Authentication**:
    - Navigate to **Authentication → Providers**
    - Enable **Email** provider (so users can sign up with email & password)
    - Configure SMTP if you want to send real emails. For testing, Supabase provides a **magic link preview** in the
      dashboard.

---

## Install Supabase SDK

```bash
npm install @supabase/supabase-js
```

---

## Setup Supabase Client

Create `supabaseClient.js` in the root of the project:

```js
import { createClient } from '@supabase/supabase-js'

const supabaseUrl = 'YOUR_SUPABASE_URL'
const supabaseKey = 'YOUR_SUPABASE_ANON_KEY'
export const supabase = createClient(supabaseUrl, supabaseKey)
```

Replace placeholders with your credentials from **Settings → API**.

---

## Database Setup (Supabase SQL)

**Tables:**

```sql
-- Companies table
create table companies (
    id uuid primary key default gen_random_uuid(),
    name text not null,
    created_at timestamp default now()
);

-- Users table (custom profile info)
create table users (
    id uuid primary key,
    email text unique not null,
    company_id uuid references companies(id),
    role text check (role in ('admin', 'employee')) default 'employee',
    created_at timestamp default now()
);
```

> Note: `auth.users` table is automatically managed by Supabase for authentication. The `users` table here is for extra
> profile/company information.

---

## Signup Screen Example (`app/index.tsx`)

```tsx
import React, { useState } from 'react'
import { View, StyleSheet, Text } from 'react-native'
import { TextInput, Button } from 'react-native-paper'
import { supabase } from '../supabaseClient'
import { useRouter } from 'expo-router'

export default function SignupScreen() {
    const [email, setEmail] = useState('')
    const [password, setPassword] = useState('')
    const [message, setMessage] = useState('')
    const router = useRouter()

    const signUp = async () => {
        const {data, error} = await supabase.auth.signUp({email, password})
        if (error) {
            setMessage(error.message)
            return
        }

        // Insert into custom users table
        await supabase.from('users').insert([{id: data.user.id, email: data.user.email}])

        setMessage('Check your email for confirmation!')
    }

    return (
        <View style={styles.container}>
            <TextInput label="Email" value={email} onChangeText={setEmail} style={styles.input}/>
            <TextInput label="Password" value={password} onChangeText={setPassword} secureTextEntry
                       style={styles.input}/>
            <Button mode="contained" onPress={signUp}>Sign Up</Button>
            {message ? <Text>{message}</Text> : null}
        </View>
    )
}

const styles = StyleSheet.create(
    {container: {flex: 1, justifyContent: 'center', padding: 20}, input: {marginBottom: 10}})
```

---

## Add Company Screen (`app/add-company.tsx`)

```tsx
import React, { useState } from 'react'
import { View, StyleSheet, Text } from 'react-native'
import { TextInput, Button } from 'react-native-paper'
import { supabase } from '../supabaseClient'
import { useRouter } from 'expo-router'

export default function AddCompany() {
    const [companyName, setCompanyName] = useState('')
    const [message, setMessage] = useState('')
    const router = useRouter()

    const handleAddCompany = async () => {
        const {data: {user}, error: userError} = await supabase.auth.getUser()
        if (userError || !user) {
            setMessage('User not found')
            return
        }

        const {data: company, error: companyError} = await supabase.from('companies')
            .insert([{name: companyName}]).select().single()

        if (companyError || !company) {
            setMessage(companyError?.message || 'Company not created')
            return
        }

        const {error: updateError} = await supabase.from('users')
            .update({company_id: company.id}).eq('id', user.id)

        if (updateError) {
            setMessage(updateError.message)
            return
        }

        router.replace('/(tabs)/explore')
    }

    return (
        <View style={styles.container}>
            <TextInput label="Company Name" value={companyName} onChangeText={setCompanyName} style={styles.input}/>
            <Button mode="contained" onPress={handleAddCompany}>Add Company</Button>
            {message ? <Text>{message}</Text> : null}
        </View>
    )
}

const styles = StyleSheet.create(
    {container: {flex: 1, justifyContent: 'center', padding: 20}, input: {marginBottom: 10}})
```

---

## Login and Dashboard Routing

- On login, fetch `users.company_id`.
- If `company_id` is **null** → navigate to `/add-company`.
- If `company_id` exists → navigate to `/(tabs)/explore`.
- Use `useRouter()` from Expo Router.

---

## Testing Authentication

- Run app: `npx expo start`
- In Supabase Dashboard → **Authentication → Users**, you can see registered accounts.
- When you sign up, an email confirmation is sent:
    - If SMTP configured → check your real inbox.
    - If SMTP not configured → check **Supabase Dashboard → Authentication → Users → Click user → Confirm account**
      manually.
- After confirmation, login works normally.

---

## Notes

- Always use `supabase.auth.getUser()` for v2 SDK.
- Keep `auth.users` (system table) and `users` (custom table) in sync.
- Email confirmations require SMTP settings for production apps.
- You can test login/signup flow directly in **Supabase Dashboard** before wiring UI.
