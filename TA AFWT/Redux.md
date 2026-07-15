# Lab 22: Redux State Management in React

## Table of Contents

1. [What is State?](#what-is-state)
2. [The Problem Redux Solves](#the-problem-redux-solves)
3. [What is Redux?](#what-is-redux)
4. [Core Concepts of Redux](#core-concepts-of-redux)
5. [Redux vs useContext + useReducer](#redux-vs-usecontext--usereducer)
6. [Redux Toolkit (RTK) — The Modern Way](#redux-toolkit-rtk--the-modern-way)
7. [Setting Up Redux in a React Project](#setting-up-redux-in-a-react-project)
8. [Understanding Each Piece](#understanding-each-piece)
9. [Program A — Theme Switcher](#program-a--theme-switcher)
10. [Program B — User Authentication](#program-b--user-authentication)
11. [Program C — Shopping Cart](#program-c--shopping-cart)
12. [How to Run](#how-to-run)
13. [Complete Data Flow Walkthrough](#complete-data-flow-walkthrough)
14. [File Structure](#file-structure)
15. [Summary](#summary)

---

## What is State?

Think of **state** as the memory of your application. Just like you remember things in your daily life (your name, what you had for breakfast, whether the lights are on or off), a React application remembers things through state.

For example:
- Is the user logged in? → That is **state**
- What items are in the shopping cart? → That is **state**
- Is the theme set to dark or light? → That is **state**

In React, we usually manage state using `useState` inside a component. This works perfectly for simple cases where the data belongs to only one component.

```jsx
const [count, setCount] = useState(0);
```

But what happens when multiple components need access to the same piece of state?

---

## The Problem Redux Solves

Imagine this situation in your app:

```
        App
       /   \
    Header   Main
      |       |
   CartIcon  ProductList
                |
             ProductCard
```

If `ProductCard` adds an item to the cart, and `CartIcon` needs to show the updated count, how do they share data?

**Without Redux**, you would have to:
1. Lift the state up to the `App` component
2. Pass it down through `Header → CartIcon` as props
3. Pass the update function down through `Main → ProductList → ProductCard` as props

This is called **prop drilling** — and it gets messy very fast as your app grows.

**With Redux**, every component can directly access and modify the shared state from a central place called the **store**, no matter how deeply nested the component is.

```
Before Redux:                    After Redux:
App (holds state)                Redux Store (holds state)
  ↓ props ↓ props                  ↕         ↕
Header    Main                   CartIcon  ProductCard
  ↓ props   ↓ props              (reads)   (reads & writes)
CartIcon  ProductList
            ↓ props
          ProductCard
```

---

## What is Redux?

**Redux** is a state management library. It provides a **single, centralized store** where all your application's shared state lives. Any component in your app can:

- **Read** from the store (get data)
- **Dispatch actions** to the store (request changes)

Redux follows three fundamental principles:

### Principle 1: Single Source of Truth

All shared state is stored in ONE object (the store). There are no duplicate copies of data scattered across components.

### Principle 2: State is Read-Only

You cannot directly modify the state. The only way to change state is by **dispatching an action** — a plain JavaScript object that describes what happened.

```js
{ type: "theme/toggleTheme" }
{ type: "cart/addToCart", payload: { id: 1, name: "Headphones", price: 2499 } }
```

### Principle 3: Changes are Made with Pure Functions

State changes happen inside **reducers** — pure functions that take the current state and an action, and return the new state.

```js
function themeReducer(state, action) {
  if (action.type === "theme/toggleTheme") {
    return { mode: state.mode === "light" ? "dark" : "light" };
  }
  return state;
}
```

---

## Core Concepts of Redux

Let us understand the four core building blocks of Redux with a real-world analogy.

### Analogy: A Bank

| Redux Concept | Bank Analogy |
|---------------|-------------|
| **Store** | The bank vault (holds all the money/data) |
| **Action** | A withdrawal/deposit slip (describes what you want to do) |
| **Reducer** | The bank teller (processes your slip and updates the vault) |
| **Dispatch** | Submitting your slip to the teller (triggering the change) |

### 1. Store

The store is a JavaScript object that holds the entire state of your application.

```js
{
  theme: { mode: "light" },
  auth: { isLoggedIn: false, user: null },
  cart: { items: [], totalQuantity: 0, totalPrice: 0 }
}
```

### 2. Action

An action is a plain object with a `type` field that describes what happened. It can optionally carry data in a `payload` field.

```js
{ type: "auth/login", payload: { email: "admin@test.com", password: "password123" } }
{ type: "cart/addToCart", payload: { id: 1, name: "Mouse", price: 1499 } }
{ type: "theme/toggleTheme" }
```

### 3. Reducer

A reducer is a function that receives the current state and an action, then returns a NEW state based on the action type. It never modifies the existing state directly.

```
(currentState, action) → newState
```

### 4. Dispatch

Dispatch is how you send an action to the store. When you call `dispatch(action)`, Redux sends that action to the reducer, which updates the state.

### The Redux Data Flow

```
User clicks a button
       ↓
Component calls dispatch(action)
       ↓
Redux sends the action to the reducer
       ↓
Reducer calculates and returns new state
       ↓
Store updates with the new state
       ↓
All subscribed components re-render with new data
```

---

## Redux vs useContext + useReducer

You might wonder: React already has `useContext` and `useReducer`. Why use Redux?

| Feature | useContext + useReducer | Redux Toolkit |
|---------|----------------------|---------------|
| Setup complexity | Simple | Slightly more setup |
| Performance | Re-renders all consumers on any state change | Only re-renders when selected state changes |
| DevTools | No built-in devtools | Powerful Redux DevTools for time-travel debugging |
| Middleware | Not built-in | Built-in support for middleware (async logic, logging) |
| Scalability | Good for small apps | Designed for medium to large apps |
| Community | Part of React | Massive ecosystem and community |

**Use useContext** when you have simple shared state (like theme or locale).

**Use Redux** when your app has complex state logic, many state slices, or when you need devtools and middleware.

---

## Redux Toolkit (RTK) — The Modern Way

In the early days, Redux required a LOT of boilerplate code — action type constants, action creators, switch-case reducers, immutable update logic. It was powerful but verbose.

**Redux Toolkit** (RTK) is the official, recommended way to write Redux today. It simplifies everything:

| Old Redux | Redux Toolkit |
|-----------|--------------|
| Manually create action type strings | Auto-generated from slice name |
| Write action creator functions by hand | Auto-generated by `createSlice` |
| Use switch-case in reducers | Write simple reducer functions |
| Must return new state objects (immutability) | Can "mutate" state directly (uses Immer internally) |
| Configure store manually with middleware | `configureStore` handles everything |

### What is a Slice?

A **slice** is a collection of Redux reducer logic and actions for a single feature. Think of it as one "section" of your store.

For example, your app might have:
- A `theme` slice (manages light/dark mode)
- An `auth` slice (manages login/logout)
- A `cart` slice (manages shopping cart items)

Each slice has:
- A **name** (used as a prefix for action types)
- An **initial state** (the default values)
- **Reducers** (functions that handle state changes)

---

## Setting Up Redux in a React Project

### Step 1: Install the Packages

```bash
npm install @reduxjs/toolkit react-redux
```

- `@reduxjs/toolkit` — The core Redux library with modern utilities
- `react-redux` — The binding library that connects Redux to React components

### Step 2: Create a Slice

A slice file defines the state, reducers, and auto-generated actions for one feature.

```js
import { createSlice } from "@reduxjs/toolkit";

const themeSlice = createSlice({
  name: "theme",
  initialState: {
    mode: "light",
  },
  reducers: {
    toggleTheme: (state) => {
      state.mode = state.mode === "light" ? "dark" : "light";
    },
  },
});

export const { toggleTheme } = themeSlice.actions;
export default themeSlice.reducer;
```

What happens here:
- `createSlice` creates the slice with a name, initial state, and reducer functions
- `state.mode = "dark"` looks like mutation, but Redux Toolkit uses **Immer** under the hood, which safely creates a new immutable state
- `themeSlice.actions` automatically contains action creators like `toggleTheme()`
- `themeSlice.reducer` is the combined reducer function we export

### Step 3: Create the Store

The store combines all slices into a single state tree.

```js
import { configureStore } from "@reduxjs/toolkit";
import themeReducer from "./themeSlice";

const store = configureStore({
  reducer: {
    theme: themeReducer,
  },
});

export default store;
```

The store now looks like:
```js
{
  theme: { mode: "light" }
}
```

### Step 4: Provide the Store to React

Wrap your app (or part of it) with the `Provider` component from `react-redux`. This makes the store available to all child components.

```jsx
import { Provider } from "react-redux";
import store from "./store/store";

function App() {
  return (
    <Provider store={store}>
      <MyComponent />
    </Provider>
  );
}
```

### Step 5: Use Redux State in Components

Use two hooks from `react-redux`:

- `useSelector` — Reads data from the store
- `useDispatch` — Gets the dispatch function to trigger actions

```jsx
import { useSelector, useDispatch } from "react-redux";
import { toggleTheme } from "./store/themeSlice";

function MyComponent() {
  const theme = useSelector((state) => state.theme.mode);
  const dispatch = useDispatch();

  return (
    <button onClick={() => dispatch(toggleTheme())}>
      Current: {theme}
    </button>
  );
}
```

---

## Understanding Each Piece

### `createSlice`

```js
createSlice({
  name: "sliceName",
  initialState: { ... },
  reducers: {
    actionName: (state, action) => { ... }
  }
})
```

- `name` → Used as a prefix for action types (e.g., `"theme/toggleTheme"`)
- `initialState` → The default state when the app starts
- `reducers` → An object where each key is an action name and the value is the reducer function

### `configureStore`

```js
configureStore({
  reducer: {
    sliceName: sliceReducer,
  }
})
```

- Takes an object of slice reducers
- Automatically sets up Redux DevTools
- Automatically adds useful middleware (like serialization checks)

### `useSelector(selectorFunction)`

```js
const value = useSelector((state) => state.theme.mode);
```

- Takes a function that receives the entire Redux state
- Returns the specific piece of state you need
- The component re-renders only when this selected value changes

### `useDispatch()`

```js
const dispatch = useDispatch();
dispatch(toggleTheme());
```

- Returns the store's dispatch function
- Use it to send actions to the store
- The action creator function (like `toggleTheme()`) returns an action object like `{ type: "theme/toggleTheme" }`

### `<Provider store={store}>`

- A React component that makes the Redux store available to all nested components
- Must wrap any component that needs access to Redux state
- Usually placed at the top level of your app

---

## Program A — Theme Switcher

**Goal**: Use Redux to manage a light/dark theme across the application.

### How It Works

1. The `themeSlice` stores the current theme mode (`"light"` or `"dark"`)
2. Three reducer actions are available: `toggleTheme`, `setLight`, `setDark`
3. The `ThemeSwitcher` component reads the theme with `useSelector` and dispatches changes with `useDispatch`
4. All styles dynamically change based on the Redux theme state

### Files

- `store/themeSlice.js` — Defines the theme state and reducers
- `ThemeSwitcher.jsx` — The UI component

### State Shape

```js
{
  theme: {
    mode: "light"    // or "dark"
  }
}
```

### Actions Available

| Action | What It Does |
|--------|-------------|
| `toggleTheme()` | Switches between light and dark |
| `setLight()` | Forces theme to light |
| `setDark()` | Forces theme to dark |

---

## Program B — User Authentication

**Goal**: Implement login/logout functionality with Redux managing the authentication state.

### How It Works

1. The `authSlice` stores whether the user is logged in, user info, and any error messages
2. When the user submits the login form, it dispatches the `login` action with email and password
3. The reducer checks the credentials (hardcoded for demo: `admin@test.com` / `password123`)
4. If credentials match, state updates to logged in with user info
5. If credentials do not match, an error message is stored in state
6. The component conditionally renders either the login form or the welcome screen

### Files

- `store/authSlice.js` — Defines auth state and reducers
- `UserAuth.jsx` — The UI component with login form and dashboard

### State Shape

```js
{
  auth: {
    isLoggedIn: false,
    user: null,          // or { email: "...", name: "..." }
    error: null          // or "Invalid email or password"
  }
}
```

### Actions Available

| Action | What It Does |
|--------|-------------|
| `login({ email, password })` | Validates credentials and logs in |
| `logout()` | Clears user data and sets isLoggedIn to false |
| `clearError()` | Removes the error message |

---

## Program C — Shopping Cart

**Goal**: Build a full shopping cart with add, remove, increase, decrease, and clear functionality using Redux.

### How It Works

1. The `cartSlice` manages an array of cart items plus total quantity and total price
2. A list of products is displayed with "Add to Cart" buttons
3. Clicking "Add to Cart" dispatches `addToCart` — if the item already exists, its quantity increases
4. The cart section shows all added items with +/− buttons and a remove button
5. Total price and quantity are tracked in real-time in the Redux state
6. A "Clear Cart" button dispatches `clearCart` to reset everything

### Files

- `store/cartSlice.js` — Defines cart state and reducers
- `CartApp.jsx` — The UI component with products and cart

### State Shape

```js
{
  cart: {
    items: [
      {
        id: 1,
        name: "Wireless Headphones",
        price: 2499,
        quantity: 2,
        totalPrice: 4998
      }
    ],
    totalQuantity: 2,
    totalPrice: 4998
  }
}
```

### Actions Available

| Action | What It Does |
|--------|-------------|
| `addToCart({ id, name, price })` | Adds item or increases quantity if exists |
| `removeFromCart(id)` | Completely removes item from cart |
| `increaseQuantity(id)` | Adds 1 to item quantity |
| `decreaseQuantity(id)` | Removes 1 from quantity (removes item if quantity is 1) |
| `clearCart()` | Empties the entire cart |

---

## How to Run

1. Make sure dependencies are installed:
   ```bash
   npm install @reduxjs/toolkit react-redux
   ```

2. In your `App.jsx`, import and use the `Lab22Demo` component:
   ```jsx
   import Lab22Demo from "./Labs/lab-22/Lab22Demo";

   function App() {
     return <Lab22Demo />;
   }
   ```

3. Start the dev server:
   ```bash
   npm run dev
   ```

4. Use the tab navigation at the top to switch between the three programs.

---

## Complete Data Flow Walkthrough

Let us trace exactly what happens when a user clicks "Add to Cart" on the Wireless Headphones:

```
STEP 1: User clicks "Add to Cart" button
         ↓
STEP 2: onClick handler runs:
         dispatch(addToCart({ id: 1, name: "Wireless Headphones", price: 2499 }))
         ↓
STEP 3: dispatch() sends this action object to the Redux store:
         {
           type: "cart/addToCart",
           payload: { id: 1, name: "Wireless Headphones", price: 2499 }
         }
         ↓
STEP 4: The store sends this action to the cartSlice reducer
         ↓
STEP 5: The reducer runs the addToCart function:
         - Checks if item with id:1 exists in state.items
         - If not, pushes a new item object
         - Adds 1 to state.totalQuantity
         - Adds 2499 to state.totalPrice
         ↓
STEP 6: Redux Toolkit (Immer) creates a new immutable state from the "mutations"
         ↓
STEP 7: The store updates with the new state
         ↓
STEP 8: All components using useSelector for cart data re-render
         - CartApp sees the new items array and displays the item
         - The total count badge updates
         - The total price updates
```

---

## File Structure

```
lab-22/
├── store/
│   ├── store.js           ← Configures the Redux store with all slices
│   ├── themeSlice.js      ← Theme state: light/dark mode
│   ├── authSlice.js       ← Auth state: login/logout/error
│   └── cartSlice.js       ← Cart state: items/quantities/totals
├── ThemeSwitcher.jsx      ← Program A: Theme switching UI
├── UserAuth.jsx           ← Program B: Login/Logout UI
├── CartApp.jsx            ← Program C: Shopping cart UI
├── Lab22Demo.jsx          ← Main wrapper with tab navigation + Provider
└── README.md              ← This file
```

---

## Summary

| Concept | What It Means |
|---------|--------------|
| **Redux** | A centralized state management library for JavaScript apps |
| **Store** | A single JavaScript object holding all shared state |
| **Action** | A plain object describing what happened (has `type` and optional `payload`) |
| **Reducer** | A pure function that takes state + action and returns new state |
| **Dispatch** | The method used to send actions to the store |
| **Slice** | A feature-based bundle of state + reducers + actions (Redux Toolkit) |
| **Provider** | React component that gives all children access to the store |
| **useSelector** | Hook to read data from the Redux store |
| **useDispatch** | Hook to get the dispatch function for sending actions |
| **Redux Toolkit** | The modern, simplified way to write Redux (less boilerplate) |
| **Immer** | Library used internally by RTK that lets you write "mutating" code that safely produces immutable updates |

Redux gives your React application a predictable, centralized, and debuggable way to manage state. Redux Toolkit makes it easy to set up and use. For any feature in your app, you create a slice that defines the state and how it changes, register that slice in the store, and then use `useSelector` and `useDispatch` in your components to interact with that state.
