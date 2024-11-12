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

### **Explanation**

- **State**: The `count` variable in `Counter` is stateful, and the `setCount` function allows `Counter` to update its own state.
- **Props**: The `CounterDisplay` component receives `count` as a prop from `Counter`. `CounterDisplay` does not control or modify this prop but only displays it.

---

### **Summary Points** for an Interview

- **State** is local, mutable, and owned by the component that uses it; it's mainly for dynamic, interactive data.
- **Props** are immutable inputs from a parent component, used for data or configuration, allowing components to be reusable and modular.
- **State Changes** trigger a re-render of the component, while **Prop Changes** in a child component re-render it based on updated data from the parent.
- **Mutual Dependency**: While state holds data and behavior within a component, props allow the child to display or interact with this data without altering it directly.
- **Example Use Case**: Props suit static data passed to child components (like UI labels), while state fits interactive elements within the component, such as counters, form inputs, or toggle switches.

This thorough yet concise breakdown should convey a solid understanding of the roles and differences between state and props in React, suitable for a senior-level React interview.

</details>

---

2. **How do you pass data from a child component to a parent component?**

<details> <summary>Click to see the answer</summary>

To pass data from **child to parent** in React, we use a **callback function**. The parent component defines a function and passes it to the child as a prop. The child then calls this function to send data back up.

### Code Example

```jsx
// Parent Component
import React, { useState } from 'react';
import ChildComponent from './ChildComponent';

function ParentComponent() {
  const [message, setMessage] = useState('');

  // Callback to receive data from child
  const handleMessageChange = (newMessage) => {
    setMessage(newMessage);
  };

  return (
    <div>
      <h1>Parent Component</h1>
      <p>Message from Child: {message}</p>
      <ChildComponent onMessageChange={handleMessageChange} />
    </div>
  );
}

export default ParentComponent;

// Child Component
import React from 'react';

function ChildComponent({ onMessageChange }) {
  const sendMessageToParent = () => {
    onMessageChange('Hello from Child!');
  };

  return (
    <div>
      <h2>Child Component</h2>
      <button onClick={sendMessageToParent}>Send Message to Parent</button>
    </div>
  );
}

export default ChildComponent;

Explanation:
The ParentComponent defines a message state and handleMessageChange function to update it.
The ChildComponent calls the onMessageChange function (passed from the parent) when the button is clicked, sending data back up.
```

**Follow-Up Question:**

Q: What are other ways to share data between components in React?

A: In addition to callbacks, other ways include:

-Context API: Useful for sharing data across multiple levels without prop drilling.

-Global State Management (e.g., Redux): Stores global state accessible by any component.

-Custom Hooks: Can contain logic and state shared across components.
Event Emitters: Emit events to share data in complex applications.

</details>

---

3. **Why do we need to create a copy of the state when updating it in React?**
<details> <summary>Click to see the answer</summary>

In React, we create a copy of the state when updating it to maintain immutability. This helps React detect changes more effectively, making the app’s behavior predictable and improving performance. Directly modifying state can lead to bugs and unnecessary re-renders.

Key Points:

Immutability for Change Detection: Creating a new copy lets React detect changes, which is essential for triggering the correct re-renders.
Predictability: Immutable updates make the state predictable and debugging easier.
Performance: Immutability helps React optimize re-renders, improving overall performance.
</details>

---

4. **How Would You Prevent Unnecessary Re-renders of a Component?**

<details> <summary>Click to see the answer</summary>

Preventing unnecessary re-renders is essential in React to enhance performance and avoid slowdowns, especially in large applications. Here are effective strategies to avoid re-renders:

### Answer

To prevent unnecessary re-renders, you can use tools like `React.memo`, `useMemo`, and `useCallback` to control when a component should re-render. These techniques ensure that components only re-render when their props or state have actually changed.

---

### Key Techniques to Prevent Unnecessary Re-renders

1. **React.memo for Functional Components**
   - **Purpose**: Wraps a functional component to prevent re-renders if its props haven't changed.
   - **How**: `React.memo` performs a shallow comparison of the component's props, re-rendering only when there's a change.
   - **Example**:

     ```javascript
     const MemoizedComponent = React.memo(MyComponent);
     ```

2. **useMemo for Expensive Calculations**
   - **Purpose**: Memoizes the result of an expensive calculation, preventing it from re-running on every render.
   - **How**: Use `useMemo` to cache calculated values based on dependencies, ensuring the calculation only runs when dependencies change.
   - **Example**:

     ```javascript
     const memoizedValue = useMemo(() => computeExpensiveValue(a, b), [a, b]);
     ```

