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

```typescript
export default function MyApp() {
  const [currentUser, setCurrentUser] = useState(null);
  return (
    <CurrentUserContext.Provider
      value={{
        currentUser,
        setCurrentUser
      }}
    >
      <Form />
    </CurrentUserContext.Provider>
  );
}
```
