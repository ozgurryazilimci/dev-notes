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

### 5. **DevTools**

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
