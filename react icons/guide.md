To **find and use icons** from **React Icons** in your **React** project, the easiest way is to use their official website.

---

# 1️⃣ Search Icons

Go to the website:

**[https://react-icons.github.io/react-icons/](https://react-icons.github.io/react-icons/)**

There you can:

* 🔍 Search icons
* 📚 Browse libraries (Font Awesome, Material, etc.)
* 📋 Copy the import code

Example search: **github**

You will see something like:

```
FaGithub
```

---

# 2️⃣ Import the Icon

The site shows the exact code to use.

Example:

```javascript
import { FaGithub } from "react-icons/fa";
```

Then use it in JSX:

```jsx
function App() {
  return (
    <div>
      <FaGithub />
    </div>
  );
}
```

---

# 3️⃣ Library Prefix Guide

| Library         | Import path      |
| --------------- | ---------------- |
| Font Awesome    | `react-icons/fa` |
| Material Icons  | `react-icons/md` |
| Feather         | `react-icons/fi` |
| Bootstrap Icons | `react-icons/bs` |
| Heroicons       | `react-icons/hi` |
| Remix Icons     | `react-icons/ri` |

Example:

```javascript
import { MdHome } from "react-icons/md";
```

---

# 4️⃣ Style the Icons (works great with **Tailwind CSS**)

```jsx
<FaGithub className="text-4xl text-black hover:text-blue-500" />
```

or

```jsx
<FaGithub size={40} color="red" />
```

---

# 5️⃣ Real Example Component

```jsx
import { FaGithub } from "react-icons/fa";
import { MdEmail } from "react-icons/md";

function Contact() {
  return (
    <div className="flex gap-4 text-3xl">
      <FaGithub />
      <MdEmail />
    </div>
  );
}

export default Contact;
```

---

💡 **Pro Developer Trick:**
Most developers open **React Icons site**, search the icon, copy the **import**, and paste it directly into their component.

---

If you want, I can also show you:

* ⚡ **50 most used React icons for websites**
* ⚡ **Best icons for navbar / dashboard**
* ⚡ **How to animate icons using GSAP (you already installed it)**.
