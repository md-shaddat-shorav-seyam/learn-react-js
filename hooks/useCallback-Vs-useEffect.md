`useCallback` and `useEffect` are both React hooks, but they solve completely different problems.

---

# Core Difference

| Hook          | Purpose                                                  |
| ------------- | -------------------------------------------------------- |
| `useCallback` | Memoizes a function so it is not recreated unnecessarily |
| `useEffect`   | Runs side effects after rendering                        |

---

# 1. `useCallback`

## What it does

`useCallback` caches a function between renders.

Without it, React creates a new function every render.

---

## Syntax

```jsx
const memoizedFn = useCallback(() => {
   // function logic
}, [dependencies]);
```

---

# Example Without `useCallback`

```jsx
function App() {
  const handleClick = () => {
    console.log("clicked");
  };

  return <Button onClick={handleClick} />;
}
```

Every render creates a new `handleClick` function.

This can cause unnecessary re-renders in child components.

---

# Example With `useCallback`

```jsx
import { useCallback } from "react";

function App() {
  const handleClick = useCallback(() => {
    console.log("clicked");
  }, []);

  return <Button onClick={handleClick} />;
}
```

Now the function reference stays the same.

---

# Real Project Usage of `useCallback`

## 1. Prevent Child Re-render

Very common with `React.memo`.

```jsx
const Child = React.memo(({ onDelete }) => {
  console.log("Child rendered");

  return <button onClick={onDelete}>Delete</button>;
});
```

Parent:

```jsx
function Parent() {
  const [count, setCount] = useState(0);

  const handleDelete = useCallback(() => {
    console.log("Deleted");
  }, []);

  return (
    <>
      <button onClick={() => setCount(count + 1)}>
        Increment
      </button>

      <Child onDelete={handleDelete} />
    </>
  );
}
```

Without `useCallback`, child re-renders every time.

---

## 2. Stable Function in Dependency Array

```jsx
const fetchData = useCallback(() => {
   axios.get("/api/users");
}, []);
```

Useful when another hook depends on it.

---

## 3. Expensive Components

Large tables, charts, dashboards.

Example:

* Data grid
* Chat app
* Kanban board
* File explorer

---

# When NOT to Use `useCallback`

Do NOT wrap every function.

Bad practice:

```jsx
const hello = useCallback(() => {
   console.log("hello");
}, []);
```

`useCallback` itself has overhead.

Use it only when:

* Passing function to memoized child
* Function used in dependency array
* Performance issue exists

---

# 2. `useEffect`

## What it does

Runs side effects after rendering.

A side effect means:

* API call
* DOM manipulation
* Timer
* Subscription
* Local storage
* Event listener

---

# Syntax

```jsx
useEffect(() => {
   // side effect

   return () => {
      // cleanup
   };
}, [dependencies]);
```

---

# Real Project Usage of `useEffect`

---

## 1. Fetch API Data

```jsx
useEffect(() => {
   fetch("/api/users")
      .then(res => res.json())
      .then(data => setUsers(data));
}, []);
```

Runs once when component mounts.

---

## 2. Event Listener

```jsx
useEffect(() => {
   const handleResize = () => {
      console.log(window.innerWidth);
   };

   window.addEventListener("resize", handleResize);

   return () => {
      window.removeEventListener("resize", handleResize);
   };
}, []);
```

Cleanup prevents memory leaks.

---

## 3. Timer / Interval

```jsx
useEffect(() => {
   const id = setInterval(() => {
      console.log("running");
   }, 1000);

   return () => clearInterval(id);
}, []);
```

---

## 4. Sync With Local Storage

```jsx
useEffect(() => {
   localStorage.setItem("theme", theme);
}, [theme]);
```

---

## 5. Authentication Check

```jsx
useEffect(() => {
   checkUserLogin();
}, []);
```

Very common in dashboards/admin panels.

---

# Dependency Array Behavior

## Run Every Render

```jsx
useEffect(() => {
   console.log("render");
});
```

---

## Run Once

```jsx
useEffect(() => {
   console.log("mounted");
}, []);
```

---

## Run When Value Changes

```jsx
useEffect(() => {
   console.log("count changed");
}, [count]);
```

---

# Major Difference Visually

## `useCallback`

Caches FUNCTION.

```jsx
const fn = useCallback(() => {}, []);
```

---

## `useEffect`

RUNS CODE after render.

```jsx
useEffect(() => {}, []);
```

---

# Real Project Comparison

| Situation                 | useCallback | useEffect |
| ------------------------- | ----------- | --------- |
| Prevent child re-render   | ✅           | ❌         |
| API request               | ❌           | ✅         |
| Event listeners           | ❌           | ✅         |
| Memoize handler           | ✅           | ❌         |
| Timer                     | ❌           | ✅         |
| Stable callback reference | ✅           | ❌         |
| DOM side effects          | ❌           | ✅         |

---

# Important Connection Between Them

They are often used together.

Example:

```jsx
const fetchUsers = useCallback(async () => {
   const res = await fetch("/api/users");
   const data = await res.json();

   setUsers(data);
}, []);

useEffect(() => {
   fetchUsers();
}, [fetchUsers]);
```

Why?

Without `useCallback`, `fetchUsers` changes every render.

Then `useEffect` runs infinitely.

---

# Common Beginner Mistake

## Infinite Loop

```jsx
useEffect(() => {
   fetchData();
}, [fetchData]);
```

If `fetchData` is recreated every render → infinite loop.

Fix:

```jsx
const fetchData = useCallback(() => {
   ...
}, []);
```

---

# Mental Model

Think like this:

---

## `useEffect`

> “Run this code when something changes.”

---

## `useCallback`

> “Keep this function stable between renders.”

---

# In Modern Real Projects

## Frequently Used Together In:

* Admin dashboards
* Chat apps
* E-commerce sites
* Kanban apps
* Realtime apps
* Data visualization
* Infinite scroll apps

---

# Quick Interview Answer

> `useEffect` is used for handling side effects like API calls, subscriptions, and timers after rendering.
> `useCallback` is used to memoize functions and prevent unnecessary re-creations or re-renders.
