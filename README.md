# learn-react-js
Here’s a **start-to-end React.js guide** with code, concepts, and a **real project use case**.

---

# React.js: Start to End

## 1. What is React?

React is a **JavaScript library for building user interfaces**, especially **single-page applications**.

With React, you build UI using **components**.

Example:

* Navbar
* Sidebar
* Product card
* Login form
* Dashboard

Each of these can be a separate component.

---

# 2. Why React?

React is popular because it gives:

* **Reusable components**
* **Fast UI updates**
* **State management**
* **Clean code structure**
* **Huge ecosystem**

---

# 3. Prerequisites

Before learning React, you should know:

* HTML
* CSS
* JavaScript basics
* ES6+ features:

  * `let`, `const`
  * arrow functions
  * template literals
  * destructuring
  * modules
  * array methods (`map`, `filter`, `find`)
  * promises
  * async/await

---

# 4. React Environment Setup

The easiest modern way is **Vite**.

## Create project

```bash
npm create vite@latest my-react-app
```

Choose:

* Framework: `React`
* Variant: `JavaScript`

Then:

```bash
cd my-react-app
npm install
npm run dev
```

---

# 5. Project Structure

A simple React project looks like this:

```bash
my-react-app/
│── public/
│── src/
│   │── assets/
│   │── components/
│   │── App.jsx
│   │── main.jsx
│   │── index.css
│── package.json
```

---

# 6. Entry Files

## `main.jsx`

```jsx
import React from "react";
import ReactDOM from "react-dom/client";
import App from "./App";
import "./index.css";

ReactDOM.createRoot(document.getElementById("root")).render(
  <React.StrictMode>
    <App />
  </React.StrictMode>
);
```

## `App.jsx`

```jsx
function App() {
  return (
    <div>
      <h1>Hello React</h1>
    </div>
  );
}

export default App;
```

---

# 7. JSX

JSX means **JavaScript XML**.
It lets you write HTML-like syntax inside JavaScript.

Example:

```jsx
function App() {
  return <h1>Welcome to React</h1>;
}
```

### Rules of JSX

* Return one parent element
* Use `className` instead of `class`
* Use `{}` to write JavaScript inside JSX

Example:

```jsx
function App() {
  const name = "Seyam";

  return (
    <div className="box">
      <h1>Hello, {name}</h1>
    </div>
  );
}
```

---

# 8. Components

A component is a reusable piece of UI.

## Functional component

```jsx
function Greeting() {
  return <h2>Hello from Greeting component</h2>;
}

export default Greeting;
```

Use it:

```jsx
import Greeting from "./Greeting";

function App() {
  return (
    <div>
      <Greeting />
      <Greeting />
    </div>
  );
}
```

---

# 9. Props

Props are used to pass data from parent to child.

## Example

```jsx
function UserCard(props) {
  return (
    <div>
      <h2>{props.name}</h2>
      <p>Age: {props.age}</p>
    </div>
  );
}

export default UserCard;
```

Use it:

```jsx
import UserCard from "./UserCard";

function App() {
  return (
    <div>
      <UserCard name="Seyam" age={18} />
      <UserCard name="Rahim" age={20} />
    </div>
  );
}
```

## Destructuring props

```jsx
function UserCard({ name, age }) {
  return (
    <div>
      <h2>{name}</h2>
      <p>Age: {age}</p>
    </div>
  );
}
```

---

# 10. Events in React

React events are written in camelCase.

```jsx
function App() {
  function handleClick() {
    alert("Button clicked");
  }

  return <button onClick={handleClick}>Click Me</button>;
}
```

Inline:

```jsx
<button onClick={() => alert("Hello")}>Say Hello</button>
```

---

# 11. State with `useState`

State stores changing data in a component.

```jsx
import { useState } from "react";

function Counter() {
  const [count, setCount] = useState(0);

  return (
    <div>
      <h2>Count: {count}</h2>
      <button onClick={() => setCount(count + 1)}>Increment</button>
    </div>
  );
}

export default Counter;
```

