# Functional Programming Patterns

Functional programming is a programming paradigm that emphasizes functions, immutability, declarative code, and predictable data transformations.

JavaScript supports functional programming through first-class functions, closures, higher-order functions, callbacks, and array methods.

Functional programming patterns provide reusable approaches to structuring application logic.

Common patterns include:

* Pure functions.
* Immutability.
* Function composition.
* Currying.
* Partial application.
* Higher-order functions.
* Declarative transformations.
* Function pipelines.
* Point-free programming.
* Memoization.
* Recursion.
* Functors and monads.

# 🗁 Pure Function Pattern

A pure function always produces the same output for the same input and does not cause observable side effects.

Example:

```javascript
function calculateTotal(price, quantity) {
  return price * quantity;
}

console.log(calculateTotal(100, 3)); // 300
console.log(calculateTotal(100, 3)); // 300
```

The function depends only on its arguments.

An impure function:

```javascript
let taxRate = 0.15;

function calculateTax(price) {
  return price * taxRate;
}
```

The result depends on external state.

If `taxRate` changes, the same input can produce a different result.

Pure functions are easier to test, reuse, and reason about.

# 🗁 Immutability Pattern

Immutability means avoiding modifications to existing data and creating new values when changes are required.

Mutable approach:

```javascript
const user = {
  name: "Alice",
  age: 25
};

user.age = 26;

console.log(user);
```

Immutable approach:

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

Array example:

```javascript
const numbers = [1, 2, 3];

const updatedNumbers = [...numbers, 4];

console.log(numbers);        // [1, 2, 3]
console.log(updatedNumbers); // [1, 2, 3, 4]
```

Immutability helps prevent unexpected state changes and makes application behavior easier to track.

# 🗁 Higher-Order Function Pattern

A higher-order function accepts another function as an argument, returns a function, or both.

Example:

```javascript
function operate(a, b, operation) {
  return operation(a, b);
}

const add = (a, b) => a + b;
const multiply = (a, b) => a * b;

console.log(operate(5, 3, add));      // 8
console.log(operate(5, 3, multiply)); // 15
```

The `operate()` function delegates the operation to another function.

Higher-order functions are commonly used in array transformations, event handling, middleware, and reusable utilities.

# 🗁 Function Composition Pattern

Function composition combines smaller functions into a larger operation.

Example:

```javascript
const trim = (value) => value.trim();

const lowercase = (value) => value.toLowerCase();

const addPrefix = (value) => `user-${value}`;

const pipe = (...functions) => (value) =>
  functions.reduce((result, fn) => fn(result), value);

const normalizeUsername = pipe(
  trim,
  lowercase,
  addPrefix
);

console.log(normalizeUsername(" Alice "));
// user-alice
```

Each function performs one transformation.

The pipeline combines them into a reusable operation.

# 🗁 Currying Pattern

Currying transforms a multi-argument function into a sequence of single-argument functions.

Example:

```javascript
const multiply = (a) => (b) => a * b;

const double = multiply(2);
const triple = multiply(3);

console.log(double(5)); // 10
console.log(triple(5)); // 15
```

Currying allows functions to be configured and reused with different inputs.

# 🗁 Partial Application Pattern

Partial application fixes some arguments of a function and returns a new function that accepts the remaining arguments.

Example:

```javascript
function calculateDiscount(price, discountRate) {
  return price * (1 - discountRate);
}

const applyDiscount = (discountRate) => (price) =>
  calculateDiscount(price, discountRate);

const applyTenPercentDiscount = applyDiscount(0.1);
const applyTwentyPercentDiscount = applyDiscount(0.2);

console.log(applyTenPercentDiscount(100));    // 90
console.log(applyTwentyPercentDiscount(100)); // 80
```

This pattern is useful when several operations share the same configuration.

# 🗁 Declarative Programming Pattern

Declarative programming describes what should happen rather than specifying every step of how to accomplish it.

Imperative approach:

