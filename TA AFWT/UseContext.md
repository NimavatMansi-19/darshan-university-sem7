# 🌟 React useContext Hook Example

The `useContext` hook in React allows you to **share state globally** across components without prop drilling.  
It works together with the `createContext` API.

---
# 🌐 What is Context in React?

**Context** in React provides a way to share values (data/state) across multiple components **without passing props manually** at every level (called **prop drilling**).

Think of it like a **global storage** for your app where you can keep values (like user info, theme, language, authentication state) and access them anywhere in your component tree.

---

## 📌 Why do we need Context?

👉 **Without Context:**  
You have to pass props from **parent → child → grandchild → great-grandchild** (very messy!).

👉 **With Context:**  
You can directly access the value in any component, no matter how deep it is in the component tree.


## 🛠 Steps to Use useContext

1. **Create a Context** using `createContext()`.
2. **Wrap your components** inside a `Context.Provider`.
3. **Consume the context** in child components using `useContext`.

---

## 📂 Project Structure

```
src/
├── App.js
├── index.js
└── context/
└── ThemeContext.js
```

---

## 📌 Example Code

### 1️⃣ Create Context (`ThemeContext.js`)
```javascript
import { createContext, useState } from "react";

// Create Context
export const ThemeContext = createContext();

// Wrapper Component
export const ThemeProvider = ({ children }) => {
  const [darkMode, setDarkMode] = useState(false);

  const toggleTheme = () => setDarkMode((prev) => !prev);

  return (
    <ThemeContext.Provider value={{ darkMode, toggleTheme }}>
      {children}
    </ThemeContext.Provider>
  );
};
```
### 📌 Explanation of `<MyContext.Provider>`

In React, a **Context Provider** is used to supply (provide) data to the components in its child tree.  

#### 🔎 What’s happening here?

##### 1. `MyContext.Provider`
- This is the **Provider** component created when you call `createContext()`.  
- It makes data available to all nested components inside it.  

##### 2. `value={{ theme: "dark" }}`
- The `value` prop is where you pass the data you want to share.  
- In this case, you’re sharing an object: `{ theme: "dark" }`.  

##### 3. `<App />` inside Provider
- All components inside `<App />` (and further nested children) can access `theme` without prop drilling.  
- Any component that uses `useContext(MyContext)` will get access to `{ theme: "dark" }`.  


## App.js
```javascript
import { useContext } from "react";
import { ThemeContext, ThemeProvider } from "./context/ThemeContext";

function AppContent() {
  const { darkMode, toggleTheme } = useContext(ThemeContext);

  return (
    <div
      className={`h-screen flex flex-col items-center justify-center ${
        darkMode ? "bg-black text-white" : "bg-gray-100 text-black"
      }`}
    >
      <h1 className="text-2xl font-bold mb-4">
        {darkMode ? "🌙 Dark Mode" : "☀️ Light Mode"}
      </h1>
      <button
        onClick={toggleTheme}
        className="px-4 py-2 bg-blue-500 text-white rounded hover:bg-blue-600"
      >
        Toggle Theme
      </button>
    </div>
  );
}

export default function App() {
  return (
    <ThemeProvider>
      <AppContent />
    </ThemeProvider>
  );
}
```

 
