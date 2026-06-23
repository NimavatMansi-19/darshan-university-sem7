# useCallback Hook

## What is useCallback?

`useCallback` is used to remember a function between renders.

Without `useCallback`, React creates a new function every time the parent component re-renders.

With `useCallback`, React reuses the same function until dependencies change.

---

## Syntax

```jsx
const memoizedFunction = useCallback(() => {
  // code
}, [dependencies]);
```

---

## Real-Life Analogy

Suppose your teacher asks for the same attendance sheet every day.

Without useCallback:

* You create a new sheet every day.

With useCallback:

* You keep using the same sheet.

---

## Example: Passing Function to Child Component

### Parent Component

```jsx
import React, { useCallback } from "react";
import Child from "./Child";

function App() {

  const showMessage = useCallback(() => {
    console.log("Hello from Parent");
  }, []);

  return (
    <>
      <Child onButtonClick={showMessage} />
    </>
  );
}

export default App;
```

### Child Component

```jsx
import React from "react";

function Child({ onButtonClick }) {
  return (
    <button onClick={onButtonClick}>
      Click Me
    </button>
  );
}

export default React.memo(Child);
```

---


