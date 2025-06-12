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

## Render and Commit

[docs](https://react.dev/learn/render-and-commit)

This section describes the steps for how a React component is rendered.

As described in the diagram, there are 3 steps: `Trigger`, `Render` and `Commit`.

![image](./react%20render.png)

### Trigger

A component will render for two reasons.

1. For the initial render
2. The component or its ancestors state has been updated.

### Render

After triggering a render, React has to figure out what to display. Rendering is just another word for calling your components.

React calls this recursively, so it will render components within components.

For the initial render, React will create DOM nodes for each HTML tag in your component. For a rerender, React calculates which properties have changed since the last render but won't do anything until it the `Commit` stage.

### Commit

Committing changes only applies to rerendering. For the initial render, React uses `appendChild()` to put the DOM nodes into the screen.

For rerenders, React just does the minimal necessary operations to make the DOM match the latest output.

### Browser paint

After React has done the commit to send the changes to the DOM. The browser itself will then do a repaint of the screen.

## Hooks

### useEffect

#### Synchronizing with effects

[docs](https://react.dev/learn/synchronizing-with-effects#how-to-handle-the-effect-firing-twice-in-development)

An effect runs to specify a side effect on rendering, not on a user event like clicking. This may be connecting to a server.

useEffect runs after the commit stage during the render loop. So it only runs after the paint.

Doing something like this:

```javascript
import { useState, useRef, useEffect } from "react";

function VideoPlayer({ src, isPlaying }) {
  const ref = useRef(null);

  if (isPlaying) {
    ref.current.play(); // Calling these while rendering isn't allowed.
  } else {
    ref.current.pause(); // Also, this crashes.
  }

  return <video ref={ref} src={src} loop playsInline />;
}

export default function App() {
  const [isPlaying, setIsPlaying] = useState(false);
  return (
    <>
      <button onClick={() => setIsPlaying(!isPlaying)}>
        {isPlaying ? "Pause" : "Play"}
      </button>
      <VideoPlayer
        isPlaying={isPlaying}
        src="https://interactive-examples.mdn.mozilla.net/media/cc0-videos/flower.mp4"
      />
    </>
  );
}
```

Is not allowed because it modifies the DOM during the render loop which is not allowed. React components must be pure.

Also, the DOM hasn't been created yet on the first render so it can't even call the video's play function.

##### Running on every render

useEffect runs on every render so this will become an infinite loop.

```javascript
const [count, setCount] = useState(0);
useEffect(() => {
  setCount(count + 1);
});
```

The useEffect will add to count, which triggers another render which then calls the useEffect again...

To fix this, set the dependencies parameter.

```javascript
useEffect(() => {
  if (isPlaying) {
    // It's used here...
    // ...
  } else {
    // ...
  }
}, [isPlaying]); // ...so it must be declared here!
```

Now this only calls when isPlaying is changed. It does a check using `Object.is`. So if the objects are defined in two different places but have the same values then it is still difference.

i.e.

```javascript
let obj = {};
Object.is(obj, {}); // false
```

##### useEffect Cleanup

If you need to connect to a service on component mount, then when the component unmounts, you'll need to disconnect otherwise when you come back, you'll have created two connections.

The fix for this is to use the cleanup function when you return from your effect.

```javascript
useEffect(() => {
  const connection = createConnection();
  connection.connect();
  return () => {
    connection.disconnect();
  };
}, []);
```

In development, this will call the createConnection twice. i.e. Connecting, Disconnected, Connecting.

It is a dev feature to allow you to see whether a component had fully unmounted and cleaned up all the side effects.

#### Lifecycle of Reactive Effects

[docs](https://react.dev/learn/lifecycle-of-reactive-effects)

Effects should be thought as separate and independent as processes. In this example we have an effect that creates a function and then runs a cleanup when the roomId changes.

```javascript
function ChatRoom({ roomId }) {
  useEffect(() => {
    logVisit(roomId); // avoid this!
    const connection = createConnection(serverUrl, roomId);
    connection.connect();
    return () => {
      connection.disconnect();
    };
  }, [roomId]);
  // ...
}
```

The issue here is with the logVisit when roomId changes. While this may look "cleaner", it is less maintainable because later you may want the connection code may depend on another dependency (**reactive values**) which means you'll now have an unnecessary log.

It is better to separate this into another useEffect.

```javascript
function ChatRoom({ roomId }) {
  useEffect(() => {
    logVisit(roomId);
  }, [roomId]);

  useEffect(() => {
    const connection = createConnection(serverUrl, roomId);
    // ...
  }, [roomId]);
  // ...
}
```

When the dependencies are empty, it means that it will only synchronise when the component mounts and run cleanup when the component unmounts.

##### All variables declared in the component body are reactive

When you declare variables based on other reactive values, then they too become reactive and need to be added to the effect's dependencies.

```javascript
function ChatRoom({ roomId, selectedServerUrl }) {
  // roomId is reactive
  const settings = useContext(SettingsContext); // settings is reactive
  const serverUrl = selectedServerUrl ?? settings.defaultServerUrl; // serverUrl is reactive
  useEffect(() => {
    const connection = createConnection(serverUrl, roomId); // Your Effect reads roomId and serverUrl
    connection.connect();
    return () => {
      connection.disconnect();
    };
  }, [roomId, serverUrl]); // So it needs to re-synchronize when either of them changes!
  // ...
}
```

Otherwise you'll get a linting error.

If you don't want to synchronise to some values, you should either move their declaration into the useEffect block, split them into separate effects, or separate events from your effects.

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

[docs](https://react.dev/reference/react/useLayoutEffect)

This is a useEffect that is called before the browser repaints the screen but after rendering [Link to Render and Commit](React.md#render-and-commit). This allows the JSX to be a pure calculation but also adds in a side effect to the component.

It blocks the screen from repainting which can hurt performance.

Use cases for this would be getting the size of a component/HTML element before it has been painted on the screen.

```javascript
function Tooltip() {
  const ref = useRef(null);
  const [tooltipHeight, setTooltipHeight] = useState(0); // You don't know real height yet

  useLayoutEffect(() => {
    const { height } = ref.current.getBoundingClientRect();
    setTooltipHeight(height); // Re-render now that you know the real height
  }, []);

  // ...use tooltipHeight in the rendering logic below...
}
```

In the example, it will retrigger the effect before the screen is painted. This pauses the paint and the useLayoutEffect's changes will run.

Because the browser usually batches the paints to wait for all the JavaScript microtask to run, the useLayoutEffect will be batched with the initial commit JavaScript code, before the screen is painted. So only one paint occurs which avoids a flicker if you used `useEffect`.

#### Rendering Timeline

- Runs DOM updates
- Immediately runs useLayoutEffect
- Optionally does another render
- Then allows the browser to proceed with layout and paint

### useMemo

[docs](https://react.dev/reference/react/useMemo)

This is an optimisation that caches your calculations between re-renders.

```javascript
import { useMemo } from "react";

function TodoList({ todos, tab }) {
  const visibleTodos = useMemo(() => filterTodos(todos, tab), [todos, tab]);
  // ...
}
```

This means that filterTodos will not run again unless the dependencies have changed. It is good for big calculations. To properly see when to use `useMemo`, you can use `console.time()` to see how your component works in practice. The docs also recommends that you throttle your CPU by setting it to a mid-tier or low-end mobile as the average user probably doesn't have 32gb of ram on their machine.

```javascript
console.time("filter array");
const visibleTodos = filterTodos(todos, tab);
console.timeEnd("filter array");
```

### useOptimistic

[docs](https://react.dev/reference/react/useOptimistic)

```javascript
const [optimisticTodos, addOptimisticTodo] = useOptimistic(todos);

async function handleAddTodo(newTodo) {
  addOptimisticTodo([...todos, newTodo]); // optimistic UI update

  try {
    await api.addTodo(newTodo);
  } catch (err) {
    // error handling goes here
    console.error("Failed to add todo:", err);
    // You can refetch data or roll back state manually if needed
  }
}
```

This hooks lets you show the optimistic/happy path when state is changed. It is really used for mostly form handling. When a form is saved, you'd want the new saved values to show instantly, instead of waiting for the request to complete before showing the new values.

If the request fails, then you wouldn't want the new values to be displayed so you'll have to manually revert back.

<!-- This seems very much like useState -->

In terms of practical use, they seem very similar. However, useOptimistic is mostly used to tell the developer that the real state is not handled in the component but actually in a server which is why useOptimistic is used a lot in Server Components. This can be written with useState as such.

```javascript
const [todos, setTodos] = useState([]);

async function handleAddTodo(newTodo) {
  const optimisticTodos = [...todos, newTodo];
  setTodos(optimisticTodos); // optimistic update

  try {
    await api.addTodo(newTodo);
  } catch (err) {
    setTodos(todos); // manually rollback
  }
}
```

The key difference is that it will automatically merge and remove duplicates from the list when the server responds.

## Escape Hatches

### Refs

[docs](https://react.dev/learn/escape-hatches)
Refs are a good way to remember something in your component but not have it trigger a new render. Because it doesn't trigger a new render, it is not tracked by React and is an `escape hatch`.

`import { useRef } from 'react'`

Then you call the useRef hook.

`const ref = useRef(0); // default value`
Your value will be saved in ref.current.

In this example, we are showing the number of times you've clicked on the button, saving that value in ref.current and displaying it in an alert.

```javascript
import { useRef } from "react";

export default function Counter() {
  let ref = useRef(0);

  function handleClick() {
    ref.current = ref.current + 1;
    alert("You clicked " + ref.current + " times!");
  }

  return <button onClick={handleClick}>Click me!</button>;
}
```

However, because the component doesn't rerender on ref change. Something like this will never be updated.

```javascript
import { useRef } from "react";

export default function Counter() {
  let countRef = useRef(0);

  function handleClick() {
    // This doesn't re-render the component!
    countRef.current = countRef.current + 1;
  }

  return (
    <button onClick={handleClick}>You clicked {countRef.current} times</button>
  );
}
```

## "use strict"

## Components

### Profiler

### StrictMode

### Suspense

#### lazy

## memo

## error boundaries

## createPortal

## React Devtools Profiler

### Profiling

### Flamegraph Chart
