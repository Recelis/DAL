---
title: "React"
date: 2024-11-03T20:09:30.484Z
---

# React

## React Context

### Creating Context
[docs](https://react.dev/reference/react/createContext)

React Context allows you to pass data globally through your component tree.

You create it with `createContext`.

```typescript
const SomeContext = createContext(defaultValue);
```

The defaultValue is what the context will be if you don't have a provider above the component that is reading that context. This value is `null` by default.

It returns a context oject that doesn't actually hold any information on its own.
You will also need to create a provider component to wrap the components that need access to the context.

```typescript
function MyPage() {
  return (
    <ThemeContext.Provider value="dark">
      <Form />
    </ThemeContext.Provider>
  );
}

function Form() {
  // ... renders buttons inside ...
}
```

### Using Context
[docs](https://react.dev/reference/react/useContext)
If you want to then read the context in a component you can use `useContext`.

```typescript
const value = useContext(SomeContext);
```

You can use context with state, with useState, and this will allow you to update the context value. To do this, you will need to provide the state's updater function as part of the value of the provider.
Typically, the Provider is wrapped up as its own component.

```typescript
export default function CurrentUserProvider (props: {children: React.ReactNode}) {
  const [currentUser, setCurrentUser] = useState<User | undefined>(undefined);

// it is better to use a handler than the setCurrentUser modifier to avoid setting the type as a SetDispatch
const handleCurrentUserChange = (newUser: User) => setCurrentUser(newUser);
  return (
    <CurrentUserContext.Provider
      value={{
        currentUser,
        handleCurrentUserChange
      }}
    >
      <Form />
    </CurrentUserContext.Provider>
  );
}
```

Then you need to wrap the Provider around the component that you want access to that context.
```typescript
<CurrentUserProvider>
    <MyComponent>
</CurrentUserProvider>
```

And to use that context inside MyComponent.

```typescript
const {currentUser, handleCurrentUserChange} = useContext(CurrentUserContext);
```