### Explanation

* `count` = current value
* `setCount` = function to update value
* `useState(0)` = initial value is 0

---

# 12. Multiple State Values

```jsx
import { useState } from "react";

function Form() {
  const [name, setName] = useState("");
  const [email, setEmail] = useState("");

  return (
    <div>
      <input
        type="text"
        placeholder="Enter name"
        value={name}
        onChange={(e) => setName(e.target.value)}
      />

      <input
        type="email"
        placeholder="Enter email"
        value={email}
        onChange={(e) => setEmail(e.target.value)}
      />

      <h3>{name}</h3>
      <h3>{email}</h3>
    </div>
  );
}
```

---

# 13. Rendering Lists

Use `map()`.

```jsx
function App() {
  const users = ["Seyam", "Rahim", "Karim"];

  return (
    <ul>
      {users.map((user, index) => (
        <li key={index}>{user}</li>
      ))}
    </ul>
  );
}
```

For objects:

```jsx
function App() {
  const users = [
    { id: 1, name: "Seyam" },
    { id: 2, name: "Rahim" },
  ];

  return (
    <div>
      {users.map((user) => (
        <h2 key={user.id}>{user.name}</h2>
      ))}
    </div>
  );
}
```

---

# 14. Conditional Rendering

## Using `if`

```jsx
function App() {
  const isLoggedIn = true;

  if (isLoggedIn) {
    return <h1>Welcome back</h1>;
  }

  return <h1>Please log in</h1>;
}
```

## Using ternary operator

```jsx
function App() {
  const isLoggedIn = false;

  return <h1>{isLoggedIn ? "Dashboard" : "Login Page"}</h1>;
}
```

## Using `&&`

```jsx
function App() {
  const show = true;

  return <div>{show && <p>This is visible</p>}</div>;
}
```

---

# 15. Forms in React

Controlled form example:

```jsx
import { useState } from "react";

function LoginForm() {
  const [form, setForm] = useState({
    email: "",
    password: "",
  });

  function handleChange(e) {
    const { name, value } = e.target;

    setForm({
      ...form,
      [name]: value,
    });
  }

  function handleSubmit(e) {
    e.preventDefault();
    console.log(form);
  }

  return (
    <form onSubmit={handleSubmit}>
      <input
        type="email"
        name="email"
        placeholder="Email"
        value={form.email}
        onChange={handleChange}
      />

      <input
        type="password"
        name="password"
        placeholder="Password"
        value={form.password}
        onChange={handleChange}
      />

      <button type="submit">Login</button>
    </form>
  );
}

export default LoginForm;
```

---

# 16. `useEffect`

`useEffect` runs side effects:

* API call
* timer
* localStorage
* event listeners

## Basic example

```jsx
import { useEffect } from "react";

function App() {
  useEffect(() => {
    console.log("Component mounted");
  }, []);

  return <h1>Hello</h1>;
}
```

## Example with dependency

```jsx
import { useEffect, useState } from "react";

function App() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    console.log("Count changed:", count);
  }, [count]);

  return (
    <div>
      <h2>{count}</h2>
      <button onClick={() => setCount(count + 1)}>Add</button>
    </div>
  );
}
```

---

# 17. Fetching API Data

```jsx
import { useEffect, useState } from "react";

function Users() {
  const [users, setUsers] = useState([]);
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    async function fetchUsers() {
      try {
        const res = await fetch("https://jsonplaceholder.typicode.com/users");
        const data = await res.json();
        setUsers(data);
      } catch (error) {
        console.log("Error:", error);
      } finally {
        setLoading(false);
      }
    }

    fetchUsers();
  }, []);

  if (loading) return <h2>Loading...</h2>;

  return (
    <div>
      <h1>User List</h1>
      {users.map((user) => (
        <div key={user.id}>
          <h3>{user.name}</h3>
          <p>{user.email}</p>
        </div>
      ))}
    </div>
  );
}

export default Users;
```