3. **useCallback for Functions**
   - **Purpose**: Memoizes function instances to prevent passing a new function reference on each render.
   - **How**: Use `useCallback` to create stable function references, especially when passing functions as props to child components.
   - **Example**:

     ```javascript
     const memoizedCallback = useCallback(() => handleClick(id), [id]);
     ```

4. **Avoiding Inline Functions and Inline Object Declarations**
   - **Purpose**: Prevents creating new object or function references on each render, which can trigger re-renders in child components.
   - **How**: Define functions and objects outside the render or memoize them with `useCallback` or `useMemo`.
   - **Example**:

     ```javascript
     const memoizedObject = useMemo(() => ({ key: 'value' }), []);
     ```

5. **shouldComponentUpdate and PureComponent for Class Components**
   - **Purpose**: Controls re-renders in class components by skipping updates when props and state haven’t changed.
   - **How**: Use `shouldComponentUpdate` or extend `React.PureComponent` to only re-render when there are shallow changes in props or state.
   - **Example**:

     ```javascript
     class MyComponent extends React.PureComponent {
       render() { /*...*/ }
     }

     class AnotherComponent extends React.Component {
       shouldComponentUpdate(nextProps) {
         return nextProps.someProp !== this.props.someProp;
       }
       render() { /*...*/ }
     }
     ```

6. **Key Usage in Lists**
   - **Purpose**: Stable keys help React correctly identify and update items in lists, avoiding unnecessary re-renders.
   - **How**: Ensure each list item has a unique and consistent key.
   - **Example**:

     ```javascript
     items.map(item => <ListItem key={item.id} item={item} />);
     ```

These strategies ensure that components in React only re-render when necessary, enhancing performance and keeping the app responsive.
</details>

---

5. **How would you share data across multiple components in React?**
<details> <summary>Click to see the answer</summary>

Answer:
In React, data can be shared across components through props, Context API, and state management libraries. Each method serves different use cases depending on the complexity of data sharing.

Key Points:

Props: The simplest way to share data, by passing it down from parent to child components.
Context API: Useful for sharing data across deeply nested components without prop drilling. It’s ideal for global data like themes or user authentication.
State Management Libraries (e.g., Redux, MobX): These libraries are used for larger applications where multiple components need access to shared state.
Custom Hooks: Custom hooks can encapsulate and share logic across components, making it easier to reuse shared functionality.
</details>

---

6. **How Does Lazy Loading Work in React?**

<details> <summary>Click to see the answer</summary>

In React, **lazy loading** is a technique that allows components or resources to load only when they’re needed, rather than loading everything at once. This improves initial load time and optimizes the app's performance by splitting code and reducing the amount of JavaScript loaded at startup.

### Answer

Lazy loading in React uses `React.lazy` to defer loading of components until they’re required. This approach is often paired with `Suspense` to display fallback content while the component is loading, making the application faster and more responsive.

### Key Points

1. **React.lazy**: Loads components on demand, preventing unnecessary code from being bundled at the initial load.
2. **Suspense**: Provides a fallback UI while the lazy-loaded component is loading.
3. **Optimizes Performance**: Reduces initial load times, especially beneficial for larger apps with many components.

### Example Code for Lazy Loading a Component

```javascript
import React, { Suspense } from 'react';

// Lazy load the component
const LazyComponent = React.lazy(() => import('./LazyComponent'));

function App() {
  return (
    <div>
      <h1>My App</h1>
      {/* Suspense provides a fallback UI while LazyComponent is loading */}
      <Suspense fallback={<div>Loading...</div>}>
        <LazyComponent />
      </Suspense>
    </div>
  );
}

export default App;

```

</details>

---

7. **What Are Some Optimization Techniques for React Applications?**

<details> <summary>Click to see the answer</summary>

In React applications, optimization techniques are crucial for improving performance, enhancing user experience, and ensuring efficient resource usage. Here’s a list of key optimization strategies that can be discussed in an interview:

### Answer

To optimize React applications, you can use techniques like code splitting, lazy loading, and memoization to improve performance and manage rendering more efficiently. These strategies help reduce load times, prevent unnecessary renders, and ensure the app runs smoothly.

---

### Key Optimization Techniques

