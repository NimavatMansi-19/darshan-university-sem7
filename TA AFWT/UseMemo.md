# useMemo Hook

## What is useMemo?

`useMemo` is used to remember a calculated value.

React recalculates the value only when dependencies change.

---

## Syntax

```jsx
const memoizedValue = useMemo(() => {
  return calculation;
}, [dependencies]);
```

---

## Real-Life Analogy

Suppose you already solved:

```text
25 × 25 = 625
```

Would you solve it again and again?

No.

You remember the answer.

That is exactly what useMemo does.

---

## Example: Factorial Calculation

```jsx
import React, { useState, useMemo } from "react";

function App() {
  const [number, setNumber] = useState(5);

  const factorial = useMemo(() => {

    let result = 1;

    for (let i = 1; i <= number; i++) {
      result *= i;
    }

    return result;

  }, [number]);

  return (
    <>
      <h2>Factorial: {factorial}</h2>

      <button
        onClick={() => setNumber(number + 1)}
      >
        Increase
      </button>
    </>
  );
}

export default App;
```

---

## How It Works

```jsx
const factorial = useMemo(() => {
   ...
}, [number]);
```

If number changes:

```text
Recalculate factorial
```

If number does not change:

```text
Use old factorial value
```

---

## Daily Use Cases

### Filtering

```jsx
const filteredUsers = useMemo(() => {
  return users.filter(
    user => user.age > 18
  );
}, [users]);
```

### Sorting

```jsx
const sortedProducts = useMemo(() => {
  return products.sort();
}, [products]);
```

### Total Price

```jsx
const total = useMemo(() => {
  return cart.reduce(
    (sum, item) => sum + item.price,
    0
  );
}, [cart]);
```

---


## Memory Trick

useMemo = Save Calculation Result
