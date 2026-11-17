# Immutability

Immutability is a programming concept in which data cannot be modified after it has been created.

Instead of changing existing data, immutable programming creates new values containing the required changes.

JavaScript does not enforce immutability for ordinary objects and arrays. However, developers can follow immutable programming practices using techniques such as:

* `const` declarations.
* Spread syntax.
* Array transformation methods.
* Object copying.
* `Object.freeze()`.
* Immutable data structures.
* Pure functions.

Example:

```javascript
const user = {
  name: "Alice",
  age: 25
};

const updatedUser = {
  ...user,
  age: 26
};

console.log(user.age);        // 25
console.log(updatedUser.age); // 26
```

The original object remains unchanged, and a new object contains the updated data.

# 🗁 Mutable vs Immutable Data

**Mutable data**

Mutable data can be changed after creation.

```javascript
const user = {
  name: "Alice",
  age: 25
};

user.age = 26;

console.log(user);
```

Output:

```javascript
{
  name: "Alice",
  age: 26
}
```

The existing object was modified.

**Immutable transformation**

An immutable transformation creates a new object instead of modifying the existing one.

```javascript
const user = {
  name: "Alice",
  age: 25
};

const updatedUser = {
  ...user,
  age: 26
};

console.log(user);
console.log(updatedUser);
```

Output:

```javascript
// Original
{
  name: "Alice",
  age: 25
}

// Updated
{
  name: "Alice",
  age: 26
}
```

# 🗁 Understanding const and Immutability

The `const` keyword prevents reassignment of a variable binding.

It does not make the referenced object or array immutable.

Example:

```javascript
const user = {
  name: "Alice"
};

user.name = "Bob"; // Allowed

console.log(user.name); // Bob
```

Reassigning the variable is not allowed:

```javascript
const user = {
  name: "Alice"
};

user = {
  name: "Bob"
}; // TypeError
```

The distinction is important:

* `const` protects the variable binding.
* Immutability protects the data from modification.

# 🗁 Immutability with Primitive Values

JavaScript primitive values are immutable.

Primitive types include:

* String
* Number
* BigInt
* Boolean
* Undefined
* Null
* Symbol

Example:

```javascript
let name = "Alice";

name.toUpperCase();

console.log(name); // Alice
```

The `toUpperCase()` method returns a new string instead of modifying the original string.

```javascript
const original = "hello";
const transformed = original.toUpperCase();

console.log(original);    // hello
console.log(transformed); // HELLO
```

Changing a variable to reference another primitive value does not mutate the original value.

# 🗁 Immutability with Objects

Objects are mutable by default.

Example:

```javascript
const product = {
  name: "Laptop",
  price: 1200
};

product.price = 1100;

console.log(product.price); // 1100
```

To update an object immutably, use object spread syntax.

```javascript
const product = {
  name: "Laptop",
  price: 1200
};

const updatedProduct = {
  ...product,
  price: 1100
};

console.log(product.price);        // 1200
console.log(updatedProduct.price); // 1100
```

# 🗁 Immutability with Arrays

Array methods can either mutate an array or return a new array.

Understanding the difference is essential.

**Mutating methods**

These methods modify the original array:

* `push()`
* `pop()`
* `shift()`
* `unshift()`
* `splice()`
* `sort()`
* `reverse()`
* `fill()`
* `copyWithin()`

Example:

```javascript
const numbers = [1, 2, 3];

numbers.push(4);

console.log(numbers); // [1, 2, 3, 4]
```

**Non-mutating methods**

These methods return new arrays or values without modifying the original array:

* `map()`
* `filter()`
* `slice()`
* `concat()`
* `flat()`
* `flatMap()`
* `toSorted()`
* `toReversed()`
* `toSpliced()`
* `with()`

Example:

```javascript
const numbers = [1, 2, 3];

const updatedNumbers = numbers.concat(4);

console.log(numbers);        // [1, 2, 3]
console.log(updatedNumbers); // [1, 2, 3, 4]
```

# 🗁 Adding Items Immutably

Instead of using `push()`, create a new array using spread syntax.

```javascript
const users = ["Alice", "Bob"];

const updatedUsers = [...users, "Charlie"];

console.log(users);
// ["Alice", "Bob"]

console.log(updatedUsers);
// ["Alice", "Bob", "Charlie"]
```

Adding an item at the beginning:

