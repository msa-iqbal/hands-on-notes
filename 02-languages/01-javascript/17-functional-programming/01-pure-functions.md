# Pure Functions

A pure function is a function that:

* Always returns the same output when given the same input.
* Does not produce observable side effects.
* Does not modify external state or its input arguments.

Pure functions are a fundamental concept in functional programming. They make code predictable, reusable, testable, and easier to maintain.

Example:

```javascript
function add(a, b) {
  return a + b;
}

console.log(add(10, 20)); // 30
console.log(add(10, 20)); // 30
```

The function always returns the same result for the same arguments and does not modify anything outside itself.

# 🗁 Characteristics of Pure Functions

A function is considered pure when it satisfies two important properties.

**Referential transparency**

An expression can be replaced with its resulting value without changing the program's behavior.

```javascript
function multiply(a, b) {
  return a * b;
}

const result = multiply(5, 4);

console.log(result);
```

Because `multiply(5, 4)` always returns `20`, the expression can be replaced with `20`:

```javascript
const result = 20;
```

The behavior remains the same.

**No observable side effects**

A pure function does not modify external state, mutate its arguments, perform I/O, or trigger other externally observable operations.

```javascript
function calculateArea(width, height) {
  return width * height;
}
```

This function only calculates and returns a value.

# 🗁 Pure vs Impure Functions

An impure function depends on or modifies something outside its local execution.

**Pure function**

```javascript
function calculateTotal(price, quantity) {
  return price * quantity;
}

console.log(calculateTotal(100, 3)); // 300
```

**Impure function**

```javascript
let taxRate = 0.1;

function calculateTotal(price) {
  return price + price * taxRate;
}
```

The output depends on the external `taxRate` variable.

If `taxRate` changes, the same function call can produce a different result.

```javascript
console.log(calculateTotal(100)); // 110

taxRate = 0.2;

console.log(calculateTotal(100)); // 120
```

A pure alternative receives the tax rate explicitly:

```javascript
function calculateTotal(price, taxRate) {
  return price + price * taxRate;
}

console.log(calculateTotal(100, 0.1)); // 110
console.log(calculateTotal(100, 0.2)); // 120
```

Now the function's dependencies are explicit.

# 🗁 Avoiding External State

Pure functions should not depend on mutable variables outside their scope.

**Impure example**

```javascript
let discount = 20;

function applyDiscount(price) {
  return price - discount;
}
```

The function depends on the external `discount` variable.

**Pure alternative**

```javascript
function applyDiscount(price, discount) {
  return price - discount;
}

console.log(applyDiscount(100, 20)); // 80
```

Passing dependencies as arguments makes the function easier to understand and test.

# 🗁 Avoiding Mutation

Mutating an input argument is a common cause of impure behavior.

**Impure example**

```javascript
function addRole(user) {
  user.role = "admin";

  return user;
}

const user = {
  name: "Alice"
};

const updatedUser = addRole(user);

console.log(user);
```

Output:

```javascript
{
  name: "Alice",
  role: "admin"
}
```

The original object has been modified.

**Pure alternative**

```javascript
function addRole(user) {
  return {
    ...user,
    role: "admin"
  };
}

const user = {
  name: "Alice"
};

const updatedUser = addRole(user);

console.log(user);
// { name: "Alice" }

console.log(updatedUser);
// { name: "Alice", role: "admin" }
```

The function returns a new object without modifying the original.

# 🗁 Avoiding Array Mutation

Array methods such as `push()`, `pop()`, `splice()`, `sort()`, and `reverse()` modify the original array.

A pure transformation should avoid modifying its input.

**Impure example**

```javascript
function addItem(items, item) {
  items.push(item);

  return items;
}

const numbers = [1, 2, 3];

addItem(numbers, 4);

console.log(numbers); // [1, 2, 3, 4]
```

**Pure alternative**

```javascript
function addItem(items, item) {
  return [...items, item];
}

const numbers = [1, 2, 3];

const updatedNumbers = addItem(numbers, 4);

console.log(numbers);        // [1, 2, 3]
console.log(updatedNumbers); // [1, 2, 3, 4]
```

Other non-mutating array methods include:

* `map()`
* `filter()`
* `slice()`
* `concat()`
* `flatMap()`

These methods return new arrays rather than modifying the original array.

# 🗁 Pure Functions and Object References

JavaScript objects and arrays are reference values.

Even if a function does not reassign an argument, modifying the referenced object still mutates external state.

Example:

