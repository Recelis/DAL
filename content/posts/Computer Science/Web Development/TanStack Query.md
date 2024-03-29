+++
title = 'TanStack Query'
date = 2024-03-29T21:59:35.732Z
draft = false
+++

# TanStack Query
Helps with <b>fetching</b>, <b>caching<b>, <b>synchronizing and updating server state<b>.

## Queries
```typescript
const query = useQuery({ queryKey: ['todos'], queryFn: getTodos })
```

## Mutations
Defining a mutation looks like this:
```typescript
const mutation = useMutation({
    mutationFn: postTodo,
    onSuccess: () => {
      // Invalidate and refetch
      queryClient.invalidateQueries({ queryKey: ['todos'] })
    },
  })
```

And then calling it:
```javascript
mutation.mutate({
    id: Date.now(),
    title: 'Do Laundry',
})
```


## Query Invalidation
```typescript
```


## Typescript
TanStack is written in Typescript nowadays. But you should lock in the patch version in case the types change.

### Type Narrowing
TanStack uses <b>discriminated union types<b> for the query result as discriminated by the status field.
```typescript
const { data, isSuccess } = useQuery({
  queryKey: ['test'],
  queryFn: () => Promise.resolve(5),
})

if (isSuccess) {
  data
  //  ^? const data: number
}

const fetchGroups = (): Promise<Group[]> =>
  axios.get('/groups').then((response) => response.data)

const { data } = useQuery({ queryKey: ['groups'], queryFn: fetchGroups })
//      ^? const data: Group[] | undefined
```

#### Typing the error field
By default the type of the error is `unknown`, however you can do a runtime check which would lead to `Error | null`. Or you can explicitly define types for `data` and `error`.
```typescript
const { error } = useQuery({ queryKey: ['groups'], queryFn: fetchGroups })
//      ^? const error: unknown

if (error instanceof Error) {
  error
  // ^? const error: Error
}
```

