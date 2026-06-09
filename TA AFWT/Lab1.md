# ReactJS Lab 01 – Components, Props, Event Handling, Conditional Rendering & Map Method

## Introduction

ReactJS is a JavaScript library used to build user interfaces using reusable components.


# 1. Function Component in Separate File and Link with App.js

## What is a Function Component?

A Function Component is a JavaScript function that returns JSX.

### Syntax

```jsx
function ComponentName() {
  return (
    <h1>Hello React</h1>
  );
}

export default ComponentName;
```

### Steps

1. Create a new file `MyComponent.jsx`
2. Create the function component.
3. Export the component.
4. Import it into `App.js`.
5. Use the component as a tag.

### Import Syntax

```jsx
import MyComponent from './MyComponent';
```

### Usage Syntax

```jsx
<MyComponent />
```

### Advantages

* Simple and easy to write.
* Less code than class components.
* Preferred in modern React.

---

# 2. Class Component in Separate File and Link with App.js

## What is a Class Component?

A Class Component is created using an ES6 class and extends React.Component.

### Syntax

```jsx
import React, { Component } from 'react';

class MyClass extends Component {
  render() {
    return (
      <h1>Hello React</h1>
    );
  }
}

export default MyClass;
```

### Import Syntax

```jsx
import MyClass from './MyClass';
```

### Usage Syntax

```jsx
<MyClass />
```

### Advantages

* Supports lifecycle methods.
* Can manage state.
* Found in older React projects.

### Difference

| Function Component | Class Component        |
| ------------------ | ---------------------- |
| Uses function      | Uses class             |
| Less code          | More code              |
| Uses Hooks         | Uses lifecycle methods |
| Modern approach    | Traditional approach   |

---

# 3. Props in ReactJS

## What are Props?

Props (Properties) are used to pass data from a parent component to a child component.

### Parent Component Syntax

```jsx
<Student
  name="Mansi"
  age="22"
/>
```

### Child Component Syntax

```jsx
function Student(props) {
  return (
    <div>
      {props.name}
      {props.age}
    </div>
  );
}
```

### Destructuring Syntax

```jsx
function Student({ name, age }) {
  return (
    <div>
      {name}
      {age}
    </div>
  );
}
```

### Types of Props

#### String

```jsx
<Component name="Mansi" />
```

#### Number

```jsx
<Component age={22} />
```

#### Boolean

```jsx
<Component isActive={true} />
```

#### Array

```jsx
<Component subjects={["React","Java"]} />
```

#### Object

```jsx
<Component student={{id:1,name:"Mansi"}} />
```

### Advantages

* Data sharing between components.
* Reusable components.
* Dynamic UI.

---

# 4. Event Handling in ReactJS

## What is Event Handling?

Event handling is used to perform actions when users interact with the application.

### Click Event Syntax

```jsx
<button onClick={handleClick}>
  Click Me
</button>
```

### Event Handler Function

```jsx
function handleClick() {
  alert("Button Clicked");
}
```

### Arrow Function Syntax

```jsx
<button onClick={() => alert("Clicked")}>
  Click
</button>
```

### Common Events

| Event       | Syntax      |
| ----------- | ----------- |
| Click       | onClick     |
| Change      | onChange    |
| Submit      | onSubmit    |
| Key Press   | onKeyDown   |
| Mouse Hover | onMouseOver |

### Example Syntax

```jsx
<input
  type="text"
  onChange={handleChange}
/>
```

```jsx
<form onSubmit={handleSubmit}>
```

### Advantages

* Makes applications interactive.
* Handles user actions easily.

---

# 5. Conditional Rendering

## What is Conditional Rendering?

Conditional rendering means displaying elements based on a condition.

### Scenario

Display an element only if `isDisplay` is true.

---

## Method 1: if-else

### Syntax

```jsx
if(isDisplay){
  return <h1>Visible</h1>;
}
else{
  return <h1>Hidden</h1>;
}
```

---

## Method 2: Ternary Operator

### Syntax

```jsx
{
  isDisplay
  ? <h1>Visible</h1>
  : <h1>Hidden</h1>
}
```

---

## Method 3: Logical AND (&&)

### Syntax

```jsx
{
  isDisplay &&
  <h1>Visible</h1>
}
```

### Advantages

* Dynamic UI.
* Show/Hide content easily.
* Better user experience.

---

# 6. Map Method in ReactJS

## What is map()?

The map() method is used to display array data dynamically.

### JavaScript Syntax

```javascript
array.map((item,index)=>{
   return value;
});
```

### React Syntax

```jsx
{
  students.map((student) => (
    <p>{student}</p>
  ))
}
```

### Array Example

```javascript
const students = [
  "Mansi",
  "Rahul",
  "Priya"
];
```

### Using Key Attribute

```jsx
{
  students.map((student,index)=>(
    <li key={index}>
      {student}
    </li>
  ))
}
```

### Why Key is Required?

* Identifies unique elements.
* Improves rendering performance.
* Helps React update UI efficiently.


