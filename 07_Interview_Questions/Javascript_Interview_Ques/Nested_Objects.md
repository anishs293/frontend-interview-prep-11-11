Certainly! Here are some interview questions related to working with nested objects, covering topics from destructuring to immutability, reference handling, and deep cloning. These questions test your understanding of nested structures and how to manipulate them effectively.

---

### **1. Destructuring Nested Objects**

**Q1**: How would you destructure the following object to extract `city` and `zip` from the nested `address` object?

```jsx
const user = {
  name: "John",
  age: 30,
  address: {
    city: "New York",
    zip: "10001"
  }
};

```

*Expected Answer*:

```jsx
const { address: { city, zip } } = user;
console.log(city, zip); // Output: "New York", "10001"

```

---

### **2. Updating Properties in Nested Objects Without Mutation**

**Q2**: Given the following `user` object, how would you update the `zip` code in `address` without mutating the original object?

```jsx
const user = {
  name: "John",
  age: 30,
  address: {
    city: "New York",
    zip: "10001"
  }
};

```

*Expected Answer*:

```jsx
const updatedUser = { ...user, address: { ...user.address, zip: "10002" } };
console.log(updatedUser.address.zip); // Output: "10002"
console.log(user.address.zip); // Original remains unchanged

```

---

### **3. Freezing a Nested Object**

**Q3**: If you wanted to make the `user` object immutable, including all nested levels, how would you do it?

*Expected Answer*:

```jsx
function deepFreeze(obj) {
  Object.freeze(obj);
  Object.keys(obj).forEach((key) => {
    if (typeof obj[key] === 'object' && obj[key] !== null && !Object.isFrozen(obj[key])) {
      deepFreeze(obj[key]);
    }
  });
}

deepFreeze(user);

```

---

### **4. Deep Cloning a Nested Object**

**Q4**: How would you create a deep copy of the `user` object? What are the limitations of using `JSON.parse(JSON.stringify(user))`?

*Expected Answer*:

```jsx
// Using JSON methods (has limitations with dates, functions, undefined values)
const deepCopy = JSON.parse(JSON.stringify(user));

// Explanation:
// JSON.parse(JSON.stringify()) does not copy functions, dates, or undefined values. For deep cloning complex objects, I would use a library like Lodash’s _.cloneDeep().

```

---

### **5. Accessing Deeply Nested Properties Safely**

**Q5**: How would you safely access the `zip` code in a deeply nested object, especially if some levels might be undefined?

```jsx
const user = {
  name: "John",
  address: {
    city: "New York",
    details: {
      zip: "10001"
    }
  }
};

```

*Expected Answer*:

```jsx
// Using optional chaining (ES2020)
const zip = user?.address?.details?.zip;
console.log(zip); // Output: "10001"

// Explanation:
// Optional chaining (`?.`) allows you to access nested properties safely without throwing an error if an intermediate property is undefined.

```

---

### **6. Comparing Two Nested Objects**

**Q6**: How would you compare two nested objects to check if they are deeply equal?

*Expected Answer*:

```jsx
// Example using Lodash for deep equality check
const _ = require('lodash');

const isEqual = _.isEqual(object1, object2);
console.log(isEqual); // true if deeply equal

// Explanation:
// Simple equality (`===`) doesn’t work for nested objects. For deep comparison, Lodash’s `_.isEqual()` method or a custom recursive comparison function can be used.

```

---

### **7. Handling References in Nested Objects**

**Q7**: Given an object `user`, if you create a new variable that points to the same object and update a nested property, how would it affect the original object?

```jsx
const user = {
  name: "John",
  address: {
    city: "New York"
  }
};

const newUser = user;
newUser.address.city = "Los Angeles";

```

*Expected Answer*:

- Since `newUser` points to the same reference as `user`, changing `newUser.address.city` also changes `user.address.city`. Both `user` and `newUser` reference the same nested object, so changes in one reflect in the other.

---

### **8. Flattening a Nested Object**

**Q8**: How would you convert a deeply nested object into a single-level object where keys are a path to the values?

```jsx
const nestedObj = {
  name: "John",
  address: {
    city: "New York",
    details: {
      zip: "10001"
    }
  }
};

```

*Expected Answer*:

```jsx
function flatten(obj, prefix = '', res = {}) {
  for (const key in obj) {
    const newKey = prefix ? `${prefix}.${key}` : key;
    if (typeof obj[key] === 'object' && obj[key] !== null) {
      flatten(obj[key], newKey, res);
    } else {
      res[newKey] = obj[key];
    }
  }
  return res;
}

console.log(flatten(nestedObj));
// Output: { "name": "John", "address.city": "New York", "address.details.zip": "10001" }

```

---

### **9. Merging Two Nested Objects**

**Q9**: Given two nested objects, how would you merge them while giving priority to properties in the second object in case of conflicts?

```jsx
const obj1 = { a: 1, b: { c: 2, d: 3 } };
const obj2 = { b: { c: 5 }, e: 4 };

```

*Expected Answer*:

```jsx
// Using Lodash merge for deep merging
const _ = require('lodash');
const mergedObj = _.merge({}, obj1, obj2);
console.log(mergedObj); // Output: { a: 1, b: { c: 5, d: 3 }, e: 4 }

// Explanation:
// Lodash’s `_.merge` performs a deep merge. Directly using `{ ...obj1, ...obj2 }` would only perform a shallow merge.

```

---

### **10. Dynamically Accessing and Modifying Nested Properties**

**Q10**: How would you dynamically access or modify a property in a nested object if the property path is given as a string, like `"address.city"`?

*Expected Answer*:

```jsx
function getNestedProperty(obj, path) {
  return path.split('.').reduce((acc, key) => (acc ? acc[key] : undefined), obj);
}

function setNestedProperty(obj, path, value) {
  const keys = path.split('.');
  const lastKey = keys.pop();
  const nestedObj = keys.reduce((acc, key) => (acc[key] = acc[key] || {}), obj);
  nestedObj[lastKey] = value;
}

const user = { address: { city: "New York" } };
console.log(getNestedProperty(user, "address.city")); // Output: "New York"

setNestedProperty(user, "address.city", "Los Angeles");
console.log(user.address.city); // Output: "Los Angeles"

```

---

These questions cover a wide range of operations and challenges involving nested objects, from basic destructuring to deep cloning, immutability, and dynamic access. Mastering these concepts will prepare you to handle various scenarios and respond effectively to follow-up questions in interviews. Let me know if you need more examples or further clarification!