```javascript
const updatedUsers = ["Charlie", ...users];
```

Combining multiple arrays:

```javascript
const frontend = ["HTML", "CSS"];
const backend = ["Node.js", "Express"];

const technologies = [
  ...frontend,
  ...backend
];
```

# 🗁 Removing Items Immutably

Use `filter()` to create a new array without the selected elements.

Example:

```javascript
const users = [
  { id: 1, name: "Alice" },
  { id: 2, name: "Bob" },
  { id: 3, name: "Charlie" }
];

const updatedUsers = users.filter(
  (user) => user.id !== 2
);

console.log(updatedUsers);
```

Output:

```javascript
[
  { id: 1, name: "Alice" },
  { id: 3, name: "Charlie" }
]
```

The original array remains unchanged.

# 🗁 Updating Items Immutably

Use `map()` to create a new array with an updated element.

Example:

```javascript
const users = [
  { id: 1, name: "Alice", active: false },
  { id: 2, name: "Bob", active: true }
];

const updatedUsers = users.map((user) => {
  if (user.id === 1) {
    return {
      ...user,
      active: true
    };
  }

  return user;
});

console.log(updatedUsers);
```

Output:

```javascript
[
  { id: 1, name: "Alice", active: true },
  { id: 2, name: "Bob", active: true }
]
```

Only the matching object is replaced. The other object reference is preserved.

# 🗁 Sorting Arrays Immutably

The `sort()` method modifies the original array.

Incorrect when the original must remain unchanged:

```javascript
const numbers = [30, 10, 20];

numbers.sort((a, b) => a - b);

console.log(numbers); // [10, 20, 30]
```

Use a shallow copy:

```javascript
const numbers = [30, 10, 20];

const sortedNumbers = [...numbers].sort(
  (a, b) => a - b
);

console.log(numbers);       // [30, 10, 20]
console.log(sortedNumbers); // [10, 20, 30]
```

Modern JavaScript also provides `toSorted()`:

```javascript
const numbers = [30, 10, 20];

const sortedNumbers = numbers.toSorted(
  (a, b) => a - b
);
```

`toSorted()` returns a new sorted array without modifying the original.

# 🗁 Copying Objects with Spread Syntax

Object spread creates a shallow copy of an object.

Example:

```javascript
const original = {
  name: "Alice",
  age: 25
};

const copy = {
  ...original
};

console.log(copy);
```

The copied object has the same properties, but it is a different object reference.

```javascript
console.log(original === copy); // false
```

However, nested objects are still shared.

# 🗁 Understanding Shallow Copy

A shallow copy duplicates only the top-level structure.

Nested objects and arrays retain their original references.

Example:

```javascript
const original = {
  name: "Alice",
  address: {
    city: "London"
  }
};

const copy = {
  ...original
};

console.log(original === copy); // false

console.log(
  original.address === copy.address
); // true
```

Changing the nested object through the copy also affects the original.

```javascript
copy.address.city = "Dhaka";

console.log(original.address.city); // Dhaka
```

This is one of the most common mistakes when implementing immutable updates.

# 🗁 Understanding Deep Copy

A deep copy duplicates nested objects and arrays so that the copied structure does not share mutable nested references with the original.

Modern JavaScript provides `structuredClone()` for many common data types.

Example:

```javascript
const original = {
  name: "Alice",
  address: {
    city: "London"
  },
  skills: ["JavaScript", "React"]
};

const copy = structuredClone(original);

copy.address.city = "Dhaka";
copy.skills.push("Node.js");

console.log(original);
console.log(copy);
```

The original nested structures remain unchanged.

`structuredClone()` supports many structured-cloneable values, including circular references, but it does not support every JavaScript value. For example, functions and DOM nodes cannot be cloned this way.

# 🗁 Object.freeze()

`Object.freeze()` prevents an object's own properties from being added, removed, or reassigned.

Example:

```javascript
const user = {
  name: "Alice",
  age: 25
};

Object.freeze(user);

user.age = 30;

console.log(user.age); // 25
```

In strict mode, attempting to modify a frozen property can throw a `TypeError`.

Important characteristics:

* Prevents adding properties.
* Prevents removing properties.
* Prevents changing existing property values.
* Prevents changing property descriptors.
* Returns the same object reference.

Check whether an object is frozen:

```javascript
console.log(Object.isFrozen(user)); // true
```