---

# 18. Component Styling

## 1) External CSS

```jsx
function App() {
  return <h1 className="title">Hello</h1>;
}
```

```css
.title {
  color: blue;
  font-size: 40px;
}
```

## 2) Inline CSS

```jsx
function App() {
  return <h1 style={{ color: "red", fontSize: "40px" }}>Hello</h1>;
}
```

## 3) CSS Module

`App.module.css`

```css
.title {
  color: green;
}
```

`App.jsx`

```jsx
import styles from "./App.module.css";

function App() {
  return <h1 className={styles.title}>Hello</h1>;
}
```

---

# 19. Handling Input Search Example

```jsx
import { useState } from "react";

function SearchBox() {
  const [text, setText] = useState("");

  return (
    <div>
      <input
        type="text"
        placeholder="Search here"
        value={text}
        onChange={(e) => setText(e.target.value)}
      />
      <p>You typed: {text}</p>
    </div>
  );
}
```

---

# 20. Reusable Components

## Button component

```jsx
function Button({ text, onClick }) {
  return <button onClick={onClick}>{text}</button>;
}

export default Button;
```

Use it:

```jsx
import Button from "./Button";

function App() {
  return (
    <div>
      <Button text="Save" onClick={() => alert("Saved")} />
      <Button text="Delete" onClick={() => alert("Deleted")} />
    </div>
  );
}
```

---

# 21. Lifting State Up

When two child components need same data, move state to parent.

```jsx
import { useState } from "react";

function Input({ value, onChange }) {
  return <input value={value} onChange={onChange} />;
}

function Display({ value }) {
  return <h2>{value}</h2>;
}

function App() {
  const [text, setText] = useState("");

  return (
    <div>
      <Input value={text} onChange={(e) => setText(e.target.value)} />
      <Display value={text} />
    </div>
  );
}

export default App;
```

---

# 22. `useRef`

Used to access DOM elements directly or store values without rerender.

```jsx
import { useRef } from "react";

function App() {
  const inputRef = useRef();

  function focusInput() {
    inputRef.current.focus();
  }

  return (
    <div>
      <input ref={inputRef} type="text" />
      <button onClick={focusInput}>Focus</button>
    </div>
  );
}
```

---

# 23. `useMemo`

Optimizes expensive calculations.

```jsx
import { useMemo, useState } from "react";

function App() {
  const [count, setCount] = useState(0);
  const [dark, setDark] = useState(false);

  const doubleNumber = useMemo(() => {
    console.log("Calculating...");
    return count * 2;
  }, [count]);

  return (
    <div>
      <h2>{doubleNumber}</h2>
      <button onClick={() => setCount(count + 1)}>Increment</button>
      <button onClick={() => setDark(!dark)}>Toggle</button>
    </div>
  );
}
```

---

# 24. `useCallback`

Used to memoize functions.

```jsx
import { useCallback, useState } from "react";

function App() {
  const [count, setCount] = useState(0);

  const handleClick = useCallback(() => {
    console.log("Clicked");
  }, []);

  return (
    <div>
      <h2>{count}</h2>
      <button onClick={() => setCount(count + 1)}>Add</button>
      <button onClick={handleClick}>Log</button>
    </div>
  );
}
```

---

# 25. React Router

Install:

```bash
npm install react-router-dom
```

## Basic Routing

```jsx
import { BrowserRouter, Routes, Route, Link } from "react-router-dom";

function Home() {
  return <h2>Home Page</h2>;
}

function About() {
  return <h2>About Page</h2>;
}

function Contact() {
  return <h2>Contact Page</h2>;
}

function App() {
  return (
    <BrowserRouter>
      <nav>
        <Link to="/">Home</Link> | <Link to="/about">About</Link> |{" "}
        <Link to="/contact">Contact</Link>
      </nav>

      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/about" element={<About />} />
        <Route path="/contact" element={<Contact />} />
      </Routes>
    </BrowserRouter>
  );
}

export default App;
```

