# React `useRef` Hook — Full Tutorial with Examples

The `useRef` hook in React is used to:

* Access DOM elements directly
* Store mutable values without re-rendering
* Keep previous values
* Work with timers, animations, external libraries, etc.

---

# Basic Syntax

```jsx
const refName = useRef(initialValue)
```

Example:

```jsx
const inputRef = useRef(null)
```

---

# 1. Access DOM Elements

Most common use case.

## Example: Focus Input Automatically

```jsx
import { useRef } from "react";

export default function App() {
  const inputRef = useRef(null);

  const focusInput = () => {
    inputRef.current.focus();
  };

  return (
    <div>
      <input ref={inputRef} type="text" placeholder="Enter name" />

      <button onClick={focusInput}>
        Focus Input
      </button>
    </div>
  );
}
```

---

## How It Works

```jsx
ref={inputRef}
```

React stores the actual DOM element inside:

```jsx
inputRef.current
```

So:

```jsx
inputRef.current.focus()
```

calls the native HTML input focus method.

---

# 2. Store Value Without Re-render

Unlike `useState`, changing `useRef.current` does NOT re-render the component.

---

## Example: Render Counter

```jsx
import { useRef, useState } from "react";

export default function App() {
  const countRef = useRef(0);
  const [state, setState] = useState(0);

  return (
    <div>
      <h2>Ref Count: {countRef.current}</h2>
      <h2>State Count: {state}</h2>

      <button
        onClick={() => {
          countRef.current++;
          console.log(countRef.current);
        }}
      >
        Increase Ref
      </button>

      <button
        onClick={() => setState(state + 1)}
      >
        Increase State
      </button>
    </div>
  );
}
```

---

## Difference

| useRef       | useState         |
| ------------ | ---------------- |
| No re-render | Causes re-render |
| Mutable      | Immutable update |
| Fast         | UI update        |
| Stores value | Updates UI       |

---

# 3. Store Previous Value

Very useful.

## Example

```jsx
import { useEffect, useRef, useState } from "react";

export default function App() {
  const [count, setCount] = useState(0);
  const prevCount = useRef();

  useEffect(() => {
    prevCount.current = count;
  }, [count]);

  return (
    <div>
      <h1>Current: {count}</h1>
      <h2>Previous: {prevCount.current}</h2>

      <button onClick={() => setCount(count + 1)}>
        Increase
      </button>
    </div>
  );
}
```

---

# 4. Timer / Interval Example

## Example

```jsx
import { useEffect, useRef, useState } from "react";

export default function App() {
  const [count, setCount] = useState(0);
  const intervalRef = useRef(null);

  const startTimer = () => {
    intervalRef.current = setInterval(() => {
      setCount((prev) => prev + 1);
    }, 1000);
  };

  const stopTimer = () => {
    clearInterval(intervalRef.current);
  };

  return (
    <div>
      <h1>{count}</h1>

      <button onClick={startTimer}>
        Start
      </button>

      <button onClick={stopTimer}>
        Stop
      </button>
    </div>
  );
}
```

---

# 5. Scroll To Element

## Example

```jsx
import { useRef } from "react";

export default function App() {
  const sectionRef = useRef(null);

  const scrollToSection = () => {
    sectionRef.current.scrollIntoView({
      behavior: "smooth",
    });
  };

  return (
    <div>
      <button onClick={scrollToSection}>
        Go Down
      </button>

      <div style={{ height: "100vh" }}></div>

      <div ref={sectionRef}>
        <h1>Hello Section</h1>
      </div>
    </div>
  );
}
```

---

# 6. Video Control Example

## Example

```jsx
import { useRef } from "react";

export default function App() {
  const videoRef = useRef(null);

  const playVideo = () => {
    videoRef.current.play();
  };

  const pauseVideo = () => {
    videoRef.current.pause();
  };

  return (
    <div>
      <video
        ref={videoRef}
        width="400"
        src="video.mp4"
      />

      <button onClick={playVideo}>
        Play
      </button>

      <button onClick={pauseVideo}>
        Pause
      </button>
    </div>
  );
}
```

