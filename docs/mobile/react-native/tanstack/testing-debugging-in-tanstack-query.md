# Testing & Debugging in TanStack React Query

Cache invalidation is one of the trickiest parts of state management. Ensuring it's done correctly requires both
**debugging tools** and **reliable tests**.

## Developer Tools Integration

For Chrome, Firefox, and Edge users: Third-party browser extensions are available for debugging TanStack Query directly
in browser DevTools. These provide the same functionality as the framework-specific devtools packages:

- [Chrome logo Devtools for Chrome](https://chromewebstore.google.com/detail/tanstack-query-devtools/annajfchloimdhceglpgglpeepfghfai)
- [Firefox logo Devtools for Firefox](https://addons.mozilla.org/en-US/firefox/addon/tanstack-query-devtools/)
- [Edge logo Devtools for Edge](https://microsoftedge.microsoft.com/addons/detail/tanstack-query-devtools/edmdpkgkacmjopodhfolmphdenmddobj)

React Query provides **DevTools** to help visualize queries, cache states, and invalidation in real time.

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

With DevTools, you can:

- Inspect query keys and their current state
- See which queries are marked invalidated
- Debug the lifecycle of queries and mutations

---

## Writing Tests for Cache Invalidation

To ensure correctness, write tests that explicitly check query cache behavior after mutations.

### Test Utilities

```ts
// test-utils.ts
export function createTestQueryClient() {
    return new QueryClient({
        defaultOptions: {
            queries: {retry: false},
        },
        logger: {
            log: console.log,
            warn: console.warn,
            error: () => {
            }, // suppress errors during tests
        },
    })
}
```

### Example Test

```ts
// invalidation.test.ts
test('invalidates todo list after adding new todo', async () => {
    const queryClient = createTestQueryClient()

    // prefetch existing todos
    await queryClient.prefetchQuery({
        queryKey: queryKeys.todos.all,
        queryFn: () => ['todo1', 'todo2'],
    })

    // simulate mutation that adds a new todo
    await queryClient.executeMutation({
        mutationFn: () => Promise.resolve('todo3'),
        onSuccess: () => {
            queryClient.invalidateQueries({queryKey: queryKeys.todos.all})
        },
    })

    // assert cache invalidation
    expect(queryClient.getQueryState(queryKeys.todos.all)?.isInvalidated).toBe(true)
})
```

---

## References

- [TanStack Official Docs](https://tanstack.com/query/latest)
- [DevTools](https://tanstack.com/query/v5/docs/framework/react/devtools)