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

# Real Projects Where `useRef` Is Actually Used

In real-world React apps, `useRef` is heavily used for:

* animations
* media control
* canvas
* WebSocket storage
* form focus
* performance optimization
* third-party libraries
* infinite scroll
* drag/drop
* Three.js
* GSAP

---

# 1. OTP Input System

Very common in authentication apps.

## Features

* Auto move to next input
* Backspace to previous input
* Auto focus

---

## Example

```jsx id="yv3fxz"
import { useRef } from "react";

export default function OTP() {
  const inputs = useRef([]);

  const handleChange = (e, index) => {
    if (e.target.value && index < 5) {
      inputs.current[index + 1].focus();
    }
  };

  const handleKeyDown = (e, index) => {
    if (e.key === "Backspace" && !e.target.value && index > 0) {
      inputs.current[index - 1].focus();
    }
  };

  return (
    <div style={{ display: "flex", gap: 10 }}>
      {[...Array(6)].map((_, index) => (
        <input
          key={index}
          maxLength={1}
          ref={(el) => (inputs.current[index] = el)}
          onChange={(e) => handleChange(e, index)}
          onKeyDown={(e) => handleKeyDown(e, index)}
          style={{
            width: 40,
            height: 40,
            textAlign: "center",
            fontSize: 20,
          }}
        />
      ))}
    </div>
  );
}
```

---

# 2. Infinite Scroll Feed

Used in:

* Facebook
* Instagram
* Twitter/X
* Reddit

---

## Example

```jsx id="3gx34d"
import { useEffect, useRef, useState } from "react";

export default function Feed() {
  const loaderRef = useRef(null);
  const [items, setItems] = useState([1, 2, 3]);

  useEffect(() => {
    const observer = new IntersectionObserver((entries) => {
      if (entries[0].isIntersecting) {
        setItems((prev) => [...prev, prev.length + 1]);
      }
    });

    observer.observe(loaderRef.current);

    return () => observer.disconnect();
  }, []);

  return (
    <div>
      {items.map((item) => (
        <h2 key={item}>Post {item}</h2>
      ))}

      <div ref={loaderRef}>
        Loading...
      </div>
    </div>
  );
}
```

---

# 3. Custom Video Player

Real streaming apps use this.

---

## Example

```jsx id="k4y8a4"
import { useRef, useState } from "react";

export default function VideoPlayer() {
  const videoRef = useRef(null);
  const [playing, setPlaying] = useState(false);

  const togglePlay = () => {
    if (playing) {
      videoRef.current.pause();
    } else {
      videoRef.current.play();
    }

    setPlaying(!playing);
  };

  return (
    <div>
      <video
        ref={videoRef}
        width="500"
        src="movie.mp4"
      />

      <button onClick={togglePlay}>
        {playing ? "Pause" : "Play"}
      </button>
    </div>
  );
}
```

---

# 4. Drag & Drop Kanban Board

Used in:

* Trello
* Jira
* Notion

---

## Example

```jsx id="lxjv0w"
import { useRef } from "react";

export default function DragBox() {
  const dragItem = useRef();

  const handleDragStart = (index) => {
    dragItem.current = index;
  };

  const handleDrop = (index) => {
    console.log(
      "Dragged:",
      dragItem.current,
      "Dropped:",
      index
    );
  };

  return (
    <div>
      {[1, 2, 3].map((item, index) => (
        <div
          key={item}
          draggable
          onDragStart={() => handleDragStart(index)}
          onDrop={() => handleDrop(index)}
          onDragOver={(e) => e.preventDefault()}
          style={{
            padding: 20,
            margin: 10,
            border: "1px solid black",
          }}
        >
          Item {item}
        </div>
      ))}
    </div>
  );
}
```

---

# 5. Chat Application Auto Scroll

Used in:

* WhatsApp
* Messenger
* Discord

---

## Example

```jsx id="qvx6fz"
import { useEffect, useRef, useState } from "react";

export default function Chat() {
  const bottomRef = useRef(null);

  const [messages, setMessages] = useState([
    "Hello",
  ]);

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
        Send
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

# 6. Canvas Drawing App

Used in:

* Paint apps
* Whiteboards
* Signature pads

---

## Example

```jsx id="6ch1qo"
import { useEffect, useRef } from "react";

export default function CanvasApp() {
  const canvasRef = useRef(null);

  useEffect(() => {
    const canvas = canvasRef.current;
    const ctx = canvas.getContext("2d");

    ctx.fillStyle = "red";
    ctx.fillRect(50, 50, 100, 100);
  }, []);

  return (
    <canvas
      ref={canvasRef}
      width={500}
      height={500}
      style={{ border: "1px solid black" }}
    />
  );
}
```

---

# 7. GSAP Animation

Very common with GSAP.

---

## Example

```jsx id="4yoe8k"
import { useEffect, useRef } from "react";
import gsap from "gsap";

