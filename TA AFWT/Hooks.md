
# ReactJS Hooks Demonstration

## 1. useState Hook

**Definition:**  
The `useState` hook allows us to add state (data that changes over time) to functional components. It returns an array with two values:  
1. The current state.  
2. A function to update that state.  

**Example:**

```jsx
import React, { useState } from "react";

export default function Counter() {
  // Declare a state variable "count" with initial value 0
  const [count, setCount] = useState(0);

  return (
    <div>
      <h2>Counter Example using useState</h2>
      <p>Current Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>Increase</button>
      <button onClick={() => setCount(count - 1)}>Decrease</button>
    </div>
  );
}
```

---

## 2. useEffect Hook

**Definition:**  
The `useEffect` hook is used to perform **side effects** in functional components such as fetching data, updating the DOM, or setting timers.  
It runs after the component renders. We can control when it runs using a dependency array.
### 🔹 Syntax
```jsx
useEffect(() => {
  // code to run (the side effect)
  
  return () => {
    // cleanup function (optional)
  };
}, [dependencies]);
```
-> Effect function → Runs after the component renders.

-> Dependencies array → Controls when the effect runs.

->Cleanup function → Runs before the component unmounts or before re-running the effect (useful for removing event listeners, stopping timers, etc.).
## 🔹 How it Works

#### 1.Without dependency array:
Runs after every render (initial + updates).

#### 2.With empty dependency array []:
Runs only once after the component mounts.

#### 3.With specific dependencies [count]:
Runs only when that dependency changes.
**Example:**

```jsx
import React, { useState, useEffect } from "react";

export default function Timer() {
  const [count, setCount] = useState(0);

  // useEffect runs after every render (when 'count' changes)
  useEffect(() => {
    console.log("Count updated:", count);
  }, [count]); // Runs only when 'count' changes

  return (
    <div>
      <h2>useEffect Example</h2>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>Click Me</button>
    </div>
  );
}
```

---

✅ With `useState`, we manage component state.  
✅ With `useEffect`, we handle side effects like updating the DOM or fetching data.  