```javascript
function updateAddress(user) {
  user.address.city = "Dhaka";

  return user;
}

const user = {
  name: "Alice",
  address: {
    city: "London"
  }
};

updateAddress(user);

console.log(user.address.city); // Dhaka
```

The nested object was modified.

A safer implementation:

```javascript
function updateAddress(user) {
  return {
    ...user,
    address: {
      ...user.address,
      city: "Dhaka"
    }
  };
}

const user = {
  name: "Alice",
  address: {
    city: "London"
  }
};

const updatedUser = updateAddress(user);

console.log(user.address.city);        // London
console.log(updatedUser.address.city); // Dhaka
```

A shallow copy alone is insufficient when nested structures must also remain unchanged.

# 🗁 Pure Functions and Side Effects

A side effect is an observable interaction with something outside a function's local computation.

Common side effects include:

* Modifying global variables.
* Mutating external objects or arrays.
* Writing to files.
* Sending HTTP requests.
* Updating the DOM.
* Writing to a database.
* Logging to the console.
* Reading the current time.
* Generating random values.

Example of an impure function:

```javascript
function saveUser(user) {
  localStorage.setItem("user", JSON.stringify(user));
}
```

This function modifies browser storage.

A pure transformation can be separated from the side effect:

```javascript
function serializeUser(user) {
  return JSON.stringify(user);
}

function saveUser(user) {
  const serializedUser = serializeUser(user);

  localStorage.setItem("user", serializedUser);
}
```

Here:

* `serializeUser()` is pure for ordinary JSON-compatible data.
* `saveUser()` performs the storage side effect.

This separation keeps the data transformation predictable while allowing the application to interact with external systems.

# 🗁 Reading External Data

Reading external state can also make a function impure because its result may change independently of its arguments.

**Reading the current time**

```javascript
function getCurrentYear() {
  return new Date().getFullYear();
}
```

The result depends on the current date.

A pure alternative:

```javascript
function getYear(date) {
  return date.getFullYear();
}

const date = new Date("2026-11-14");

console.log(getYear(date)); // 2026
```

**Generating random values**

```javascript
function generateId() {
  return Math.random();
}
```

The output can differ between calls with identical arguments.

A pure alternative receives the generated value:

```javascript
function createUserId(randomValue) {
  return `user-${randomValue}`;
}

console.log(createUserId(0.123)); // user-0.123
```

The random value can be generated outside the pure function.

# 🗁 Function Composition with Pure Functions

Pure functions can be combined to create more complex operations.

Example:

```javascript
function addTax(price) {
  return price * 1.1;
}

function applyDiscount(price) {
  return price * 0.9;
}

const finalPrice = applyDiscount(addTax(100));

console.log(finalPrice); // 99
```

Each function performs one calculation and returns a result.

The operations can be composed without introducing hidden state or mutation.

# 🗁 Pure Functions with map(), filter(), and reduce()

Array transformation methods work especially well with pure callbacks.

Example:

```javascript
const products = [
  { name: "Laptop", price: 1200 },
  { name: "Mouse", price: 40 },
  { name: "Keyboard", price: 80 }
];

const affordableProducts = products
  .filter((product) => product.price < 100)
  .map((product) => product.name);

console.log(affordableProducts);
```

Output:

```javascript
["Mouse", "Keyboard"]
```

The callbacks do not modify the original objects or depend on external state.

Aggregation example:

```javascript
const prices = [100, 200, 300];

const total = prices.reduce(
  (sum, price) => sum + price,
  0
);

console.log(total); // 600
```

Pure callbacks make transformation pipelines more predictable.

# 🗁 Referential Transparency in Practice

Referential transparency makes expressions easier to reason about.

Example:

```javascript
function square(number) {
  return number * number;
}

const result = square(5) + square(5);
```

Since `square(5)` always returns `25`, the expression can be simplified:

```javascript
const result = 25 + 25;
```

Output:

```javascript
50
```

This property helps with:

* Simplifying expressions.
* Caching calculation results.
* Testing individual operations.
* Reasoning about complex transformations.
* Optimizing programs when the optimization preserves observable behavior.

# 🗁 Testing Pure Functions

Pure functions are easy to test because their output depends only on their inputs.

Example function:

```javascript
function calculateDiscount(price, percentage) {
  return price - (price * percentage) / 100;
}
```

Simple assertions:

```javascript
console.assert(
  calculateDiscount(100, 10) === 90,
  "Expected a 10% discount"
);

console.assert(
  calculateDiscount(200, 25) === 150,
  "Expected a 25% discount"
);

console.assert(
  calculateDiscount(100, 0) === 100,
  "Expected the original price"
);
```