---

# 26. Context API

Used for global state.

## Create context

```jsx
import { createContext, useContext, useState } from "react";

const ThemeContext = createContext();

function ThemeProvider({ children }) {
  const [theme, setTheme] = useState("light");

  return (
    <ThemeContext.Provider value={{ theme, setTheme }}>
      {children}
    </ThemeContext.Provider>
  );
}

function Navbar() {
  const { theme, setTheme } = useContext(ThemeContext);

  return (
    <div>
      <h2>Current theme: {theme}</h2>
      <button onClick={() => setTheme(theme === "light" ? "dark" : "light")}>
        Toggle Theme
      </button>
    </div>
  );
}

function App() {
  return (
    <ThemeProvider>
      <Navbar />
    </ThemeProvider>
  );
}

export default App;
```

---

# 27. Custom Hook

A custom hook is a reusable function using React hooks.

```jsx
import { useEffect, useState } from "react";

function useFetch(url) {
  const [data, setData] = useState([]);
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    async function getData() {
      const res = await fetch(url);
      const result = await res.json();
      setData(result);
      setLoading(false);
    }

    getData();
  }, [url]);

  return { data, loading };
}

export default useFetch;
```

Use it:

```jsx
import useFetch from "./useFetch";

function App() {
  const { data, loading } = useFetch(
    "https://jsonplaceholder.typicode.com/posts"
  );

  if (loading) return <h2>Loading...</h2>;

  return (
    <div>
      {data.slice(0, 5).map((post) => (
        <h3 key={post.id}>{post.title}</h3>
      ))}
    </div>
  );
}
```

---

# 28. Forms Validation Example

```jsx
import { useState } from "react";

function SignupForm() {
  const [email, setEmail] = useState("");
  const [error, setError] = useState("");

  function handleSubmit(e) {
    e.preventDefault();

    if (!email.includes("@")) {
      setError("Invalid email");
      return;
    }

    setError("");
    alert("Form submitted");
  }

  return (
    <form onSubmit={handleSubmit}>
      <input
        type="text"
        value={email}
        onChange={(e) => setEmail(e.target.value)}
        placeholder="Enter email"
      />
      <button type="submit">Submit</button>
      {error && <p>{error}</p>}
    </form>
  );
}
```

---

# 29. React Folder Structure for Bigger Apps

```bash
src/
│── components/
│   │── Navbar.jsx
│   │── ProductCard.jsx
│── pages/
│   │── Home.jsx
│   │── Products.jsx
│   │── Cart.jsx
│── hooks/
│   │── useFetch.js
│── context/
│   │── CartContext.jsx
│── services/
│   │── api.js
│── App.jsx
│── main.jsx
```

---

# 30. Real Project Use Case: E-commerce Product App

Now let’s build a small real project with:

* Navbar
* Product list
* Add to cart
* Search filter
* Cart count

---

## Step 1: `main.jsx`

```jsx
import React from "react";
import ReactDOM from "react-dom/client";
import App from "./App";
import "./index.css";

ReactDOM.createRoot(document.getElementById("root")).render(
  <React.StrictMode>
    <App />
  </React.StrictMode>
);
```

---

## Step 2: `App.jsx`

```jsx
import { useState } from "react";
import Navbar from "./components/Navbar";
import ProductList from "./components/ProductList";
import Cart from "./components/Cart";

function App() {
  const [cart, setCart] = useState([]);
  const [search, setSearch] = useState("");

  const products = [
    { id: 1, name: "Laptop", price: 700 },
    { id: 2, name: "Phone", price: 300 },
    { id: 3, name: "Headphone", price: 50 },
    { id: 4, name: "Keyboard", price: 40 },
  ];

  function addToCart(product) {
    setCart([...cart, product]);
  }

  const filteredProducts = products.filter((product) =>
    product.name.toLowerCase().includes(search.toLowerCase())
  );

  return (
    <div>
      <Navbar cartCount={cart.length} />
      <div style={{ padding: "20px" }}>
        <input
          type="text"
          placeholder="Search product..."
          value={search}
          onChange={(e) => setSearch(e.target.value)}
        />

        <ProductList products={filteredProducts} addToCart={addToCart} />
        <Cart cart={cart} />
      </div>
    </div>
  );
}

export default App;
```