---

# 7. `forwardRef` Example

When parent wants access to child DOM.

---

## Child Component

```jsx
import { forwardRef } from "react";

const CustomInput = forwardRef((props, ref) => {
  return <input ref={ref} {...props} />;
});

export default CustomInput;
```

---

## Parent Component

```jsx
import { useRef } from "react";
import CustomInput from "./CustomInput";

export default function App() {
  const inputRef = useRef(null);

  return (
    <div>
      <CustomInput ref={inputRef} />

      <button
        onClick={() => inputRef.current.focus()}
      >
        Focus
      </button>
    </div>
  );
}
```

---

# 8. `useRef` vs Normal Variable

❌ Wrong:

```jsx
let count = 0;
```

This resets every render.

✅ Correct:

```jsx
const countRef = useRef(0);
```

Persists between renders.

---

# 9. Important Rules

## ✅ Good Use Cases

* DOM access
* Timers
* Previous values
* Mutable storage
* Animations
* Third-party libraries
* Canvas
* Three.js objects

---

## ❌ Bad Use Cases

Do NOT use `useRef` for:

* UI state
* Form rendering
* Data that should update screen

Use `useState` instead.

---

# 10. Real World Example — Chat Auto Scroll

```jsx
import { useEffect, useRef, useState } from "react";

export default function Chat() {
  const [messages, setMessages] = useState([]);
  const bottomRef = useRef(null);

  useEffect(() => {
    bottomRef.current.scrollIntoView({
      behavior: "smooth",
    });
  }, [messages]);

  return (
    <div>
      <button
        onClick={() =>
          setMessages([
            ...messages,
            "New Message",
          ])
        }
      >
        Add Message
      </button>

      {messages.map((msg, index) => (
        <p key={index}>{msg}</p>
      ))}

      <div ref={bottomRef}></div>
    </div>
  );
}
```

---

# 11. `useRef` Internal Concept

`useRef` returns:

```jsx
{
  current: value
}
```

React keeps the SAME object between renders.

That is why value persists.

---

# 12. `createRef` vs `useRef`

| createRef                            | useRef               |
| ------------------------------------ | -------------------- |
| Used in class component              | Functional component |
| New ref every render                 | Same ref persists    |
| Less efficient in function component | Best for hooks       |

---

# 13. Advanced Example — Previous Search API

```jsx
import { useEffect, useRef, useState } from "react";

export default function Search() {
  const [query, setQuery] = useState("");
  const prevQuery = useRef("");

  useEffect(() => {
    prevQuery.current = query;
  }, [query]);

  return (
    <div>
      <input
        value={query}
        onChange={(e) => setQuery(e.target.value)}
      />

      <h3>Current: {query}</h3>
      <h3>Previous: {prevQuery.current}</h3>
    </div>
  );
}
```

---

# 14. Common Interview Questions

## Q1: Does `useRef` cause re-render?

No.

---

## Q2: Why use `useRef`?

To store mutable persistent values.

---

## Q3: Difference between `useRef` and `useState`?

`useState` updates UI.
`useRef` stores data silently.

---

## Q4: Can we manipulate DOM with `useRef`?

Yes.

---

# 15. Best Practices

✅ Use refs minimally
✅ Prefer declarative React
✅ Use state for UI
✅ Clean intervals/timeouts
✅ Use forwardRef when needed

---

# Final Summary

`useRef` is mainly for:

* DOM access
* Persistent mutable values
* Timers
* Previous values
* Animations
* External library integration

---

# Mini Practice Tasks

Try building:

1. Auto focus login form
2. Stopwatch
3. Auto scroll chat
4. Video player
5. Drag & drop system
6. Canvas drawing app
7. Three.js scene controller
