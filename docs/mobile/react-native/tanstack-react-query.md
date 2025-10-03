# TanStack React Query

TanStack React Query is a powerful library for **managing server state** in React applications. It simplifies data
fetching, caching, synchronization, and updating while keeping your UI in sync with your backend APIs.

---

## 🚀 Why React Query?

- Eliminates manual state management for server data.
- Built-in caching and background updates.
- Automatic retry and error handling.
- Works seamlessly with REST, GraphQL, or any async function.
- Great developer experience (DevTools included).

---

## 🛠 Setup

Before you can use `useQuery` or `useMutation`, you need to configure a **Query Client** and wrap your app with
`QueryClientProvider`.

First, install React Query and (optionally) DevTools:

```bash
npm install @tanstack/react-query
npm install @tanstack/react-query-devtools
```

Or with Yarn / pnpm:

```bash
yarn add @tanstack/react-query @tanstack/react-query-devtools
# or
pnpm add @tanstack/react-query @tanstack/react-query-devtools
```

After installation, you need to configure a Query Client and wrap your app with `QueryClientProvider`:

```tsx
import React from 'react'
import ReactDOM from 'react-dom/client'
import {QueryClient, QueryClientProvider} from '@tanstack/react-query'
import {ReactQueryDevtools} from '@tanstack/react-query-devtools'
import App from './App'

// Create a client instance with default options
const queryClient = new QueryClient({
    defaultOptions: {
        queries: {
            staleTime: 1000 * 60 * 5,     // 5 minutes "fresh"
            gcTime: 1000 * 60 * 10,       // 10 minutes before garbage collection
            refetchOnWindowFocus: false,  // don't auto refetch when window refocuses
            retry: 2,                     // retry failed requests up to 2 times
        },
        mutations: {
            retry: 1,                     // mutations retry once if failed
        },
    },
})

ReactDOM.createRoot(document.getElementById('root')).render(
    <React.StrictMode>
        <QueryClientProvider client={queryClient}>
            <App/>
            <ReactQueryDevtools initialIsOpen={false}/>
        </QueryClientProvider>
    </React.StrictMode>
)
```

---

## 🔑 Core Concepts

### 1. **Query**

Queries are used to fetch and cache data.

```tsx
import {useQuery} from '@tanstack/react-query'
import axios from 'axios'

async function fetchUsers() {
    const {data} = await axios.get('/api/users')
    return data
}

export default function Users() {
    const {data, error, isLoading} = useQuery({
        queryKey: ['users'],
        queryFn: fetchUsers,
    })

    if (isLoading) return <p>Loading...</p>
    if (error) return <p>Error loading users</p>

    return (
        <ul>
            {data.map(user => (
                <li key={user.id}>{user.name}</li>
            ))}
        </ul>
    )
}
```

---

### 2. **Mutation**

Mutations are used to create, update, or delete data.

```tsx
import {useMutation, useQueryClient} from '@tanstack/react-query'
import axios from 'axios'

async function addUser(user) {
    const {data} = await axios.post('/api/users', user)
    return data
}

export default function AddUser() {
    const queryClient = useQueryClient()

    const mutation = useMutation({
        mutationFn: addUser,
        onSuccess: () => {
            queryClient.invalidateQueries({queryKey: ['users']})
        },
    })

    return (
        <button
            onClick={() => mutation.mutate({name: 'New User'})}
            disabled={mutation.isLoading}
        >
            {mutation.isLoading ? 'Adding...' : 'Add User'}
        </button>
    )
}
```

---

### 3. **Query Invalidation**

Keeps cache fresh by refetching queries.

```tsx
queryClient.invalidateQueries({queryKey: ['users']})
```

---

### 4. **Query States**

- **isLoading**
- **isFetching**
- **isError**
- **isSuccess**

```tsx
const {data, isLoading, isError, error, isFetching} = useQuery({...})
```

---

### 5. **useSuspenseQuery**

- Works with **React Suspense**.
- Instead of returning loading/error states, it throws promises that Suspense boundaries can handle.

```tsx
import {useSuspenseQuery} from '@tanstack/react-query'

function UserList() {
    const {data} = useSuspenseQuery({
        queryKey: ['users'],
        queryFn: fetchUsers,
    })

    return (
        <ul>
            {data.map(user => <li key={user.id}>{user.name}</li>)}
        </ul>
    )
}

function App() {
    return (
        <React.Suspense fallback={<p>Loading...</p>}>
            <UserList/>
        </React.Suspense>
    )
}

```

---