# 🗁 Object.seal()

`Object.seal()` prevents adding or removing properties, but existing writable properties can still be modified.

Example:

```javascript
const user = {
  name: "Alice",
  age: 25
};

Object.seal(user);

user.age = 30;       // Allowed
user.role = "admin"; // Not allowed
delete user.name;    // Not allowed

console.log(user);
```

Check whether an object is sealed:

```javascript
console.log(Object.isSealed(user)); // true
```

# 🗁 Object.preventExtensions()

`Object.preventExtensions()` prevents new properties from being added to an object.

However, existing properties can still be modified or deleted when their descriptors permit it.

Example:

```javascript
const user = {
  name: "Alice",
  age: 25
};

Object.preventExtensions(user);

user.age = 30;       // Allowed
delete user.name;    // Allowed
user.role = "admin"; // Not allowed
```

Check whether an object is extensible:

```javascript
console.log(Object.isExtensible(user)); // false
```

# 🗁 Comparing Object.freeze(), Object.seal(), and Object.preventExtensions()

| Operation | preventExtensions() | seal() | freeze() |
| --- | --- | --- | --- |
| Add properties | No | No | No |
| Delete properties | Yes, if configurable | No | No |
| Modify writable properties | Yes | Yes | No |
| Change property descriptors | Some changes allowed | Restricted | No |

All three methods affect the object itself, not its nested objects.

# 🗁 Shallow Freeze vs Deep Freeze

`Object.freeze()` is shallow.

Example:

```javascript
const user = {
  name: "Alice",
  address: {
    city: "London"
  }
};

Object.freeze(user);

user.name = "Bob"; // Not allowed

user.address.city = "Dhaka"; // Allowed

console.log(user.address.city); // Dhaka
```

The nested `address` object was not frozen.

A recursive deep-freeze helper can freeze nested objects:

```javascript
function deepFreeze(value) {
  if (
    value === null ||
    typeof value !== "object" ||
    Object.isFrozen(value)
  ) {
    return value;
  }

  for (const key of Reflect.ownKeys(value)) {
    deepFreeze(value[key]);
  }

  return Object.freeze(value);
}
```

Usage:

```javascript
const user = {
  name: "Alice",
  address: {
    city: "London"
  }
};

deepFreeze(user);

user.address.city = "Dhaka";

console.log(user.address.city); // London
```

This helper is intended for ordinary object graphs. Specialized objects, accessors, proxies, and other unusual structures may require additional handling.

# 🗁 Updating Nested Objects Immutably

Nested data structures require copying each level that needs to change.

Example:

```javascript
const user = {
  id: 1,
  name: "Alice",
  profile: {
    address: {
      city: "London",
      country: "UK"
    }
  }
};
```

Suppose the city must change.

An immutable update:

```javascript
const updatedUser = {
  ...user,
  profile: {
    ...user.profile,
    address: {
      ...user.profile.address,
      city: "Dhaka"
    }
  }
};

console.log(user.profile.address.city);        // London
console.log(updatedUser.profile.address.city); // Dhaka
```

Every level between the root object and the changed property receives a new reference.

# 🗁 Structural Sharing

Structural sharing means reusing unchanged parts of a data structure while creating new references only for the parts that change.

Example:

```javascript
const original = {
  name: "Alice",
  profile: {
    city: "London"
  },
  preferences: {
    theme: "dark"
  }
};

const updated = {
  ...original,
  profile: {
    ...original.profile,
    city: "Dhaka"
  }
};

console.log(original === updated); // false

console.log(
  original.profile === updated.profile
); // false

console.log(
  original.preferences === updated.preferences
); // true
```

The unchanged `preferences` object is safely reused because it was not modified.

Structural sharing can reduce unnecessary copying and improve performance in state management systems.

# 🗁 Immutability with Object.assign()

`Object.assign()` can create a shallow copy by using an empty object as the target.

Example:

```javascript
const user = {
  name: "Alice",
  age: 25
};

const updatedUser = Object.assign({}, user, {
  age: 26
});

console.log(user.age);        // 25
console.log(updatedUser.age); // 26
```

Remember that `Object.assign()` mutates its target object. Passing a new empty object avoids modifying the original object in this example.

# 🗁 Modern Non-Mutating Array Methods

Modern JavaScript provides methods that return modified copies instead of mutating the original array.

**toSorted()**