---

## Step 3: `components/Navbar.jsx`

```jsx
function Navbar({ cartCount }) {
  return (
    <nav
      style={{
        display: "flex",
        justifyContent: "space-between",
        padding: "15px",
        background: "#222",
        color: "#fff",
      }}
    >
      <h2>My Shop</h2>
      <h3>Cart: {cartCount}</h3>
    </nav>
  );
}

export default Navbar;
```

---

## Step 4: `components/ProductList.jsx`

```jsx
import ProductCard from "./ProductCard";

function ProductList({ products, addToCart }) {
  return (
    <div>
      <h2>Products</h2>
      {products.length === 0 ? (
        <p>No products found</p>
      ) : (
        products.map((product) => (
          <ProductCard
            key={product.id}
            product={product}
            addToCart={addToCart}
          />
        ))
      )}
    </div>
  );
}

export default ProductList;
```

---

## Step 5: `components/ProductCard.jsx`

```jsx
function ProductCard({ product, addToCart }) {
  return (
    <div
      style={{
        border: "1px solid #ccc",
        margin: "10px 0",
        padding: "10px",
        borderRadius: "8px",
      }}
    >
      <h3>{product.name}</h3>
      <p>Price: ${product.price}</p>
      <button onClick={() => addToCart(product)}>Add to Cart</button>
    </div>
  );
}

export default ProductCard;
```

---

## Step 6: `components/Cart.jsx`

```jsx
function Cart({ cart }) {
  const total = cart.reduce((sum, item) => sum + item.price, 0);

  return (
    <div style={{ marginTop: "30px" }}>
      <h2>Cart Items</h2>
      {cart.length === 0 ? (
        <p>Cart is empty</p>
      ) : (
        <>
          {cart.map((item, index) => (
            <div key={index}>
              <p>
                {item.name} - ${item.price}
              </p>
            </div>
          ))}
          <h3>Total: ${total}</h3>
        </>
      )}
    </div>
  );
}

export default Cart;
```

---

## Step 7: `index.css`

```css
body {
  font-family: Arial, sans-serif;
  margin: 0;
  padding: 0;
}

input {
  padding: 10px;
  margin-bottom: 20px;
  width: 250px;
}

button {
  padding: 8px 12px;
  cursor: pointer;
}
```

---

# 31. What This Project Teaches

This small project teaches:

* components
* props
* state
* events
* form input
* search filtering
* list rendering
* conditional rendering
* data flow from parent to child

---

# 32. Another Real Project Ideas

After this, build these:

## Beginner

* Counter app
* To-do app
* Weather app
* Quiz app
* BMI calculator
* Expense tracker

## Intermediate

* Blog app
* Movie search app
* Notes app with localStorage
* Admin dashboard
* Authentication UI

## Advanced

* Full e-commerce app
* Chat app
* Social media dashboard
* LMS/student portal
* Job portal

---

# 33. Important React Concepts You Must Master

Learn these properly:

* JSX
* Components
* Props
* State
* Event handling
* Conditional rendering
* List rendering
* Forms
* Hooks
* API calls
* Routing
* Context API
* Reusable components
* Project structure

---

# 34. Common Beginner Mistakes

## 1. Forgetting `key` in list

Wrong:

```jsx
{items.map(item => <p>{item}</p>)}
```

Correct:

```jsx
{items.map((item, index) => <p key={index}>{item}</p>)}
```

## 2. Updating state incorrectly

Wrong:

```jsx
setCount(count++);
```

