# Currying and Partial Application

## **Currying**
Currying is a functional programming technique where a function with multiple arguments is transformed into a sequence of functions, each taking a single argument. This allows you to call the function one argument at a time.

### **Key Points:**
1. Currying translates a function of multiple arguments into a series of nested unary functions (functions taking a single argument).
2. It’s especially useful in scenarios where you want to reuse a function by pre-filling some arguments.

### **Example:**
```javascript
// Regular function
const multiply = (a, b) => a * b;
console.log(multiply(3, 4)); // Output: 12

// Curried function
const multiplyCurried = (a) => (b) => a * b;
console.log(multiplyCurried(3)(4)); // Output: 12

// Reuse with partial invocation
const multiplyBy5 = multiplyCurried(5);
console.log(multiplyBy5(4)); // Output: 20
```


### **When and Why Use Currying?**

**Benefits**:

- **Partial Application**: Currying allows you to create partially applied functions by supplying some arguments in advance. This is useful when certain parameters are fixed, and the remaining ones will vary.
- **Reusability**: You can create specialized functions from a general-purpose function by partially applying arguments.
- **Readability**: In some cases, curried functions make code more readable and declarative, especially in functional programming.
- **Avoid Repetition**: Currying reduces the need to repeatedly pass the same arguments, making code cleaner.

**When to Use Currying**:

- Use currying when you need to create functions that are partially applied and reusable with different arguments.
- It’s also helpful when working with functions that need to handle configuration options or context, such as logging functions or functions interacting with APIs.

### **Practical Use Case:**
Currying can be beneficial when dealing with functional pipelines, where functions are composed to process data step by step. For instance, in **React**, you can create higher-order functions that accept configuration values and return a function for event handlers.

---

## **Partial Application**
Partial application involves fixing a few arguments of a function, creating a new function with the remaining arguments. This technique uses **closures** to remember the arguments that are already supplied.

### **Key Points:**
1. Partially applied functions take some arguments and return a new function waiting for the rest.
2. It's like currying but not limited to one argument at a time.

### **Example:**
```javascript
// Regular function
const normalFunc = (a, b, c) => a * b * c;

// Partially applied function
const partialMultiplyBy5 = normalFunc.bind(null, 5);
console.log(partialMultiplyBy5(4, 10)); // Output: 200
```

### **Difference from Currying:**
- **Currying:** Each invocation takes one argument.
  ```javascript
  const curriedFunc = (a) => (b) => (c) => a * b * c;
  console.log(curriedFunc(3)(4)(5)); // Output: 60
  ```
- **Partial Application:** Fixes some arguments and expects the rest together.
  ```javascript
  const partial = normalFunc.bind(null, 3);
  console.log(partial(4, 5)); // Output: 60
  ```

---

## **Summary of Differences**
- **Currying:** Expects arguments one at a time.
- **Partial Application:** Fixes some arguments and expects the rest in a single call.

---

## **Common Interview Coding Questions**
"Currying is a functional programming concept where a function taking multiple arguments is transformed into a series of functions, each taking a single argument. For example:

```javascript
const add = (a) => (b) => a + b;
console.log(add(10)(20)); // 30
```

In contrast, Partial Application fixes some arguments in advance and creates a new function with the remaining arguments. For example:

```javascript
const multiply = (a, b, c) => a * b * c;
const multiplyBy2 = multiply.bind(null, 2);
console.log(multiplyBy2(3, 4)); // 24
```

While currying is a step-by-step approach, partial application is more flexible, allowing you to reuse functions by pre-filling specific arguments."

---

## **When to Use Currying and Partial Application**
- **Currying** is ideal for functional programming patterns where each step processes one argument at a time (e.g., composing functions, applying middleware).
- **Partial Application** is useful for pre-configuring reusable functions, such as setting default parameters or context.

---

This structured explanation and examples should help you articulate the concept in interviews confidently.

---
### CODE

### **1. Write a curried function to add two numbers.**
**Problem:**
Create a function `add` such that it works as `add(10)(20)` and returns `30`.

**Solution:**
```javascript
const add = (a) => (b) => a + b;

console.log(add(10)(20)); // Output: 30
```

---

### 2. **Add Two Numbers (Both Curried and Non-Curried):**
```javascript
const add = (a, b) => (b !== undefined ? a + b : (c) => a + c);
console.log(add(10, 20)); // Output: 30
console.log(add(10)(20)); // Output: 30
```

---

### **3. Implement a curried function that sums an infinite number of arguments.**
**Problem:**
Write a function `sum` such that it works like this:
```javascript
sum(1)(2)(3)(4)(); // Output: 10
```

**Solution:**
```javascript
const sum = (a) => (b) => b !== undefined ? sum(a + b) : a;

console.log(sum(1)(2)(3)(4)()); // Output: 10
```

---

### **4. Convert a normal function to a curried function.**
**Problem:**
Write a function `curry` that transforms a normal function into a curried function.

**Solution:**
```javascript
const curry = (fn) => {
  return function curried(...args) {
    if (args.length >= fn.length) {
      return fn(...args);
    } else {
      return (...nextArgs) => curried(...args, ...nextArgs);
    }
  };
};

// Example usage:
const sum = (a, b, c) => a + b + c;
const curriedSum = curry(sum);

console.log(curriedSum(1)(2)(3)); // Output: 6
console.log(curriedSum(1, 2)(3)); // Output: 6
console.log(curriedSum(1, 2, 3)); // Output: 6
```

---

### **5. Implement a curried function with a fixed number of arguments.**
**Problem:**
Write a function `multiply` that takes arguments one at a time until all are supplied, then computes the product.

