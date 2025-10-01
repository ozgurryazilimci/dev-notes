# Supabase CLI Integration

This guide shows how to integrate **Supabase** with a **React Native (Expo, TypeScript)** project using the official
`todos` example.

---

## Install Supabase CLI

Install globally:

```bash
brew install supabase/tap/supabase
# or
npm install -g supabase
```

Check version:

```bash
supabase --version
```

---

## Initialize Supabase Project

```bash
supabase init
# or
npx supabase init
```

This creates a `supabase/` folder with:

- `migrations/`: This is where your SQL migration files for table and column changes will go.
- `seed.sql`: This is where you can put SQL commands to seed initial data into your database.
- `config.toml`: This is the configuration file for your Supabase project.

---

## Start Local Supabase (Docker)

Pre-requisite: [Docker installed](https://docs.docker.com/get-docker/).

```bash
supabase start
```

This runs PostgreSQL, Studio, Auth, Realtime, and Storage locally by using Docker.

### Check Supabase Status

You can inspect the local services with:

```bash
supabase status
```

This shows useful connection details:

- **API URL** → REST API base endpoint for client requests
- **GraphQL URL** → GraphQL endpoint
- **S3 Storage URL** → Storage service endpoint
- **Database URL** → PostgreSQL connection string (use in psql/clients)
- **Studio URL** → Local Supabase Studio (browser UI)
- **Mailpit URL** → Local email testing server (see passwordless links)
- **Publishable key** → Public (client-side) key for local dev (not a real JWT)
- **Secret key** → Private (server-side) key for admin operations
- **S3 Access Key / Secret Key** → Credentials for accessing storage
- **S3 Region** → Dummy region for local S3 storage

### Stop Local Database

To stop:

```bash
supabase stop
```

Stop the local Supabase Docker containers without taking a backup:

```bash
supabase stop --no-backup
```

- Use this if you want to cleanly shut down local services.
- The `--no-backup` flag prevents creating a dump before stopping.

---

## Database Setup & RLS Policies

- You can follow the [Database Setup guide](./supabase-cli-database-setup.md) to create a `todos` table with a migration
  and add some initial data with seeds.

- You can follow the [RLS & Policies guide](./supabase-cli-rls-policies.md) to add Row Level Security policies.

---

## Environment Variables (Expo)

Create a `.env` file in your project:

- local (`.env.local`):

```env
EXPO_PUBLIC_SUPABASE_URL=http://127.0.0.1:54321
EXPO_PUBLIC_SUPABASE_ANON_KEY=eyJhbGciOiJIUzI1NiIs...
```

> Local Supabase keys are not valid JWTs. You can get anon key from local dashboard under **Settings → API** by using
> Studio URL.

- prod (`.env.production`):

```env
EXPO_PUBLIC_SUPABASE_URL=https://your-project-ref.supabase.co
EXPO_PUBLIC_SUPABASE_ANON_KEY=eyJhbGciOiJIUzI1NiIs...
```

Restart Expo:

```bash
npx expo start -c
```

---

## Supabase Client (TypeScript)

### Install Supabase SDK

```bash
npm install @supabase/supabase-js
```

### Setup Supabase Client

`utils/supabase.ts`:

```ts
import {createClient} from '@supabase/supabase-js';

export type Todo = {
    id: number;
    title: string;
    is_complete: boolean;
    inserted_at: string;
};

export type Database = {
    public: {
        Tables: {
            todos: {
                Row: Todo;
                Insert: Omit<Todo, 'id' | 'inserted_at'>;
                Update: Partial<Omit<Todo, 'id'>>;
            };
        };
    };
};

const SUPABASE_URL = process.env.EXPO_PUBLIC_SUPABASE_URL as string;
const SUPABASE_ANON_KEY = process.env.EXPO_PUBLIC_SUPABASE_ANON_KEY as string;

if (!SUPABASE_URL || !SUPABASE_ANON_KEY) {
    throw new Error('Missing Supabase environment variables');
}

export const supabase = createClient<Database>(
    SUPABASE_URL,
    SUPABASE_ANON_KEY
);
```

---

## React Native Example (Todos Screen)

`todos.tsx`:

```tsx
import React, {useState, useEffect} from 'react';
import {View, Text, FlatList, ActivityIndicator} from 'react-native';
import {supabase} from './utils/supabase';
import type {Todo} from './utils/supabase';

export default function TodosScreen() {
    const [todos, setTodos] = useState<Todo[]>([]);
    const [loading, setLoading] = useState(true);

    useEffect(() => {
        const getTodos = async () => {
            setLoading(true);
            const {data, error} = await supabase.from('todos').select('*');
            if (error) {
                console.error('Error fetching todos:', error.message);
            } else if (data) {
                setTodos(data);
            }
            setLoading(false);
        };

        getTodos();
    }, []);

    if (loading) {
        return (
            <View style={{flex: 1, justifyContent: 'center', alignItems: 'center'}}>
                <ActivityIndicator size="large" color="#2563eb"/>
            </View>
        );
    }

    return (
        <View style={{flex: 1, justifyContent: 'center', alignItems: 'center'}}>
            <Text style={{fontWeight: 'bold', fontSize: 20}}>Todo List</Text>
            <FlatList
                data={todos}
                keyExtractor={(item) => item.id.toString()}
                renderItem={({item}) => (
                    <Text>
                        {item.title} {item.is_complete ? '✅' : '⬜️'}
                    </Text>
                )}
            />
        </View>
    );
}
```

---

## Cloud Project Setup (Prod)

For your production environment, you need a **Supabase Cloud project**:

1. Go to [Supabase Dashboard](https://supabase.com/dashboard)
2. Create a project
3. Copy your **API URL** and **anon key** from **Settings → API**

### Authentication for CLI

Before linking, log in with your personal access token (one-time setup):

```bash
supabase login
```

This will open a browser where you can get your access token from the dashboard (`Account Settings → Access Tokens`).

### Linking Local Project to Cloud

Link your local Supabase project folder to a specific cloud project:

```bash
supabase link --project-ref your-project-ref
```

> `your-project-ref` is the unique identifier for your Supabase cloud project (found in project settings).

### Push Local Migrations to Cloud

Once linked, you can push your local migrations to the cloud database:

```bash
supabase db push
```

### Unlink from Cloud

If you want to disconnect your local project from the cloud:

```bash
supabase unlink
```

---

✅ Summary:

- `supabase login` → authenticate CLI with your Supabase account.
- `supabase link` → connect local project to a specific cloud project.
- `supabase db push` → apply local migrations to the cloud database.
- `supabase unlink` → remove the link between local and cloud projects.

---

## CI/CD Deployment

In a CI/CD pipeline (e.g., GitHub Actions, GitLab CI, Vercel, etc.), you can automate pushing migrations and deploying
functions.

### Generate a Personal Access Token

In the Supabase `Dashboard → Account Settings → Access Tokens`, create a new token.  
Store this securely in your CI/CD provider as a secret environment variable:

```plaintext
SUPABASE_ACCESS_TOKEN=your-token
```

### Example GitHub Actions Workflow

Create `.github/workflows/deploy.yml`:

```yaml
name: Deploy Supabase

on:
  push:
    branches:
      - master

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: supabase/setup-cli@v1
        with:
          version: latest
      - run: supabase link --project-ref $SUPABASE_PROJECT_REF
        env:
          SUPABASE_ACCESS_TOKEN: ${{ secrets.SUPABASE_ACCESS_TOKEN }}
      - run: supabase db push
        env:
          SUPABASE_ACCESS_TOKEN: ${{ secrets.SUPABASE_ACCESS_TOKEN }}
```

---

✅ With this setup:

- Every push to `master` will link your project.
- Apply new migrations automatically on the cloud database.
- No need to run `supabase login` manually in CI/CD; the access token handles authentication.

---

## Database Sync (Cloud ↔ Local)

Sometimes you need to **synchronize your local database** with the cloud (production) database for testing or
development.

### Pull Cloud Schema to Local

Fetch the current cloud database schema and create a migration file:

```bash
supabase db pull
```

- Creates a SQL file under `supabase/migrations/` with the full schema.
- Use this when local DB is empty, or you want to match cloud schema.

> Note: This only pulls the **schema**, not the data.

### Generate Migration from Differences

Compare your local DB with the cloud DB and create a migration for the differences:

```bash
supabase db diff -f add_new_table
```

- Generates a migration file in `supabase/migrations/`.
- Useful when you made changes locally and want to push them to the cloud.

### Pull Cloud Data (Optional)

If you want **existing cloud data** locally for testing:

```bash
supabase db dump --data-only > supabase/seed.sql
```

Then reset local DB and load the data:

```bash
supabase db reset
psql "postgresql://postgres:postgres@127.0.0.1:54322/postgres" -f supabase/seed.sql
```

> This will give you a local copy of cloud data for development/testing purposes.

---

✅ Summary:

- `db pull` → Pull cloud schema only
- `db diff` → Generate migration from local vs cloud differences
- `db dump --data-only` → Pull cloud data

---

## Common CLI Commands

```bash
supabase init                     # initialize project
supabase start                    # start local containers
supabase stop                     # stop local containers
supabase db reset                 # reset DB and reapply migrations
supabase db diff -f <name>        # create migration from schema changes
supabase link --project-ref <ID>  # link local project to cloud
supabase db push                  # push migrations to cloud
```

---

## Recommended Workflow

- **Use Cloud Supabase for the app** → for Auth, Realtime, RLS, and proper anon keys.
- **Use Local Docker Supabase for DB dev** → for schema changes, migrations, seeds.
- Deploy to production via `supabase db push` in CI/CD.

---

## References

- [Supabase Documentation](https://supabase.com/docs) – Official docs for all Supabase features
- [Supabase GitHub](https://github.com/supabase/supabase) – Source code, CLI, and examples
- [Supabase CLI Reference](https://supabase.com/docs/guides/cli) – Detailed commands and flags
- [Supabase Expo Integration](https://supabase.com/docs/guides/with-expo) – Using Supabase with React Native / Expo
- [Youtube Video](https://youtu.be/i1STOfZ-_R0?si=dNpdAAbFNXsCcMTd) – How to Run Supabase Project Locally with Supabase
  CLI and Docker
