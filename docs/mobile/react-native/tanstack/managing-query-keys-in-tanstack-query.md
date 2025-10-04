# Managing Query Keys & Cache Invalidation in TanStack React Query

## Introduction

As React Query is used more broadly in growing apps, issues like **stale data**, **memory usage**, and **missed
invalidations** become painful. The article argues that without consistent strategies, teams may ship bugs due to
forgotten cache invalidations. It emphasizes that **query keys** and how you organize/invalidate them are foundational
to a robust caching layer.

---

## Understanding Query Keys: The Foundation

- A **query key** is the unique identifier React Query uses to cache data.
- It can be a string, array, or nested structures, e.g.:

    - `['todos']`
    - `['todo', todoId]`
    - `['todos', { status: 'done', userId: 1 }]`

- React Query uses deterministic hashing so:

    1. Order of object properties doesn’t matter
    2. Arrays and objects get consistently hashed
    3. Equivalent keys (e.g. `{status, userId}` vs `{userId, status}`) resolve to the same cache entry

This determinism is key to reliably invalidating queries or grouping them logically.

---

## Organizing Query Keys: Patterns & Strategies

The article explains several patterns, from simpler to more structured:

### 1. Co-Location Pattern

Query logic stays inline where it’s used. Example:

```tsx
function UserProfile({userId}) {
    const {data: user} = useQuery({
        queryKey: ['user', userId],
        queryFn: () => fetchUser(userId)
    })
    const {data: posts} = useQuery({
        queryKey: ['posts', userId],
        queryFn: () => fetchUserPosts(userId)
    })
    return (/* render UI */)
}
```

**Pros:**

- Simple, intuitive
- Clear dependency between UI and data

**Cons:**

- Duplication of query key definitions
- Hard to maintain consistency across codebase
- Invalidation logic can get scattered

### 2. Custom Hooks Pattern

Extracts query logic into reusable hooks:

```ts
// hooks/useUser.ts
export function useUser(userId: string) {
    return useQuery({
        queryKey: ['user', userId],
        queryFn: () => fetchUser(userId)
    })
}

// hooks/useUserPosts.ts
export function useUserPosts(userId: string) {
    return useQuery({
        queryKey: ['posts', userId],
        queryFn: () => fetchUserPosts(userId)
    })
}

// In component
function UserProfile({userId}) {
    const {data: user} = useUser(userId)
    const {data: posts} = useUserPosts(userId)
    return (/* render UI */)
}
```

This helps DRY (don’t repeat yourself) the query logic, but still relies on consistent key usage.

### 3. Query Key Factory Pattern

Centralize your query key definitions in one file, to avoid typos and keep consistency:

```ts
// queryKeys.ts
export const queryKeys = {
    users: {
        all: ['users'] as const,
        detail: (id: string) => ['users', id] as const,
        posts: (id: string) => ['users', id, 'posts'] as const,
    },
    posts: {
        all: ['posts'] as const,
        detail: (id: string) => ['posts', id] as const,
        comments: (id: string) => ['posts', id, 'comments'] as const,
    },
}
```

Then in hooks:

```ts
function useUser(id: string) {
    return useQuery({
        queryKey: queryKeys.users.detail(id),
        queryFn: () => fetchUser(id),
    })
}
```

When invalidating:

```ts
queryClient.invalidateQueries({
    queryKey: queryKeys.users.posts(userId)
})
queryClient.invalidateQueries({
    queryKey: queryKeys.users.all
})
```

This pattern is especially useful when you have multiple related queries that need consistent invalidation.

### 4. Advanced Query Options Pattern

Combine key definitions with default query option logic:

```ts
// queryOptions.ts
interface QueryConfig {
    staleTime?: number
    cacheTime?: number
    retry?: boolean | number
}

export function createQueryOptions<T>(
    key: readonly unknown[],
    fetcher: () => Promise<T>,
    config?: QueryConfig
) {
    return {
        queryKey: key,
        queryFn: fetcher,
        staleTime: config?.staleTime ?? 5 * 60 * 1000,
        cacheTime: config?.cacheTime ?? 30 * 60 * 1000,
        retry: config?.retry ?? 3,
    }
}

// hooks/useUser.ts
export function useUser(id: string) {
    return useQuery(
        createQueryOptions(
            queryKeys.users.detail(id),
            () => fetchUser(id),
            {staleTime: 60 * 1000}
        )
    )
}
```

This ensures your queries have consistent configurations across the app and makes overrides easier.

---

## Effective Cache Invalidation Strategies

### 1. Centralized Invalidation Service

Create a service module to manage all invalidation logic:

```ts
class QueryInvalidationService {
    constructor(private queryClient: QueryClient) {
    }

    invalidateUser(userId: string) {
        this.queryClient.invalidateQueries({
            queryKey: queryKeys.users.detail(userId)
        })
    }

    invalidateUserPosts(userId: string) {
        this.queryClient.invalidateQueries({
            queryKey: queryKeys.users.posts(userId)
        })
    }

    invalidatePost(postId: string) {
        this.queryClient.invalidateQueries({
            queryKey: queryKeys.posts.detail(postId)
        })
        this.queryClient.invalidateQueries({
            queryKey: queryKeys.posts.comments(postId)
        })
    }
}
```

Then use it in your mutation hooks to keep invalidations consistent and centralized.

### 2. Optimistic Updates with Rollback

Update cache immediately, and revert on error:

```ts
function useUpdateTodo() {
    const queryClient = useQueryClient();
    return useMutation({
        mutationFn: updateTodo,
        onMutate: async (newTodo) => {
            await queryClient.cancelQueries({
                queryKey: queryKeys.todos.detail(newTodo.id)
            });
            const previous = queryClient.getQueryData(
                queryKeys.todos.detail(newTodo.id)
            );
            queryClient.setQueryData(
                queryKeys.todos.detail(newTodo.id),
                newTodo
            );
            return {previous};
        },
        onError: (err, newTodo, context) => {
            queryClient.setQueryData(
                queryKeys.todos.detail(newTodo.id),
                context?.previous
            );
        },
        onSettled: (newTodo) => {
            queryClient.invalidateQueries({
                queryKey: queryKeys.todos.detail(newTodo.id)
            });
        },
    });
}
```

### 3. Selective Invalidation with Predicates

Use `predicate` to target only specific queries:

```ts
queryClient.invalidateQueries({
    queryKey: queryKeys.todos.all,
    predicate: (query) => {
        const [, filters] = query.queryKey;
        return filters?.status === 'completed';
    }
})
```

### 4. Background Data Updates

Keep data up-to-date using `refetchOnWindowFocus`, `refetchInterval`, etc.:

```ts
function useTodoList() {
    return useQuery({
        queryKey: queryKeys.todos.all,
        queryFn: fetchTodos,
        refetchOnWindowFocus: true,
        refetchInterval: 60 * 1000,
        refetchIntervalInBackground: false,
    })
}
```

---

## Testing & Debugging Invalidation

- Use **React Query DevTools** to visualize cache states and query lifecycles.
- Create test query clients with controlled options (e.g. `retry: false`) to assert that invalidations happen as
  expected.
- Write tests that check whether queries become invalidated after mutations.

You can follow this [documentation](testing-debugging-in-tanstack-query.md) for more details.

---

## Common Pitfalls & Mitigations

### 1. Over-invalidation

Invalidating too broadly can lead to over-fetching. Use precise keys and avoid invalidating everything.

### 2. Stale-While-Revalidate Glitches

Users may briefly see stale data. Counter this by using `staleTime`, `keepPreviousData`, or optimistic updates.

### 3. Memory Leaks

If `cacheTime` is too long and you keep many keys alive, memory usage may grow. Balance caching vs cleanup.

### 4. Missed Invalidations

One of the biggest risks is forgetting to invalidate dependent queries after a mutation. Use centralized logic or key
factories to reduce that risk.

---

## Best Practices for Scaling

1. Maintain a **consistent query key structure** across your project.
2. Use **TypeScript-safe query keys** (e.g. `as const`) to reduce typos.
3. Create **standard configurations** (stale / cache / retry) for different categories of queries.
4. Organize your directory structure for query-related code:

```
src/
  queries/
    keys/
      userKeys.ts
      todoKeys.ts
    hooks/
      user/
        useUser.ts
        useUserPosts.ts
      todo/
        useTodoList.ts
        useUpdateTodo.ts
    services/
      invalidation/
        userInvalidation.ts
        todoInvalidation.ts
  config/
    queryConfig.ts
    queryClient.ts
```

---

## Conclusion

Managing query keys and invalidation in React Query is challenging but manageable. The article recommends a progression
from **co-location → custom hooks → key factories → advanced options**, combined with centralized invalidation,
optimistic updates, and background refreshes. With these patterns, your caching logic can scale with your app without
becoming fragile.

---

## References

- [Managing Query Keys for Cache Invalidation in React Query — Wisp](https://www.wisp.blog/blog/managing-query-keys-for-cache-invalidation-in-react-query)
- [React Query Documentation](https://tanstack.com/query/latest)  