```javascript
const numbers = [30, 10, 20];

const sorted = numbers.toSorted(
  (a, b) => a - b
);

console.log(numbers); // [30, 10, 20]
console.log(sorted);  // [10, 20, 30]
```

**toReversed()**

```javascript
const numbers = [1, 2, 3];

const reversed = numbers.toReversed();

console.log(numbers);  // [1, 2, 3]
console.log(reversed); // [3, 2, 1]
```

**toSpliced()**

```javascript
const numbers = [1, 2, 3, 4];

const updated = numbers.toSpliced(1, 2, 10);

console.log(numbers); // [1, 2, 3, 4]
console.log(updated); // [1, 10, 4]
```

**with()**

```javascript
const numbers = [10, 20, 30];

const updated = numbers.with(1, 99);

console.log(numbers); // [10, 20, 30]
console.log(updated); // [10, 99, 30]
```

These methods are useful when an immutable update is required.

# 🗁 Immutability in React Applications

React state should be updated immutably.

Incorrect:

```javascript
const [user, setUser] = useState({
  name: "Alice",
  age: 25
});

user.age = 26;

setUser(user);
```

This mutates the existing object and passes the same reference back to the state setter.

Correct:

```javascript
const [user, setUser] = useState({
  name: "Alice",
  age: 25
});

setUser((previousUser) => ({
  ...previousUser,
  age: 26
}));
```

For arrays:

```javascript
const [items, setItems] = useState([
  "HTML",
  "CSS"
]);

setItems((previousItems) => [
  ...previousItems,
  "JavaScript"
]);
```

Immutable updates help React detect state changes through reference comparisons and make rendering behavior more predictable.

# 🗁 Immutability in Redux

Redux encourages immutable state updates.

Example reducer:

```javascript
function counterReducer(state = { count: 0 }, action) {
  switch (action.type) {
    case "increment":
      return {
        ...state,
        count: state.count + 1
      };

    case "decrement":
      return {
        ...state,
        count: state.count - 1
      };

    default:
      return state;
  }
}
```

Each update returns a new state object instead of modifying the existing state.

Redux Toolkit uses Immer internally to allow mutation-like syntax while producing immutable state updates.

# 🗁 Immutability and Performance

Immutability provides several performance advantages:

* Reference comparisons can detect changes efficiently.
* Unchanged objects can be reused through structural sharing.
* Memoization becomes easier.
* State changes become easier to track.
* Predictable updates can reduce unnecessary computations.

However, immutability is not automatically faster.

Creating large copies repeatedly can consume memory and CPU time. For large or deeply nested structures, consider structural sharing, carefully designed data models, or specialized immutable data structures.

# 🗁 Common Mistakes

**Assuming const makes objects immutable**

`const` prevents reassignment, not property modification.

**Using shallow copies for deeply nested updates**

Spread syntax does not recursively clone nested objects.

**Mutating arrays accidentally**

Methods such as `sort()` and `splice()` modify the original array.

**Assuming Object.freeze() freezes nested objects**

`Object.freeze()` only freezes the specified object.

**Deep-cloning everything**

Deep cloning can be expensive and may be unnecessary. Prefer copying only the parts that need to change.

**Confusing immutability with serialization**

Creating a JSON string does not automatically make the original object immutable.

# 🗁 Best Practices

* Prefer immutable updates for application state.
* Use `const` for bindings that should not be reassigned.
* Use spread syntax for simple object and array copies.
* Use non-mutating array methods when available.
* Copy every nested level that needs to change.
* Reuse unchanged references through structural sharing.
* Use `Object.freeze()` when runtime protection is useful.
* Understand the limitations of shallow and deep copying.
* Avoid unnecessary deep cloning.
* Keep data transformations predictable and easy to test.
* Consider specialized immutable data structures for demanding use cases.

# 🗁 Summary

Immutability means preserving existing data instead of modifying it directly.

Important concepts include:

* Mutable and immutable values.
* The difference between `const` and immutability.
* Immutable object and array updates.
* Shallow and deep copying.
* `Object.freeze()`, `Object.seal()`, and `Object.preventExtensions()`.
* Nested immutable transformations.
* Structural sharing.
* Modern non-mutating array methods.
* Immutability in React and Redux.
* Performance considerations.

Immutability is a key foundation of functional programming and helps developers build predictable, maintainable JavaScript applications.