export default function Box() {
  const boxRef = useRef(null);

  useEffect(() => {
    gsap.to(boxRef.current, {
      x: 300,
      rotation: 360,
      duration: 2,
    });
  }, []);

  return (
    <div
      ref={boxRef}
      style={{
        width: 100,
        height: 100,
        background: "tomato",
      }}
    />
  );
}
```

---

# 8. Three.js Integration

Very important in 3D apps.

Uses:

* scene
* renderer
* camera
* mesh

---

## Example

```jsx id="k5n8ew"
import { useEffect, useRef } from "react";
import * as THREE from "three";

export default function ThreeScene() {
  const mountRef = useRef(null);

  useEffect(() => {
    const scene = new THREE.Scene();

    const camera = new THREE.PerspectiveCamera(
      75,
      window.innerWidth / window.innerHeight,
      0.1,
      1000
    );

    const renderer = new THREE.WebGLRenderer();

    renderer.setSize(
      window.innerWidth,
      window.innerHeight
    );

    mountRef.current.appendChild(renderer.domElement);

    const geometry =
      new THREE.BoxGeometry();

    const material =
      new THREE.MeshBasicMaterial({
        color: "red",
      });

    const cube = new THREE.Mesh(
      geometry,
      material
    );

    scene.add(cube);

    camera.position.z = 5;

    const animate = () => {
      requestAnimationFrame(animate);

      cube.rotation.x += 0.01;
      cube.rotation.y += 0.01;

      renderer.render(scene, camera);
    };

    animate();
  }, []);

  return <div ref={mountRef}></div>;
}
```

---

# 9. Debounce Search System

Used in:

* search bars
* API optimization

---

## Example

```jsx id="jlwmg7"
import { useRef, useState } from "react";

export default function Search() {
  const timeoutRef = useRef(null);

  const [query, setQuery] = useState("");

  const handleSearch = (value) => {
    setQuery(value);

    clearTimeout(timeoutRef.current);

    timeoutRef.current = setTimeout(() => {
      console.log("API CALL:", value);
    }, 500);
  };

  return (
    <input
      value={query}
      onChange={(e) =>
        handleSearch(e.target.value)
      }
    />
  );
}
```

---

# 10. WebSocket Connection Storage

Real-time apps use this.

---

## Example

```jsx id="84cznd"
import { useEffect, useRef } from "react";

export default function SocketApp() {
  const socketRef = useRef(null);

  useEffect(() => {
    socketRef.current = new WebSocket(
      "wss://echo.websocket.org"
    );

    socketRef.current.onmessage = (msg) => {
      console.log(msg.data);
    };

    return () => {
      socketRef.current.close();
    };
  }, []);

  return <h1>Socket Connected</h1>;
}
```

---

# 11. Stopwatch App

Classic `useRef` interview project.

---

## Example

```jsx id="bzms74"
import { useRef, useState } from "react";

export default function Stopwatch() {
  const intervalRef = useRef(null);

  const [time, setTime] = useState(0);

  const start = () => {
    intervalRef.current = setInterval(() => {
      setTime((prev) => prev + 1);
    }, 1000);
  };

  const stop = () => {
    clearInterval(intervalRef.current);
  };

  return (
    <div>
      <h1>{time}</h1>

      <button onClick={start}>Start</button>
      <button onClick={stop}>Stop</button>
    </div>
  );
}
```

---

# 12. File Upload Preview

Used everywhere.

---

## Example

```jsx id="b1fgjj"
import { useRef, useState } from "react";

export default function Upload() {
  const inputRef = useRef(null);

  const [image, setImage] = useState(null);

  const openFilePicker = () => {
    inputRef.current.click();
  };

  return (
    <div>
      <input
        ref={inputRef}
        type="file"
        hidden
        onChange={(e) => {
          setImage(
            URL.createObjectURL(
              e.target.files[0]
            )
          );
        }}
      />

      <button onClick={openFilePicker}>
        Upload
      </button>

      {image && (
        <img src={image} width={200} />
      )}
    </div>
  );
}
```

---

# Real Production Areas Using `useRef`

| Industry      | Usage            |
| ------------- | ---------------- |
| Fintech       | OTP input        |
| Streaming     | Video controls   |
| Social Media  | Infinite scroll  |
| SaaS          | Drag-drop boards |
| AI Apps       | Chat scroll      |
| Design Tools  | Canvas           |
| Games         | Animation loops  |
| 3D Websites   | Three.js         |
| Analytics     | Debounce         |
| Realtime Apps | WebSockets       |

---

# Most Important Real-World Pattern

## Store Expensive Objects

```jsx id="n3nsd4"
const socketRef = useRef()
const audioRef = useRef()
const playerRef = useRef()
const animationRef = useRef()
```

This avoids recreation on every render.

---

# When Senior Developers Use `useRef`

Usually for:

* performance
* DOM manipulation
* external libraries
* persistent mutable data
* avoiding unnecessary renders

Not for normal UI state.

3. Auto scroll chat
4. Video player
5. Drag & drop system
6. Canvas drawing app
7. Three.js scene controller