### 6. **useQueries**

- Run **multiple queries in parallel**.
- Useful when you need several independent datasets.

```tsx
import {useQueries} from '@tanstack/react-query'

const results = useQueries({
    queries: [
        {queryKey: ['user', 1], queryFn: () => fetchUser(1)},
        {queryKey: ['posts'], queryFn: fetchPosts},
    ],
})

const [userResult, postsResult] = results
```

---

### 7. **Suspended Queries (useSuspenseQueries)**

- Same as useQueries but with Suspense.

```tsx
import {useSuspenseQueries} from '@tanstack/react-query'

const results = useSuspenseQueries({
    queries: [
        {queryKey: ['user', 1], queryFn: () => fetchUser(1)},
        {queryKey: ['posts'], queryFn: fetchPosts},
    ],
})

const [userResult, postsResult] = results
```

---

### 8. **data alias**

- Every query result has both `.data` and `.dataUpdatedAt`.
- You can alias the data prop when destructuring to avoid naming conflicts:

```tsx
const {data: users} = useQuery({
    queryKey: ['users'],
    queryFn: fetchUsers,
})

const {data: posts} = useQuery({
    queryKey: ['posts'],
    queryFn: fetchPosts,
})
```

---

### 9. **Enabled Queries**

By default, queries run automatically.  
With `enabled`, you can **conditionally fetch** data.

```tsx
const {data, isLoading} = useQuery({
    queryKey: ['user', userId],
    queryFn: () => fetchUser(userId),
    enabled: !!userId, // Only runs if userId is defined
})
```

- `enabled: false` → query does not run automatically.
- Useful for dependent queries (fetch posts **after** user is loaded).

```tsx
const {data: user} = useQuery({
    queryKey: ['user', userId],
    queryFn: () => fetchUser(userId),
})

const {data: posts} = useQuery({
    queryKey: ['posts', user?.id],
    queryFn: () => fetchPosts(user!.id),
    enabled: !!user, // wait until user exists
})

```

---

### 10. **Caching & Data Freshness**

React Query caches every query result in memory.  
Understanding **when it reuses cache vs. when it refetches** is key.

---

#### 🔑 `staleTime`

- How long the data is considered **fresh**.
- Default: `0` → data is immediately stale.
- If data is **fresh**, React Query will not refetch automatically.

```tsx
useQuery({
    queryKey: ['todos'],
    queryFn: fetchTodos,
    staleTime: 1000 * 60, // 1 minute
})
```

#### 🔑 `gcTime (cacheTime)`

- How long unused (inactive) data stays in cache before being garbage collected.
- Default: `5 minutes`.
- If another component mounts with the same queryKey during this time, the cached data is reused.

```tsx
useQuery({
    queryKey: ['todos'],
    queryFn: fetchTodos,
    cacheTime: 1000 * 60 * 10, // 10 minutes
})
```

#### 🕒 Timeline Example

With `staleTime = 1 min` and `cacheTime = 5 min`:

```pgsql
0s ─── Fetch API (first time) ───> Cached (Fresh ✅)
↓
0s - 60s → Fresh (no refetch)
↓
60s → Data becomes Stale (still cached ❗, may refetch if needed)
↓
60s - 300s → Stale but available in cache
↓
300s → cacheTime expires → Data removed from cache 🗑
↓
Next mount → API fetch again
```

#### Summary

- `staleTime` → How long data is considered fresh.
- `cacheTime` → How long inactive data remains in memory.
- **Stale ≠ deleted**: stale means “old but cached”. Deleted happens only after `cacheTime`.

---

### 11. **DevTools**

For Chrome, Firefox, and Edge users: Third-party browser extensions are available for debugging TanStack Query directly
in browser DevTools. These provide the same functionality as the framework-specific devtools packages:

- [Chrome logo Devtools for Chrome](https://chromewebstore.google.com/detail/tanstack-query-devtools/annajfchloimdhceglpgglpeepfghfai)
- [Firefox logo Devtools for Firefox](https://addons.mozilla.org/en-US/firefox/addon/tanstack-query-devtools/)
- [Edge logo Devtools for Edge](https://microsoftedge.microsoft.com/addons/detail/tanstack-query-devtools/edmdpkgkacmjopodhfolmphdenmddobj)

```tsx
import {ReactQueryDevtools} from '@tanstack/react-query-devtools'

function App() {
    return (
        <QueryClientProvider client={queryClient}>
            <MyComponents/>
            <ReactQueryDevtools initialIsOpen={false}/>
        </QueryClientProvider>
    )
}
```

---

## ⚡ Advanced Features

### ✅ Refetching

```tsx
useQuery({
    queryKey: ['users'],
    queryFn: fetchUsers,
    refetchOnWindowFocus: true,
    refetchInterval: 5000,
})
```

### ✅ Caching

```tsx
useQuery({
    queryKey: ['users'],
    queryFn: fetchUsers,
    staleTime: 1000 * 60,
    gcTime: 1000 * 60 * 5,
})
```

### ✅ Pagination & Infinite Queries

```tsx
import {useInfiniteQuery} from '@tanstack/react-query'

async function fetchPosts({pageParam = 1}) {
    const res = await fetch(`/api/posts?page=${pageParam}`)
    return res.json()
}

export default function Posts() {
    const {
        data,
        fetchNextPage,
        hasNextPage,
        isFetchingNextPage,
    } = useInfiniteQuery({
        queryKey: ['posts'],
        queryFn: fetchPosts,
        getNextPageParam: (lastPage) => lastPage.nextPage ?? false,
    })

    return (
        <div>
            {data?.pages.map((page, i) => (
                <React.Fragment key={i}>
                    {page.items.map(post => (
                        <p key={post.id}>{post.title}</p>
                    ))}
                </React.Fragment>
            ))}
            <button onClick={() => fetchNextPage()} disabled={!hasNextPage || isFetchingNextPage}>
                {isFetchingNextPage ? 'Loading more...' : 'Load More'}
            </button>
        </div>
    )
}
```

### ✅ Optimistic Updates

```tsx
const mutation = useMutation({
    mutationFn: updateUser,
    onMutate: async (newUser) => {
        await queryClient.cancelQueries({queryKey: ['users']})
        const previousUsers = queryClient.getQueryData(['users'])
        queryClient.setQueryData(['users'], (old) =>
            old.map(user =>
                user.id === newUser.id ? {...user, ...newUser} : user
            )
        )
        return {previousUsers}
    },
    onError: (err, newUser, context) => {
        queryClient.setQueryData(['users'], context.previousUsers)
    },
    onSettled: () => {
        queryClient.invalidateQueries({queryKey: ['users']})
    },
})
```

### ✅ Typescript Support

👉 In modern React Query with TypeScript, you often don’t need useEffect to react to query changes. Why?

- React Query already handles **fetching lifecycle** (loading, error, success).
- If you need to run something **after success**, you can use `onSuccess` in `useQuery` options instead of manually
  checking with `useEffect`.
- That makes your code **cleaner and more declarative**.

Here’s a typed query example fetching
from [https://jsonplaceholder.typicode.com/todos](https://jsonplaceholder.typicode.com/todos):

```tsx
import {useQuery} from '@tanstack/react-query'
import axios from 'axios'

// Define a TypeScript type for the Todo
type Todo = {
    userId: number
    id: number
    title: string
    completed: boolean
}

// Fetcher function with typing
const fetchTodos = async (): Promise<Todo[]> => {
    const {data} = await axios.get<Todo[]>('https://jsonplaceholder.typicode.com/todos')
    return data
}

export default function Todos() {
    const {data: todos, isLoading, isError, error} = useQuery<Todo[]>({
        queryKey: ['todos'],
        queryFn: fetchTodos,
        onSuccess: (data) => {
            console.log('Fetched todos:', data.length)
        },
    })

    if (isLoading) return <p>Loading...</p>
    if (isError) return <p>Error: {(error as Error).message}</p>

    return (
        <ul>
            {todos?.slice(0, 5).map(todo => (
                <li key={todo.id}>
                    {todo.title} {todo.completed ? '✅' : '❌'}
                </li>
            ))}
        </ul>
    )
}
```

🔑 **Key Points**:

- **Generics in** `useQuery<Todo[]>` tell React Query what type the data should be.
- Axios also uses `<Todo[]>` so the compiler checks the API response matches our type.
- `onSuccess` replaces the old pattern of `useEffect` to “react to data being ready”.

---

## 📚 References

- [TanStack Official Docs](https://tanstack.com/query/latest)
- [TanStack GitHub](https://github.com/TanStack/query)
- [DevTools](https://tanstack.com/query/v5/docs/framework/react/devtools)

---

## ✅ Summary

- Data fetching + caching + syncing out of the box.
- Simple hooks (`useQuery`, `useMutation`).
- Powerful tools for pagination, infinite loading, and optimistic UI.
- DevTools for debugging.

👉 Use it whenever your app relies heavily on **server-side state**.