**Solution:**
```javascript
const multiply = (a) => (b) => (c) => a * b * c;

console.log(multiply(2)(3)(4)); // Output: 24
```

---

### **6. Create a function to reverse the arguments of a curried function.**
**Problem:**
Given a curried function, write a utility `reverseCurry` that allows arguments to be passed in reverse order.

**Solution:**
```javascript
const reverseCurry = (fn) => (...args) => {
  return args.reverse().reduce((acc, arg) => acc(arg), fn);
};

// Example curried function
const multiply = (a) => (b) => (c) => a * b * c;
const reverseMultiply = reverseCurry(multiply);

console.log(reverseMultiply(4)(3)(2)); // Output: 24
```

---

### **7. Write a function that curries based on dynamic parameters.**
**Problem:**
Create a function `dynamicCurry` that takes a function and dynamically curries its arguments.

**Solution:**
```javascript
const dynamicCurry = (fn) => {
  return function curried(...args) {
    if (args.length >= fn.length) {
      return fn(...args);
    } else {
      return (...nextArgs) => curried(...args, ...nextArgs);
    }
  };
};

// Example:
const sum = (a, b, c, d) => a + b + c + d;
const curriedSum = dynamicCurry(sum);

console.log(curriedSum(1)(2)(3)(4)); // Output: 10
console.log(curriedSum(1, 2)(3, 4)); // Output: 10
```

---

### **8. Currying with Variable Number of Arguments (Sum Function)**

To create a function that takes `n` number of arguments and returns their sum through currying, you need to handle indefinite arguments and return a function until no arguments are provided.

### **Implementation of Sum Function Using Currying**

There are two common approaches to this problem.

### Approach 1: Recursive Currying with Chaining

In this approach, the function keeps returning a new function if arguments are provided. When no arguments are given, it returns the accumulated sum.

```jsx
javascript
Copy code
function sum(a) {
  return function (b) {
    if (b !== undefined) {
      return sum(a + b); // Keep adding to the sum if more args are provided
    }
    return a; // Final result when no more arguments
  };
}

console.log(sum(1)(2)(3)(4)()); // Output: 10
console.log(sum(5)(10)(15)()); // Output: 30

```

### Explanation:

Each call adds the next argument to the running total (`a + b`) until no arguments are passed (indicated by `b === undefined`). When this happens, the function returns the total accumulated sum.

### Approach 2: Using Rest Parameters and Reduce

Another way to achieve a variable argument sum function is to use the `...args` syntax and `reduce`.

```jsx
javascript
Copy code
function sum(...args) {
  return function (...nextArgs) {
    if (nextArgs.length === 0) {
      return args.reduce((acc, curr) => acc + curr, 0);
    }
    return sum(...args, ...nextArgs); // Keep calling sum with all arguments
  };
}

console.log(sum(1)(2)(3)(4)()); // Output: 10
console.log(sum(5)(10)(15)()); // Output: 30

```

### Explanation:

Here, each function call adds the arguments to an array (`args`), which is then reduced to get the sum when no further arguments are passed.
---
#### **1. What is currying in JavaScript?**
Currying is a technique in functional programming where a function is transformed into a sequence of nested functions, each taking a single argument. For example:
```javascript
const add = (a) => (b) => a + b;
console.log(add(2)(3)); // Output: 5
```
This allows for more flexible and reusable code, such as creating pre-configured functions.

#### **2. How is currying different from partial application?**
Currying takes one argument at a time, while partial application fixes some arguments and takes the rest together:
```javascript
// Currying
const curried = (a) => (b) => a + b;
console.log(curried(1)(2)); // 3

// Partial Application
const partial = (a, b) => a + b;
const add5 = partial.bind(null, 5);
console.log(add5(3)); // 8
```

#### **3. Why is currying useful?**
Currying helps in:
1. Function composition in functional programming.
2. Reusability by creating pre-configured functions.
3. Making code more declarative and readable.

#### **4. Can you implement `Function.prototype.curry`?**
```javascript
Function.prototype.curry = function() {
  const fn = this;
  return function curried(...args) {
    if (args.length >= fn.length) {
      return fn(...args);
    } else {
      return (...nextArgs) => curried(...args, ...nextArgs);
    }
  };
};

// Example:
function add(a, b, c) {
  return a + b + c;
}
const curriedAdd = add.curry();
console.log(curriedAdd(1)(2)(3)); // Output: 6
```

#### **5. How would you curry a function that accepts rest parameters?**
Currying with rest parameters requires handling dynamic arguments:
```javascript
const curry = (fn) => {
  const curried = (...args) =>
    args.length >= fn.length
      ? fn(...args)
      : (...nextArgs) => curried(...args, ...nextArgs);
  return curried;
};

const sum = (...args) => args.reduce((acc, val) => acc + val, 0);
const curriedSum = curry(sum);

console.log(curriedSum(1)(2)(3)(4)(5)); // Output: 15
```

#### **6. How is currying implemented in libraries like Lodash?**
Lodash provides a utility `_.curry` that simplifies currying implementation:
```javascript
const _ = require('lodash');

const add = (a, b, c) => a + b + c;
const curriedAdd = _.curry(add);

console.log(curriedAdd(1)(2)(3)); // Output: 6
console.log(curriedAdd(1, 2)(3)); // Output: 6
```



---

### **How to Prepare for Currying Questions?**
1. Practice converting regular functions to curried ones.
2. Understand and explain the difference between currying and partial application.
3. Be prepared to write utility functions for currying.
4. Study real-world use cases like function composition and middleware chaining.
5. Explore libraries like Lodash for their curry implementations.