1. **Code Splitting with React.lazy and Suspense**
   - **Purpose**: Reduces the initial load time by splitting large bundles into smaller, on-demand chunks.
   - **How**: Use `React.lazy` to load components only when they’re needed and wrap them in `Suspense` for fallback handling.
   - **Example**:

     ```javascript
     const LazyComponent = React.lazy(() => import('./LazyComponent'));

     <Suspense fallback={<div>Loading...</div>}>
       <LazyComponent />
     </Suspense>
     ```

2. **Memoization (React.memo, useMemo, useCallback)**
   - **Purpose**: Prevents unnecessary re-renders by caching values or functions.
   - **How**:
     - **React.memo**: Wraps functional components to prevent re-renders if props haven’t changed.
     - **useMemo**: Caches expensive calculations between renders.
     - **useCallback**: Caches function instances to avoid passing a new function reference on each render.
   - **Example**:

     ```javascript
     const MemoizedComponent = React.memo(MyComponent);
     const memoizedValue = useMemo(() => computeExpensiveValue(a, b), [a, b]);
     const memoizedCallback = useCallback(() => handleClick(id), [id]);
     ```

3. **Virtualized Lists (react-window or react-virtualized)**
   - **Purpose**: Efficiently renders large lists by only rendering visible items and reusing DOM nodes for scrolling.
   - **How**: Use libraries like `react-window` or `react-virtualized` to render only the visible portion of large lists, reducing DOM operations.
   - **Example**:

     ```javascript
     import { FixedSizeList as List } from 'react-window';

     <List height={500} itemCount={1000} itemSize={35}>
       {({ index, style }) => <div style={style}>Item {index}</div>}
     </List>
     ```

4. **Avoiding Inline Functions and Inline Object Declarations**
   - **Purpose**: Reduces unnecessary re-renders due to new references created for functions or objects on each render.
   - **How**: Use `useCallback` for functions and define objects outside of the component or memoize them with `useMemo`.
   - **Example**:

     ```javascript
     const memoizedCallback = useCallback(() => doSomething(), []);
     const memoizedObject = useMemo(() => ({ key: 'value' }), []);
     ```

5. **Optimizing React Reconciliation with Keys**
   - **Purpose**: Ensures stable keys for lists to improve React’s diffing algorithm, avoiding re-renders of unchanged components.
   - **How**: Use unique and consistent keys when rendering lists, especially if the items have a unique ID.
   - **Example**:

     ```javascript
     items.map(item => <ListItem key={item.id} item={item} />);
     ```

6. **Server-Side Rendering (SSR)**
   - **Purpose**: Renders components on the server and sends HTML to the client, improving load times and SEO.
   - **How**: Use frameworks like **Next.js** to implement SSR, enabling faster load times and better initial page load experience.
   - **Example**: With Next.js:

     ```javascript
     export async function getServerSideProps() {
       // Fetch data and return as props
       return { props: { data } };
     }
     ```

7. **Using Throttling and Debouncing for Event Handlers**
   - **Purpose**: Limits the frequency of function calls in response to events like scrolling, resizing, or typing.
   - **How**: Use throttling to limit function calls to a fixed rate or debouncing to wait until an action stops.
   - **Example**:

     ```javascript
     const handleResize = debounce(() => console.log('Resized'), 300);
     ```

8. **Reducing Component Complexity**
   - **Purpose**: Improves maintainability and performance by breaking down large components into smaller, reusable pieces.
   - **How**: Refactor large components into smaller ones and use props to pass data.

9. **Optimizing Images and Assets**
   - **Purpose**: Reduces the size of images and other assets for faster load times.
   - **How**: Use optimized images, lazy load them where appropriate, and use tools like **Webpack** to manage asset sizes.

10. **Minimizing Reconciliation with shouldComponentUpdate or PureComponent (Class Components)**
    - **Purpose**: Controls re-rendering in class components by skipping updates when unnecessary.
    - **How**:
      - **PureComponent**: Extends `React.PureComponent` to enable shallow comparison for props and state.
      - **shouldComponentUpdate**: Override in a class component to control when it should re-render.
    - **Example**:

      ```javascript
      class MyComponent extends React.PureComponent {
        render() { /*...*/ }
      }

      class AnotherComponent extends React.Component {
        shouldComponentUpdate(nextProps) {
          return nextProps.someProp !== this.props.someProp;
        }
        render() { /*...*/ }
      }
      ```

</details>

---
