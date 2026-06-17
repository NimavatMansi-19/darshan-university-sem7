#  Login Functionality with Prop Drilling Example

In this version, we pass `user`, `login`, and `logout` manually through props from `App → Login/Dashboard`.

---

## 📂 File Structure
```
src/
├── App.js
├── index.js
├── components/
│ ├── Login.js
│ └── Dashboard.js
```

---

## 📄 App.js

```jsx
import { useState } from "react";
import Login from "./components/Login";
import Dashboard from "./components/Dashboard";

export default function App() {
  const [user, setUser] = useState(null);

  const login = (username, password) => {
    if (username === "admin" && password === "1234") {
      setUser({ username });
      return true;
    }
    return false;
  };

  const logout = () => setUser(null);

  return (
    <div>
      {user ? (
        <Dashboard user={user} logout={logout} />
      ) : (
        <Login login={login} />
      )}
    </div>
  );
}
```

---

## 📄 components/Login.js

```jsx
import { useState } from "react";

function Login({login}) {
  const [username, setUsername] = useState("");
  const [password, setPassword] = useState("");
  const [error, setError] = useState("");

  const handleSubmit = (e) => {
    e.preventDefault();
    const success = login(username, password);
    if (!success) setError("Invalid Credentials ❌");
  };

  return (
    <div className="flex flex-col items-center justify-center h-screen bg-gray-100">
      <h1 className="text-2xl font-bold mb-4">Login</h1>
      <form onSubmit={handleSubmit} className="flex flex-col gap-2">
        <input
          type="text"
          placeholder="Username"
          className="border px-2 py-1 rounded"
          value={username}
          onChange={(e) => setUsername(e.target.value)}
        />
        <input
          type="password"
          placeholder="Password"
          className="border px-2 py-1 rounded"
          value={password}
          onChange={(e) => setPassword(e.target.value)}
        />
        <button
          type="submit"
          className="bg-blue-500 text-white px-4 py-2 rounded hover:bg-blue-600"
        >
          Login
        </button>
      </form>
      {error && <p className="text-red-500 mt-2">{error}</p>}
    </div>
  );
};

export default Login;
```
## 📄 components/Dashboard.js

```jsx
function Dashboard ({ user, logout })  {
  return (
    <div className="flex flex-col items-center justify-center h-screen bg-green-100">
      <h1 className="text-2xl font-bold mb-4">
        Welcome, {user.username} 🎉
      </h1>
      <button
        onClick={logout}
        className="bg-red-500 text-white px-4 py-2 rounded hover:bg-red-600"
      >
        Logout
      </button>
    </div>
  );
};

export default Dashboard;
```