The tests do not need to mock databases, network requests, browser APIs, or global variables.

For production applications, dedicated testing frameworks provide more expressive assertions and reporting.

# 🗁 Pure Functions in Frontend Applications

Pure functions are valuable in frontend development, especially for transforming application state.

Example:

```javascript
function updateUserStatus(users, userId, isActive) {
  return users.map((user) => {
    if (user.id !== userId) {
      return user;
    }

    return {
      ...user,
      isActive
    };
  });
}

const users = [
  { id: 1, name: "Alice", isActive: false },
  { id: 2, name: "Bob", isActive: true }
];

const updatedUsers = updateUserStatus(users, 1, true);

console.log(users[0].isActive);        // false
console.log(updatedUsers[0].isActive); // true
```

This approach is useful in:

* React state updates.
* Redux reducers.
* Form data processing.
* API response transformations.
* Filtering and sorting UI data.
* Validation logic.

Pure state transformations make changes easier to track and debug.

# 🗁 Pure Functions in Backend Applications

Pure functions are also useful in backend development.

For example, business calculations can be separated from database operations.

```javascript
function calculateOrderTotal(items, taxRate, discount) {
  const subtotal = items.reduce(
    (total, item) => total + item.price * item.quantity,
    0
  );

  const discountedTotal = subtotal - discount;

  return discountedTotal + discountedTotal * taxRate;
}
```

Database interaction remains separate:

```javascript
async function processOrder(orderId) {
  const order = await database.orders.findById(orderId);

  const total = calculateOrderTotal(
    order.items,
    order.taxRate,
    order.discount
  );

  await database.orders.update(orderId, {
    total
  });

  return total;
}
```

The calculation is pure, while `processOrder()` performs external operations.

This separation improves unit testing and makes business rules easier to reuse.

# 🗁 When Pure Functions Are Not Practical

Not every function needs to be pure.

Applications must interact with the external world to perform useful work.

Examples include:

* Sending HTTP requests.
* Reading database records.
* Writing files.
* Updating browser storage.
* Handling user events.
* Logging application activity.

Trying to eliminate all side effects would make most applications impossible to build.

A practical approach is to isolate side effects at application boundaries and keep the core business logic as pure as reasonably possible.

Example architecture:

```text
User Request
     |
     v
External Operation
     |
     v
Input Validation
     |
     v
Pure Business Logic
     |
     v
Result
     |
     v
External Operation
     |
     v
Database / API / UI
```

This approach is often associated with functional core, imperative shell architecture.

# 🗁 Common Mistakes

**Assuming a function is pure because it returns a value**

A function can return a value and still mutate external state.

```javascript
let count = 0;

function increment() {
  count++;

  return count;
}
```

**Using const as a guarantee of immutability**

`const` prevents reassignment of a variable binding, but it does not prevent mutation of an object or array.

```javascript
const user = {
  name: "Alice"
};

user.name = "Bob"; // Allowed
```

**Making shallow copies of nested structures**

```javascript
const updatedUser = {
  ...user
};
```

Nested objects may still share references with the original.

**Confusing deterministic behavior with purity**

A function that always returns the same result may still have side effects.

For example:

```javascript
function logAndReturn() {
  console.log("Executed");

  return 10;
}
```

The return value is deterministic, but logging is an observable side effect.

**Trying to make every function pure**

External interactions are necessary. Focus on separating them from calculations and transformations.

# 🗁 Best Practices

* Keep business calculations independent of external state.
* Pass dependencies explicitly through function arguments.
* Avoid modifying input objects and arrays.
* Return new objects when transforming application data.
* Separate data transformation from I/O operations.
* Keep functions small and focused on one responsibility.
* Use pure callbacks with `map()`, `filter()`, and `reduce()`.
* Test pure functions independently.
* Isolate side effects at application boundaries.
* Avoid unnecessary abstractions that make simple logic harder to understand.

# 🗁 Summary

Pure functions are functions that return consistent results for the same inputs and do not produce observable side effects.

Important concepts include:

* Referential transparency.
* Explicit dependencies.
* Avoiding external state.
* Avoiding mutation.
* Understanding side effects.
* Immutable data transformation.
* Function composition.
* Testing pure functions.
* Separating business logic from I/O.
* Functional core and imperative shell architecture.

Pure functions provide a foundation for predictable JavaScript applications and are one of the most important building blocks of functional programming.
