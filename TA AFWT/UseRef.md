# useRef Hook

## What is useRef?

`useRef` creates a reference that can store values and access DOM elements.

Changing a ref does NOT cause re-render.

---

## Syntax

```jsx
const myRef = useRef();
```

---

## Real-Life Analogy

Think of useRef as a pocket.

You can store things inside the pocket without changing your appearance.

Similarly:

```text
Store value
No re-render
```

---

# Example 1: Read Input Without Controlled Component

```jsx
import React, { useRef } from "react";

function App() {

  const inputRef = useRef();

  const showValue = () => {
    alert(inputRef.current.value);
  };

  return (
    <>
      <input
        type="text"
        ref={inputRef}
      />

      <button onClick={showValue}>
        Show Value
      </button>
    </>
  );
}

export default App;
```

---

## How It Works

Create Ref:

```jsx
const inputRef = useRef();
```

Attach Ref:

```jsx
<input ref={inputRef} />
```

Read Value:

```jsx
inputRef.current.value
```

---

# Example 2: Focus Input Field

```jsx
import React, { useRef } from "react";

function App() {

  const inputRef = useRef();

  const focusInput = () => {
    inputRef.current.focus();
  };

  return (
    <>
      <input
        type="text"
        ref={inputRef}
      />

      <button onClick={focusInput}>
        Focus Input
      </button>
    </>
  );
}

export default App;
```

---

## How It Works

```jsx
inputRef.current.focus();
```

Places cursor inside textbox.

---

## Daily Use Cases

### Focus Input

```jsx
inputRef.current.focus();
```

### Store Previous Value

```jsx
prevRef.current = value;
```

### Store Timer ID

```jsx
timerRef.current = setInterval(...);
```

### Access DOM

```jsx
divRef.current.style.color = "red";
```

---




### How do we focus input?

```jsx
inputRef.current.focus();
```

---

## Memory Trick

useRef = React's Pocket
