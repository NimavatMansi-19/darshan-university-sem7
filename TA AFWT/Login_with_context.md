# 🚀 ReactJS Login App with `useContext`

This is a simple **ReactJS app** that demonstrates **login/logout functionality** using the **`useContext` hook**.  
It keeps the current user state available throughout the app.

---

## 📂 Project Structure
```
src/
├── App.js # Main App component, loads Login or Dashboard
├── index.js # Entry point of the React app
├── context/
│ └── UserContext.js # Context API file to store user state (login/logout)
└── components/
├── Login.js # Login form with username & password input
└── Dashboard.js # Dashboard shown after login with welcome + logout
```

---

## ⚡ Setup Instructions

1. **Create a new React app**
   ```bash
   npx create-react-app login-app
   cd login-app
## Code
### 📌 context/UserContext.js

```javascript
import { createContext, useState } from "react"

// Import after defining (to avoid circular imports)
import Login from "../components/Login";
import Dashboard from "../components/Dashboard";

// Create Context
export const UserContext = createContext();

export const UserContextWrapper = () => {
  const [user, setUser] = useState(null);

  const login = (username, password) => {
    if (username === "admin" && password === "1234") {
      setUser({ username });
      return true;
    }
    return false;
  };

  const logout = () => setUser(null);

  // Instead of children, decide what to render
  return (
    <UserContext.Provider value={{ user, login, logout }}>
      {user ? <Dashboard /> : <Login />}
    </UserContext.Provider>
  );
};


```
### 📌 components/Login.js

```javascript
import { useState, useContext } from "react";
import { UserContext } from "../context/UserContext";

export default function Login() {
  const { login } = useContext(UserContext);
  const [username, setUsername] = useState("");
  const [password, setPassword] = useState("");
  const [error, setError] = useState("");

  const handleLogin = () => {
    const success = login(username, password);
    if (!success) setError("Invalid username or password");
    else setError("");
  };

  return (
    <div className="p-4 flex flex-col gap-2">
      <h2 className="text-xl font-bold">Login</h2>
      <input
        type="text"
        placeholder="Username"
        value={username}
        onChange={(e) => setUsername(e.target.value)}
        className="border px-2 py-1 rounded"
      />
      <input
        type="password"
        placeholder="Password"
        value={password}
        onChange={(e) => setPassword(e.target.value)}
        className="border px-2 py-1 rounded"
      />
      <button
        onClick={handleLogin}
        className="px-3 py-1 bg-blue-500 text-white rounded"
      >
        Login
      </button>
      {error && <p className="text-red-500 text-sm">{error}</p>}
    </div>
  );
}

```

### 📌 components/Dashboard.js

```javascript
import { useContext } from "react";
import { UserContext } from "../context/UserContext";

export default function Dashboard() {
  const { user, logout } = useContext(UserContext);

  return (
    <div className="p-4">
      <h2 className="text-xl font-bold">Welcome, {user.username}!</h2>
      <button
        onClick={logout}
        className="px-3 py-1 bg-red-500 text-white rounded mt-2"
      >
        Logout
      </button>
    </div>
  );
}

```

### 📌 App.js

```javascript
import { useContext } from "react";
import { UserContext, UserContextWrapper } from "./context/UserContext";




export default function App() {
  return (
    <UserContextWrapper />
     
  );
}
```
