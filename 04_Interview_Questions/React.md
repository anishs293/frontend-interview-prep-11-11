# React Interview Questions

1. **Explain the difference between state and props in React?**

<details>
<summary>Click to see the answer</summary>

### **Difference Between State and Props in React**

**1. Fundamental Definition**:
   - **State**: State is a local, mutable data source owned and managed by a component. It is used to store information that a component needs to keep track of and update over time.
   - **Props**: Props (short for "properties") are read-only, immutable inputs passed from a parent component to its child components. Props allow components to communicate and share data in a one-way flow.

**2. Ownership and Management**:
   - **State**: Each component manages its own state. This data is internal to the component and is only accessible and modifiable by that component.
   - **Props**: Props are owned by the parent component, which passes them down to child components as needed. The child component has no control over the props and cannot modify them.

**3. Mutability**:
   - **State**: State is mutable, meaning it can be changed within the component. The `setState` method (in class components) or the `useState` hook (in functional components) is used to update the state, which triggers a re-render.
   - **Props**: Props are immutable within the child component. If a child needs to display or handle data from props differently, it has to rely on the parent component to pass new values rather than changing the props directly.

**4. Primary Use Cases**:
   - **State**: Used for data that changes over time, like form inputs, toggle switches, or anything requiring interactivity or real-time updates within a component.
   - **Props**: Used to pass data and configuration to child components, such as styles, text labels, or IDs. Props make components reusable by customizing their output based on different inputs.

**5. Flow Direction**:
   - **State**: State is confined to the component that owns it. However, components can pass functions down as props to allow child components to interact with or modify the parent’s state.
   - **Props**: Props follow a one-way data flow, going from parent to child only. This one-way data flow helps maintain a predictable structure in the component hierarchy.

**6. Re-rendering Behavior**:
   - **State**: When state changes, the component re-renders, and React calculates the changes needed to update the UI.
   - **Props**: Changes in props trigger re-renders in the child component, making props useful for ensuring child components update when the parent provides new data.

---

### **Code Example**: Distinguishing Between State and Props

Here’s an example of a `Counter` component that manages its own state and a `CounterDisplay` component that receives props from `Counter` to display the count.

```jsx
import React, { useState } from 'react';

// CounterDisplay is a presentational component that uses props
const CounterDisplay = ({ count }) => {
  return <h1>Current Count: {count}</h1>;
};

// Counter manages its own state and passes it as props to CounterDisplay
const Counter = () => {
  const [count, setCount] = useState(0);

  const increment = () => setCount(count + 1);
  const decrement = () => setCount(count - 1);

  return (
    <div>
      <CounterDisplay count={count} /> {/* Passing state as props */}
      <button onClick={increment}>Increment</button>
      <button onClick={decrement}>Decrement</button>
    </div>
  );
};

export default Counter;
```

### **Explanation**:
   - **State**: The `count` variable in `Counter` is stateful, and the `setCount` function allows `Counter` to update its own state.
   - **Props**: The `CounterDisplay` component receives `count` as a prop from `Counter`. `CounterDisplay` does not control or modify this prop but only displays it.

---

### **Summary Points** for an Interview:
- **State** is local, mutable, and owned by the component that uses it; it's mainly for dynamic, interactive data.
- **Props** are immutable inputs from a parent component, used for data or configuration, allowing components to be reusable and modular.
- **State Changes** trigger a re-render of the component, while **Prop Changes** in a child component re-render it based on updated data from the parent.
- **Mutual Dependency**: While state holds data and behavior within a component, props allow the child to display or interact with this data without altering it directly.
- **Example Use Case**: Props suit static data passed to child components (like UI labels), while state fits interactive elements within the component, such as counters, form inputs, or toggle switches.

This thorough yet concise breakdown should convey a solid understanding of the roles and differences between state and props in React, suitable for a senior-level React interview.

</details>

---

2. **Explain the difference between state and props in React?**

<details>
<summary>Click to see the answer</summary>