# Local and Global State Management in React

## Aim

Understand the difference between **Local State Management** and **Global State Management** in React using practical examples.

---

# What is State?

State is data that can change over time.

Whenever the state changes, React automatically updates the UI.

Example:

- Username
- Email
- Password
- Counter
- Theme
- Shopping Cart

---

# Local State Management

## Definition

Local state is the state that belongs to **only one component**.

It cannot be directly accessed by other components.

React provides the **useState()** hook to manage local state.

### Syntax

```jsx
const [state, setState] = useState(initialValue);
```

### Example

```jsx
const [count, setCount] = useState(0);
```

Here,

- `count` → Current value
- `setCount()` → Updates the value

---

## When to use Local State?

Use local state when the data is required only inside one component.

Examples

- Login Form
- Registration Form
- Counter
- Search Box
- Toggle Button

---

## Local State Flow

```text
User Input
     │
     ▼
useState()
     │
     ▼
State Updated
     │
     ▼
React Re-renders UI
```

---

# Practical Example (Local State)

## Question

**Build a login form using useState to store email and password. Display entered data on submit.**

### App.jsx

```jsx
import React, { useState } from "react";

function App() {

  const [email, setEmail] = useState("");
  const [password, setPassword] = useState("");

  const handleSubmit = () => {
    alert("Login Successful");

    console.log("Email :", email);
    console.log("Password :", password);
  };

  return (
    <div>

      <h2>Login Form</h2>

      <input
        type="email"
        placeholder="Enter Email"
        value={email}
        onChange={(e) => setEmail(e.target.value)}
      />

      <br /><br />

      <input
        type="password"
        placeholder="Enter Password"
        value={password}
        onChange={(e) => setPassword(e.target.value)}
      />

      <br /><br />

      <button onClick={handleSubmit}>
        Login
      </button>

      <h3>Entered Data</h3>

      <p>Email : {email}</p>
      <p>Password : {password}</p>

    </div>
  );
}

export default App;
```


# Global State Management

## Definition

Global state is shared among **multiple components**.

Instead of creating separate state inside every component, one common state is created and shared.

React provides the **Context API** for this purpose.

---

## Why Global State?

Imagine three components.

```text
Navbar

Profile

Dashboard
```

All need the logged-in user's name.

Using local state,

each component would have separate state.

Instead,

create one global state and share it.

---

## When to use Global State?

Examples

- Logged-in User
- Theme (Dark/Light)
- Language
- Shopping Cart
- Authentication

---

## Global State Flow

```text
Context Provider
        │
        ▼
Stores Global State
        │
        ▼
Child Components
        │
        ▼
Read Same Data
```

---

# Practical Example (Global State)

## Question

**Create a UserContext that stores Name and Email. Display data in multiple components.**

---

## Folder Structure

```text
src
│
├── App.jsx
├── UserContext.jsx
├── Profile.jsx
└── Dashboard.jsx
```

---

## Step 1 : Create Context

### UserContext.jsx

```jsx
import { createContext } from "react";

export const UserContext = createContext();

function UserProvider({ children }) {

  const user = {
    name: "Mansi",
    email: "mansi@gmail.com"
  };

  return (
    <UserContext.Provider value={user}>
      {children}
    </UserContext.Provider>
  );
}

export default UserProvider;
```

---

## Step 2 : Profile Component

```jsx
import { useContext } from "react";
import { UserContext } from "./UserContext";

function Profile() {

  const user = useContext(UserContext);

  return (
    <div>
      <h3>Profile</h3>
      <p>Name : {user.name}</p>
      <p>Email : {user.email}</p>
    </div>
  );
}

export default Profile;
```

---

## Step 3 : Dashboard Component

```jsx
import { useContext } from "react";
import { UserContext } from "./UserContext";

function Dashboard() {

  const user = useContext(UserContext);

  return (
    <div>
      <h3>Dashboard</h3>
      <p>Welcome {user.name}</p>
    </div>
  );
}

export default Dashboard;
```

---

## Step 4 : App.jsx

```jsx
import UserProvider from "./UserContext";
import Profile from "./Profile";
import Dashboard from "./Dashboard";

function App() {

  return (
    <UserProvider>

      <Profile />

      <Dashboard />

    </UserProvider>
  );
}

export default App;
```

---

# Explanation

### Step 1

Create Context

```jsx
createContext()
```

Creates a global state container.

---

### Step 2

Provider

```jsx
<UserContext.Provider value={user}>
```

Shares data with every child component.

---

### Step 3

Consume Context

```jsx
const user = useContext(UserContext);
```

Gets the shared data.

---

### Step 4

Display Data

Profile Component

```text
Name : Mansi

Email : mansi@gmail.com
```

Dashboard Component

```text
Welcome Mansi
```

Both components use the same data.

---

# Local State vs Global State

| Local State | Global State |
|-------------|--------------|
| Managed using `useState` | Managed using Context API |
| Used inside one component | Shared among multiple components |
| Cannot be accessed directly by other components | Can be accessed by any child component |
| Best for forms, counters, inputs | Best for user data, theme, cart, authentication |

---

# Summary

## Local State

- Managed using `useState`
- Used within a single component
- Example: Login Form

## Global State

- Managed using Context API
- Shared across multiple components
- Example: UserContext storing Name and Email

---

# Viva Questions

### What is Local State?

Local state belongs to a single component and is managed using `useState()`.

---

### What is Global State?

Global state is shared among multiple components using Context API.

---

### Which hook is used for Local State?

`useState()`

---

### Which hook is used to access Context?

`useContext()`

---

### What is Context API?

Context API is a React feature used to share data among multiple components without prop drilling.

---

### Give one example of Local State.

Login Form

Counter

Search Box

---

### Give one example of Global State.

Logged-in User

Theme

Shopping Cart

Authentication
