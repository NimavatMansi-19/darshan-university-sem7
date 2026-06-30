# React `useReducer` Hook - Complete Guide

## What is useReducer?

`useReducer` is a React Hook used to manage **complex state**.

Instead of updating state directly like `useState`, we send an **action** describing **what happened**, and a **reducer function** decides how the state should change.

Think of it like giving an order to a manager.

You don't change the data yourself.

You simply tell the manager:

> "Increase quantity"

The manager decides how to update everything.

---

# Real Life Example

Imagine a Shopping Cart.

You don't directly change the total bill.

Instead you perform actions like:

- Add Item
- Remove Item
- Clear Cart
- Increase Quantity
- Decrease Quantity

The shopkeeper receives your request and updates the bill.

React's `useReducer` works exactly like this.

```
Customer
   │
   ▼
dispatch(action)
   │
   ▼
Reducer Function
   │
   ▼
Returns New State
   │
   ▼
UI Updates
```

---

# Why not use useState?

Suppose your cart contains:

```javascript
{
   items: [],
   total: 0,
   discount: 10,
   tax: 5
}
```

With useState, updating everything becomes difficult.

Example:

```javascript
setCart({
   ...cart,
   total: cart.total + 500
})
```

Imagine writing dozens of updates like this.

Instead, useReducer lets us simply write

```javascript
dispatch({
   type: "ADD_ITEM"
})
```

Cleaner and easier.

---

# Syntax

```javascript
const [state, dispatch] = useReducer(reducer, initialState);
```

There are three parts.

## 1. reducer

A function that decides how state changes.

```javascript
function reducer(state, action) {

}
```

---

## 2. initialState

Starting value.

```javascript
const initialState = {
    count: 0
}
```

---

## 3. dispatch()

Used to send an action.

```javascript
dispatch({
   type: "INCREMENT"
})
```

dispatch DOES NOT update state.

It simply sends a message.

---

# Visual Flow

```
Button Click
      │
      ▼
dispatch(action)
      │
      ▼
Reducer
      │
      ▼
Creates New State
      │
      ▼
React Updates UI
```

---

# Understanding Reducer Function

Reducer has two parameters.

```javascript
(state, action)
```

Example

```javascript
function reducer(state, action){

}
```

### state

Current state

Example

```javascript
{
   count:5
}
```

### action

What user wants to do.

Example

```javascript
{
   type:"INCREMENT"
}
```

Reducer checks the action type.

---

# Example Reducer

```javascript
function reducer(state, action){

   switch(action.type){

      case "INCREMENT":

         return {
             count: state.count + 1
         }

      case "DECREMENT":

         return {
             count: state.count - 1
         }

      default:
         return state;
   }

}
```

Notice:

Reducer NEVER modifies old state.

Always return a NEW object.

Correct

```javascript
return {
   count: state.count + 1
}
```

Wrong

```javascript
state.count++;

return state;
```

Never mutate state.

---

# Step 1 : Import Hook

```javascript
import { useReducer } from "react";
```

---

# Step 2 : Create Initial State

```javascript
const initialState = {
    count:0
}
```

---

# Step 3 : Create Reducer Function

```javascript
function reducer(state, action){

   switch(action.type){

      case "INCREMENT":
         return {
            count: state.count + 1
         }

      case "DECREMENT":
         return {
            count: state.count - 1
         }

      case "RESET":
         return {
            count:0
         }

      default:
         return state;
   }

}
```

---

# Step 4 : Connect useReducer

```javascript
const [state, dispatch] = useReducer(
     reducer,
     initialState
);
```

Now

```
state.count
```

contains current value.

---

# Step 5 : Create Buttons

Increment

```javascript
<button
onClick={()=>
dispatch({
type:"INCREMENT"
})
}
>

Increment

</button>
```

Decrement

```javascript
<button
onClick={()=>
dispatch({
type:"DECREMENT"
})
}
>

Decrement

</button>
```

Reset

```javascript
<button
onClick={()=>
dispatch({
type:"RESET"
})
}
>

Reset

</button>
```

Display

```javascript
<h2>{state.count}</h2>
```

---

# Complete Counter Program

```jsx
import { useReducer } from "react";

const initialState = {
  count: 0
};

function reducer(state, action) {

  switch (action.type) {

    case "INCREMENT":
      return {
        count: state.count + 1
      };

    case "DECREMENT":
      return {
        count: state.count - 1
      };

    case "RESET":
      return {
        count: 0
      };

    default:
      return state;
  }

}

function App() {

  const [state, dispatch] = useReducer(
    reducer,
    initialState
  );

  return (

    <div>

      <h2>{state.count}</h2>

      <button
      onClick={()=>
      dispatch({
      type:"INCREMENT"
      })
      }
      >
      +
      </button>

      <button
      onClick={()=>
      dispatch({
      type:"DECREMENT"
      })
      }
      >
      -
      </button>

      <button
      onClick={()=>
      dispatch({
      type:"RESET"
      })
      }
      >
      Reset
      </button>

    </div>

  );

}

export default App;
```

---

# Real Life Example : Shopping Cart

Suppose your shopping cart starts with

```javascript
const initialState = {

    items: [],
    total:0

}
```

User clicks

```
Add Laptop
```

React executes

```javascript
dispatch({

   type:"ADD_ITEM",

   payload:{
      name:"Laptop",
      price:50000
   }

})
```

Reducer receives

```javascript
function reducer(state, action){

   switch(action.type){

      case "ADD_ITEM":

      return {

         items:[
             ...state.items,
             action.payload
         ],

         total:
         state.total +
         action.payload.price

      }

   }

}
```

Now UI automatically shows

```
Items : 1

Total : 50000
```

Without directly changing state.

---

# Why dispatch instead of setState?

With useState

```javascript
setCount(count+1)
```

With useReducer

```javascript
dispatch({
type:"INCREMENT"
})
```

dispatch only sends instructions.

Reducer performs the update.

This separates logic from UI.

---

# When should we use useReducer?

Use it when

✅ Multiple state values

✅ Complex logic

✅ Shopping Cart

✅ Login Form

✅ Employee Management

✅ Todo Application

✅ Banking Application

✅ Inventory System


---

# Difference Between useState and useReducer

| useState | useReducer |
|-----------|------------|
| Simple state | Complex state |
| Updates directly | Updates through reducer |
| Uses setState | Uses dispatch |
| Easy to learn | Better for large projects |
| Logic inside component | Logic inside reducer |

---