```javascript
const numbers = [1, 2, 3, 4, 5];

const evenNumbers = [];

for (const number of numbers) {
  if (number % 2 === 0) {
    evenNumbers.push(number);
  }
}

console.log(evenNumbers); // [2, 4]
```

Declarative approach:

```javascript
const numbers = [1, 2, 3, 4, 5];

const evenNumbers = numbers.filter(
  (number) => number % 2 === 0
);

console.log(evenNumbers); // [2, 4]
```

The declarative version expresses the intended transformation directly.

# 🗁 Data Transformation Pattern

Functional programming often transforms data through a sequence of operations.

Common array methods include:

* `map()` — transform each element.
* `filter()` — select matching elements.
* `reduce()` — accumulate a result.
* `find()` — retrieve the first matching element.
* `some()` — check whether at least one element matches.
* `every()` — check whether all elements match.

Example:

```javascript
const products = [
  { name: "Laptop", price: 1200, active: true },
  { name: "Mouse", price: 40, active: true },
  { name: "Keyboard", price: 80, active: false },
  { name: "Monitor", price: 300, active: true }
];

const result = products
  .filter((product) => product.active)
  .filter((product) => product.price < 500)
  .map((product) => product.name);

console.log(result);
// ["Mouse", "Monitor"]
```

Each operation produces a new array, allowing transformations to be chained.

# 🗁 Function Pipeline Pattern

A function pipeline passes a value through multiple operations in a defined order.

Example:

```javascript
const pipe = (...functions) => (value) =>
  functions.reduce(
    (result, fn) => fn(result),
    value
  );

const addTax = (price) => price * 1.15;

const applyDiscount = (price) => price * 0.9;

const roundPrice = (price) => Math.round(price * 100) / 100;

const calculateFinalPrice = pipe(
  addTax,
  applyDiscount,
  roundPrice
);

console.log(calculateFinalPrice(100)); // 103.5
```

Execution:

```text
100
 |
 v
addTax
 |
 v
115
 |
 v
applyDiscount
 |
 v
103.5
 |
 v
roundPrice
 |
 v
103.5
```

Pipelines make sequential data processing easier to understand.

# 🗁 Point-Free Programming Pattern

Point-free programming defines functions without explicitly mentioning the arguments they receive.

It focuses on combining functions rather than describing their input values.

Traditional implementation:

```javascript
const normalize = (value) => value.trim().toLowerCase();
```

Point-free style:

```javascript
const pipe = (...functions) => (value) =>
  functions.reduce((result, fn) => fn(result), value);

const trim = (value) => value.trim();

const lowercase = (value) => value.toLowerCase();

const normalize = pipe(trim, lowercase);

console.log(normalize(" Alice ")); // alice
```

The `normalize` function is defined by composing existing functions without explicitly declaring its argument.

Point-free programming can make transformation pipelines concise, but excessive abstraction can reduce readability.

# 🗁 Memoization Pattern

Memoization stores the results of previous function calls so that repeated inputs can reuse previously calculated results.

Example:

```javascript
function memoize(fn) {
  const cache = new Map();

  return function (...args) {
    const key = JSON.stringify(args);

    if (cache.has(key)) {
      return cache.get(key);
    }

    const result = fn(...args);

    cache.set(key, result);

    return result;
  };
}
```

Usage:

```javascript
function calculateSquare(number) {
  console.log("Calculating...");

  return number * number;
}

const memoizedSquare = memoize(calculateSquare);

console.log(memoizedSquare(5));
// Calculating...
// 25

console.log(memoizedSquare(5));
// 25
```

The second call uses the cached result.

This basic implementation is suitable for simple examples. Production memoization requires careful consideration of cache keys, object identity, memory usage, and invalidation.

Memoization is especially useful for expensive deterministic calculations.

# 🗁 Recursion Pattern

Recursion occurs when a function calls itself to solve a problem by reducing it into smaller subproblems.

Example:

```javascript
function factorial(number) {
  if (number <= 1) {
    return 1;
  }

  return number * factorial(number - 1);
}

console.log(factorial(5)); // 120
```

Every recursive function needs:

* A base case that stops recursion.
* A recursive case that moves toward the base case.

Recursion is useful for tree traversal, nested structures, and divide-and-conquer algorithms.

For large inputs, consider iteration or techniques that avoid excessive call-stack usage.

# 🗁 Closure-Based Configuration Pattern

Closures allow functions to retain access to variables from their lexical environment.

Example:

```javascript
function createCounter(initialValue = 0) {
  let count = initialValue;

  return {
    increment() {
      count++;
      return count;
    },

    decrement() {
      count--;
      return count;
    },

    getValue() {
      return count;
    }
  };
}

const counter = createCounter(10);

console.log(counter.increment()); // 11
console.log(counter.increment()); // 12
console.log(counter.decrement()); // 11
console.log(counter.getValue());  // 11
```

The returned methods retain access to `count`, even after `createCounter()` has finished executing.

Closures are useful for encapsulation, state management, and creating configurable functions.

# 🗁 Functor Pattern

A functor is an abstraction that supports mapping a function over a value inside a context while preserving the context's structure.

A simple JavaScript example uses arrays.

```javascript
const numbers = [1, 2, 3];

const doubled = numbers.map(
  (number) => number * 2
);

console.log(doubled); // [2, 4, 6]
```

The array remains an array, but its values are transformed.

A simplified custom container:

```javascript
const Box = (value) => ({
  value,

  map(fn) {
    return Box(fn(value));
  }
});

const result = Box(5)
  .map((number) => number * 2)
  .map((number) => number + 1);

console.log(result.value); // 11
```

The `map()` operation transforms the contained value while keeping it inside the `Box` context.

This example illustrates the basic mapping idea rather than a complete formal type-class implementation.

# 🗁 Monad Pattern

A monad is an abstraction for sequencing computations that produce values within a context.

Promises provide a familiar JavaScript example.

```javascript
const getUser = (id) =>
  Promise.resolve({
    id,
    name: "Alice"
  });

const getUserEmail = (user) =>
  Promise.resolve({
    ...user,
    email: "alice@example.com"
  });

getUser(101)
  .then(getUserEmail)
  .then(console.log);
```

Each asynchronous operation returns a Promise.

The `then()` method allows the next operation to work with the resolved value while preserving Promise-based control flow.

Monads are a deeper functional programming concept. Understanding them requires learning about mapping, chaining, and flattening nested computational contexts.

# 🗁 Either Pattern

The Either pattern represents a computation that can produce one of two possible outcomes.

A common use is separating successful results from failures without immediately throwing exceptions.

Example:

```javascript
const Right = (value) => ({
  value,

  map(fn) {
    return Right(fn(value));
  },

  fold(onLeft, onRight) {
    return onRight(value);
  }
});

const Left = (error) => ({
  value: error,

  map() {
    return Left(error);
  },

  fold(onLeft) {
    return onLeft(error);
  }
});
```

Usage:

```javascript
function divide(a, b) {
  if (b === 0) {
    return Left("Division by zero");
  }

  return Right(a / b);
}

const result = divide(10, 2);

const output = result.fold(
  (error) => `Error: ${error}`,
  (value) => `Result: ${value}`
);

console.log(output); // Result: 5
```

The `Left` branch represents failure, while the `Right` branch represents success.

This is a simplified Either implementation. Production implementations often provide additional operations such as chaining, error mapping, and interoperability with other functional abstractions.

# 🗁 Functional Error Handling Pattern

Functional code can represent errors as explicit return values instead of relying exclusively on exceptions.

Example:

```javascript
function parseAge(value) {
  const age = Number(value);

  if (!Number.isInteger(age) || age < 0) {
    return {
      success: false,
      error: "Invalid age"
    };
  }

  return {
    success: true,
    data: age
  };
}
```

Usage:

```javascript
const result = parseAge("25");

if (result.success) {
  console.log(result.data);
} else {
  console.error(result.error);
}
```

This pattern makes expected failure conditions explicit.

It is useful for validation, parsing, and business logic where errors are normal outcomes rather than exceptional programming failures.