Correct:

```jsx
setCount(count + 1);
```

Or safer:

```jsx
setCount((prev) => prev + 1);
```

## 3. Not using `e.preventDefault()` in forms

```jsx
function handleSubmit(e) {
  e.preventDefault();
}
```

## 4. Mutating state directly

Wrong:

```jsx
cart.push(product);
setCart(cart);
```

Correct:

```jsx
setCart([...cart, product]);
```

---

# 35. React Interview Questions

Here are common ones:

1. What is React?
2. What is JSX?
3. Difference between props and state?
4. What is virtual DOM?
5. What is `useState`?
6. What is `useEffect`?
7. What are controlled components?
8. What is Context API?
9. Why use keys in lists?
10. What is component lifecycle?
11. Difference between functional and class components?
12. What is lifting state up?
13. What is prop drilling?
14. What is React Router?
15. What are custom hooks?

---

# 36. Best Learning Path

Follow this order:

## Phase 1

* JSX
* Components
* Props
* Events
* State

## Phase 2

* Forms
* Lists
* Conditional rendering
* `useEffect`
* API fetching

## Phase 3

* Routing
* Context API
* Custom hooks
* LocalStorage
* Reusable architecture

## Phase 4

* Real projects
* Authentication
* Backend connection
* Deployment

---

# 37. Example: To-do App Mini Practice

```jsx
import { useState } from "react";

function App() {
  const [task, setTask] = useState("");
  const [tasks, setTasks] = useState([]);

  function addTask() {
    if (task.trim() === "") return;

    setTasks([...tasks, task]);
    setTask("");
  }

  function deleteTask(indexToDelete) {
    setTasks(tasks.filter((_, index) => index !== indexToDelete));
  }

  return (
    <div style={{ padding: "20px" }}>
      <h1>To-Do App</h1>

      <input
        type="text"
        value={task}
        onChange={(e) => setTask(e.target.value)}
        placeholder="Enter task"
      />
      <button onClick={addTask}>Add</button>

      <ul>
        {tasks.map((t, index) => (
          <li key={index}>
            {t} <button onClick={() => deleteTask(index)}>Delete</button>
          </li>
        ))}
      </ul>
    </div>
  );
}

export default App;
```

---

# 38. How React Is Used in Real Projects

In real companies, React is used for:

* e-commerce frontend
* admin dashboard
* banking panels
* learning platforms
* chat interfaces
* CMS dashboards
* booking systems

Real project flow usually:

* React frontend
* API backend
* authentication
* database
* deployment

Example stack:

* React
* Node.js / Express
* MongoDB / PostgreSQL
* Firebase / Supabase
* Vercel / Netlify

---

# 39. Advanced Topics After Basics

After mastering basics, move to:

* React Router deeply
* Context API deeply
* Redux / Zustand
* Form libraries
* React Query / TanStack Query
* Next.js
* Performance optimization
* Lazy loading
* Code splitting
* Protected routes
* Authentication
* Testing

---

# 40. Final Full Revision Summary

React core learning summary:

* React builds UI with components
* JSX lets you write HTML-like syntax
* Props pass data
* State stores dynamic data
* Hooks add features like state and effects
* Router handles page navigation
* Context handles shared data
* Real apps combine all of them

---

# 41. 30-Day React Study Plan

## Week 1

* React setup
* JSX
* components
* props
* events
* `useState`

## Week 2

* forms
* list rendering
* conditional rendering
* `useEffect`
* API fetching

## Week 3

* router
* context API
* localStorage
* reusable components

## Week 4

* build 2–3 projects
* improve structure
* deploy project
* practice interview questions

---

# 42. What You Should Build Next

Build in this order:

1. Counter app
2. To-do app
3. Weather app
4. Product cart app
5. Blog app
6. Auth dashboard

---

I can also make you a **complete React handwritten notes style guide**, or a **React project-based roadmap with 10 projects from beginner to advanced**.
