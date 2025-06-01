# React

## Updating State

Have to treat state as immutable because React does not know when things get updated until the setState function is called. So while you can mutate state i.e.

```typescript
const [myState, setMyState] = useState({ fish: 1 });

myState.fish = 2;
```

React doesn't actually know that myState.fish has been changed.

## React Context

### Creating Context

[docs](https://react.dev/reference/react/createContext)

React Context allows you to pass data globally through your component tree.

You create it with `createContext`.

```typescript
const SomeContext = createContext(defaultValue);
```

The defaultValue is what the context will be if you don't have a provider above the component that is reading that context. This value is `null` by default.

It returns a context object that doesn't actually hold any information on its own.
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

### Extracting State Logic into a Reducer

[docs](https://react.dev/learn/extracting-state-logic-into-a-reducer)

If you are constantly reusing a setState function, you can move this logic to one place called a `reducer`.

```typescript
function handleAddTask(text) {
  setTasks([
    ...tasks,
    {
      id: nextId++,
      text: text,
      done: false,
    },
  ]);
}

function handleChangeTask(task) {
  setTasks(
    tasks.map((t) => {
      if (t.id === task.id) {
        return task;
      } else {
        return t;
      }
    })
  );
}

function handleDeleteTask(taskId) {
  setTasks(tasks.filter((t) => t.id !== taskId));
}
```

This can be refactored using the `dispatch` command which passes in an `action` that has a `type`.

Inside your reducer, you will write your logic for handing the different actions.

```typescript
function tasksReducer(tasks, action) {
  switch (action.type) {
    case "added": {
      return [
        ...tasks,
        {
          id: action.id,
          text: action.text,
          done: false,
        },
      ];
    }
    case "changed": {
      return tasks.map((t) => {
        if (t.id === action.task.id) {
          return action.task;
        } else {
          return t;
        }
      });
    }
    case "deleted": {
      return tasks.filter((t) => t.id !== action.id);
    }
    default: {
      throw Error("Unknown action: " + action.type);
    }
  }
}
```

Finally, you will replace the:

```typescript
const [tasks, setTasks] = useState(initialTasks);
```

with:

```typescript
import { useReducer } from "react";

const [tasks, dispatch] = useReducer(tasksReducer, initialTasks);

// to call specific dispatches

dispatch({
  type: "deleted",
  id: "1",
});
```

### Scaling Up with Reducer and Context

[docs](https://react.dev/learn/scaling-up-with-reducer-and-context)

## Hooks

### useCallback

[docs](https://react.dev/reference/react/useCallback)

When a component rerenders, React will rerender all of its children recursively. So `useCallback` lets you cache a function definition between re-renders. However, because whenever`() => {}` or `function () {}` are called, it will always create a new function (similar to how calling {} will create a new object everytime), then putting a standard `memo` around a component with a function passed in will not cache it.

```javascript
import { memo } from "react";
// onSubmit will be created each time causing this to be rerendered even if none of its props have changed.
const ShippingForm = memo(function ShippingForm({ onSubmit }) {
  // ...
});
```

```javascript
function ProductPage({ productId, referrer, theme }) {
  // Tell React to cache your function between re-renders...
  const handleSubmit = useCallback(
    (orderDetails) => {
      post("/product/" + productId + "/buy", {
        referrer,
        orderDetails,
      });
    },
    [productId, referrer]
  ); // ...so as long as these dependencies don't change...

  return (
    <div className={theme}>
      {/* ...ShippingForm will receive the same props and can skip re-rendering */}
      <ShippingForm onSubmit={handleSubmit} />
    </div>
  );
}
```

Only do this if you notice that the page is freezing from some change in the component/s above.

Rerendering of components can be turned on by going into `Profiler` => `gear icon` => `Highlight updates when components render.`

### useLayoutEffect

### useMemo

### useReducer

## Escape Hatches

### Refs

[docs](https://react.dev/learn/escape-hatches)

### Lifecycle of Reactive Effects

[docs](https://react.dev/learn/lifecycle-of-reactive-effects)

## "use strict"

## Components

### Profiler

### StrictMode

### Suspense

#### lazy

## memo

## error boundaries

## createPortal
