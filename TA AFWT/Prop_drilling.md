# ReactJS App (Property Drilling)

## 🔹 What is Property Drilling?
Property drilling means passing data from parent → child → grandchild using **props**.

---

## 🔹 Real Life Example
👵 Grandma → 👩 Mom → 👧 Child → 🧸 Toy  

- Grandma has **money (state)**.  
- Child needs it → Grandma gives it to Mom → Mom gives it to Child.  
- Finally, Child uses the money.  

Here, **money = state**, and **passing step by step = property drilling**.

---

# 📘 Understanding `useState` Hook in React

## 🔹 What is `useState`?

- In React, `useState` is like a **special storage box** inside your component.  
- It helps you **remember values** between re-renders.  
- When the value inside changes, React will **update the screen automatically**.  

👉 Think of it as a **whiteboard** where you can write something.  
Whenever you erase and write a new value, everyone looking at the whiteboard sees the update immediately.  

👉 When you use count++ instead of UseState hook,You are just changing the local variable count, not telling React that the value has changed.
React only updates the browser (UI) when you use the special function setCount().

---

## 🔹 Syntax

```javascript
const [value, setValue] = useState(initialValue);
```
value → current data stored (like what’s written on the whiteboard).

setValue → function to change/update the data.

initialValue → starting value when component first loads.

---
# ReactJS Multi-Component Example with Property Drilling  

## Problem Statement  
We need to create a React application with the following components:  

- `App` component should contain `A`.  
- `A` contains `B`.  
- `B` contains `C` and also has a **textbox + button** to update the state (`name`) from `App`.  
- `C` contains `D`.  
- `D` contains `E`.  
- `E` contains `F`.  
- `F` should **print the value of `name`** that comes from the `App` component.  

---

## Code (ReactJS)  

```jsx
// App.js
import React, { useState } from "react";
import A from "./A";

function App() {
  const [name, setName] = useState("Default Name");

  return (
    <div>
      <h1>App Component</h1>
      <A name={name} setName={setName} />
    </div>
  );
}

export default App;

// A.js
import React from "react";
import B from "./B";

function A({ name, setName }) {
  return (
    <div>
      <h2>Component A</h2>
      <B name={name} setName={setName} />
    </div>
  );
}

export default A;

// B.js
import React, { useState } from "react";
import C from "./C";

function B({ name, setName }) {
  const [inputValue, setInputValue] = useState("");

  const handleClick = () => {
    setName(inputValue); // update state in App
  };

  return (
    <div>
      <h3>Component B</h3>
      <input
        type="text"
        placeholder="Enter name"
        value={inputValue}
        onChange={(e) => setInputValue(e.target.value)}
      />
      <button onClick={handleClick}>Set Name</button>
      <C name={name} setName={setName} />
    </div>
  );
}

export default B;

// C.js
import React from "react";
import D from "./D";

function C({ name, setName }) {
  return (
    <div>
      <h4>Component C</h4>
      <D name={name} setName={setName} />
    </div>
  );
}

export default C;

// D.js
import React from "react";
import E from "./E";

function D({ name, setName }) {
  return (
    <div>
      <h5>Component D</h5>
      <E name={name} setName={setName} />
    </div>
  );
}

export default D;

// E.js
import React from "react";
import F from "./F";

function E({ name }) {
  return (
    <div>
      <h6>Component E</h6>
      <F name={name} />
    </div>
  );
}

export default E;

// F.js
import React from "react";

function F({ name }) {
  return (
    <div>
      <p>Component F → Name from App: <strong>{name}</strong></p>
    </div>
  );
}

export default F;
```

---

## How It Works (Flow Explanation)  

1. **App Component**:  
   - Holds the **state** (`name`) and the function (`setName`) to update it.  
   - Passes them down to `A`.  

2. **Component A → B → C → D → E → F**:  
   - Each component receives `name` (and sometimes `setName`) as **props** and passes them further down.  
   - This is called **property drilling** (props are drilled through intermediate components).  

3. **Component B**:  
   - Has a **textbox** to enter a new name.  
   - When the button is clicked, it calls `setName` (from `App`) → this updates the **App’s state**.  

4. **Component F**:  
   - Finally displays the updated **name** that started from `App`.  

---



## Summary  

- This is an example of **property drilling**.  
- The `App` manages the **state** (`name`).  
- Every component in the chain helps **pass the data down**.  
- `B` can also **update the state** because it has access to `setName`.  
- Finally, `F` shows the value.  