# 🗁 Functional Programming in JavaScript Applications

Functional patterns are useful across different layers of an application.

**Frontend development**

* Transform API responses.
* Derive UI state.
* Create reusable event handlers.
* Validate form data.
* Process collections.
* Avoid unnecessary state mutation.

**Backend development**

* Transform request payloads.
* Implement validation pipelines.
* Compose business rules.
* Build middleware.
* Process database results.
* Handle asynchronous operations.

**Software architecture**

* Separate pure business logic from infrastructure.
* Build reusable transformation modules.
* Reduce hidden dependencies.
* Improve unit testing.
* Make data flow easier to understand.

# 🗁 Practical Example: Order Processing Pipeline

Consider an order-processing workflow.

```javascript
const order = {
  id: 101,
  items: [
    { name: "Laptop", price: 1000, quantity: 1 },
    { name: "Mouse", price: 50, quantity: 2 }
  ],
  discountRate: 0.1,
  taxRate: 0.15
};
```

Define pure transformation functions:

```javascript
const calculateSubtotal = (order) => ({
  ...order,
  subtotal: order.items.reduce(
    (total, item) => total + item.price * item.quantity,
    0
  )
});

const calculateDiscount = (order) => ({
  ...order,
  discount: order.subtotal * order.discountRate
});

const calculateTax = (order) => ({
  ...order,
  tax: (order.subtotal - order.discount) * order.taxRate
});

const calculateTotal = (order) => ({
  ...order,
  total: order.subtotal - order.discount + order.tax
});

const selectSummary = (order) => ({
  id: order.id,
  subtotal: order.subtotal,
  discount: order.discount,
  tax: order.tax,
  total: order.total
});
```

Create a pipeline:

```javascript
const pipe = (...functions) => (value) =>
  functions.reduce((result, fn) => fn(result), value);

const processOrder = pipe(
  calculateSubtotal,
  calculateDiscount,
  calculateTax,
  calculateTotal,
  selectSummary
);

const result = processOrder(order);

console.log(result);
```

Output:

```javascript
{
  id: 101,
  subtotal: 1100,
  discount: 110,
  tax: 148.5,
  total: 1138.5
}
```

The original order remains unchanged.

Each transformation has one responsibility, and the complete business operation is assembled through composition.

# 🗁 Functional Programming vs Object-Oriented Programming

Functional programming and object-oriented programming are different approaches to organizing software.

| Functional Programming | Object-Oriented Programming |
| --- | --- |
| Organizes logic around functions | Organizes logic around objects |
| Emphasizes immutable data | Often models mutable object state |
| Uses function composition | Uses object composition and inheritance |
| Encourages pure functions | Encapsulates state and behavior |
| Focuses on data transformations | Focuses on object responsibilities |
| Commonly uses higher-order functions | Commonly uses methods and interfaces |

JavaScript supports both paradigms.

They can also be combined in the same application.

For example, a class can encapsulate application state while pure functions handle data transformations.

# 🗁 Best Practices

* Prefer pure functions for business logic.
* Avoid unnecessary mutation of shared state.
* Keep functions small and focused.
* Use composition to combine related operations.
* Use higher-order functions for reusable behavior.
* Prefer declarative transformations when they improve clarity.
* Handle asynchronous operations explicitly.
* Keep side effects at well-defined application boundaries.
* Avoid excessive abstraction and unnecessary functional patterns.
* Test individual transformations independently.
* Choose the programming style that best fits the problem.

# 🗁 Summary

Functional programming patterns provide reusable techniques for structuring JavaScript applications around functions and predictable data transformations.

Important patterns include:

* Pure functions.
* Immutability.
* Higher-order functions.
* Function composition.
* Currying.
* Partial application.
* Declarative programming.
* Function pipelines.
* Point-free programming.
* Memoization.
* Recursion.
* Closures.
* Functors.
* Monads.
* Either-based error handling.

Understanding these patterns helps developers write modular, reusable, testable, and maintainable JavaScript code.
