# React Custom Hooks and useDebugValue

## 1. What is a Custom Hook?

A Custom Hook is a JavaScript function whose name starts with **use** and allows us to reuse stateful logic between multiple React components.

### Why Custom Hooks?

Suppose multiple components need:

* Toggle functionality
* Authentication logic
* Online/Offline status checking
* Form validation

Instead of writing the same code repeatedly, we create a custom hook and reuse it.

### Benefits

* Code Reusability
* Better Readability
* Easier Maintenance
* Separation of Logic and UI

---

# 2. Rules for Creating Custom Hooks

### Rule 1

The hook name must start with **use**.

✅ Correct

```js
useToggle()
useTheme()
useOnlineStatus()
```

❌ Incorrect

```js
toggle()
theme()
onlineStatus()
```

---

### Rule 2

Hooks can only be called:

* Inside React Function Components
* Inside Other Custom Hooks

---

# 3. How to Build a Custom Hook (Step-by-Step)

Let's create a simple toggle hook.

## Folder Structure

```text
src
 ├── hooks
 │    └── useToggle.js
 ├── App.js
```

---

## Step 1: Create useToggle Hook

```js
// hooks/useToggle.js

import { useState } from "react";

function useToggle(initialValue = false) {
  const [flag, setFlag] = useState(initialValue);

  const toggle = () => {
    setFlag(!flag);
  };

  return [flag, toggle];
}

export default useToggle;
```

---

## Step 2: Use Hook in Component

```js
// App.js

import useToggle from "./hooks/useToggle";

function App() {
  const [isVisible, toggle] = useToggle(false);

  return (
    <div>
      <button onClick={toggle}>
        Toggle
      </button>

      {isVisible && <h2>Hello React!</h2>}
    </div>
  );
}

export default App;
```

---
# Simple Example of Custom Hook with useDebugValue

## What is useDebugValue?

`useDebugValue()` is used inside a Custom Hook to show a meaningful label in React DevTools.

It helps developers understand the current state of the custom hook while debugging.

### Syntax

```js
useDebugValue(value);
```

---

# Simple Example: useToggle Hook with useDebugValue

This is one of the easiest and most practical examples.

## Step 1: Create Custom Hook

### useToggle.js

```js
import { useState, useDebugValue } from "react";

function useToggle(initialValue = false) {
  const [flag, setFlag] = useState(initialValue);

  // Debug Value
  useDebugValue(flag ? "ON" : "OFF");

  const toggle = () => {
    setFlag(!flag);
  };

  return [flag, toggle];
}

export default useToggle;
```

---

## Step 2: Use Hook in App.js

```js
import useToggle from "./useToggle";

function App() {
  const [isVisible, toggle] = useToggle();

  return (
    <div>
      <button onClick={toggle}>
        Toggle
      </button>

      {isVisible && <h2>Hello React!</h2>}
    </div>
  );
}

export default App;
```

---

# Output

### Initially

```text
Button: Toggle
```

No text is shown.

---

### After Clicking Button

```text
Button: Toggle
Hello React!
```

---

# How useDebugValue Works Here

When the component loads:

```js
flag = false
```

```js
useDebugValue("OFF");
```

React DevTools shows:

```text
useToggle
  OFF
```

---

After clicking the button:

```js
flag = true
```

```js
useDebugValue("ON");
```

React DevTools shows:

```text
useToggle
  ON
```

---



You must remember what true/false means.


