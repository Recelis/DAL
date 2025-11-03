# React

## React Context

### Creating Context

[docs](https://react.dev/reference/react/createContext)

React Context allows you to pass data globally or locally throughout your component tree.

You create it with `createContext`.

```typescript
const SomeContext = createContext<IContext | undefined>(defaultValue);
```

The defaultValue is `null` by default, but I prefer `undefined` to keep things consistent.

#### Typing Context

The Type you specify for your context will be all the state and methods you want to expose.

#### Provider

You will also need to create a provider component to wrap the components that need access to the context. The provider component tells React which components have access to this component. Without it, your `useContext` will return probably just `null`.

```typescript
function SomeProvider(props: { children: ReactNode }) {
  // state1, state2
  // method1, method2 defined

  return (
    <SomeContext.Provider value={{ state1, state2, method1, method2 }}>
      {props.children}
    </SomeContext.Provider>
  );
}
```

Your Context and Provider can be defined in one place.

When you want access to your Provider, you wrap your component inside the Provider.

```typescript
// pass Form into your Provider later...
function Form() {
  // ... renders buttons inside ...
}

// elsewhere
<SomeProvider>
  <Form />
</SomeProvider>;
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
export default function CurrentUserProvider(props: {
  children: React.ReactNode;
}) {
  const [currentUser, setCurrentUser] = useState<User | undefined>(undefined);

  // it is better to use a handler than the setCurrentUser modifier to avoid setting the type as a SetDispatch
  const handleCurrentUserChange = (newUser: User) => setCurrentUser(newUser);
  return (
    <CurrentUserContext.Provider
      value={{
        currentUser,
        handleCurrentUserChange,
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
const { currentUser, handleCurrentUserChange } = useContext(CurrentUserContext);
```
