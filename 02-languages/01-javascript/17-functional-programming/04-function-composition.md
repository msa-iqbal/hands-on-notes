# Function Composition

Function composition is a functional programming technique in which multiple functions are combined to create a new function.

The output of one function becomes the input of another function.

Instead of writing one large function that performs several operations, composition allows developers to build complex behavior from smaller, reusable functions.

Example:

```javascript
function double(number) {
  return number * 2;
}

function increment(number) {
  return number + 1;
}

const result = double(increment(5));

console.log(result); // 12
```

Execution:

```text
5
 |
 v
increment(5) -> 6
 |
 v
double(6) -> 12
```

The result of `increment()` becomes the input of `double()`.

# 🗁 Understanding Function Composition

Suppose there are two functions:

```javascript
function first(value) {
  return value + 10;
}

function second(value) {
  return value * 2;
}
```

They can be combined manually:

```javascript
const result = second(first(5));

console.log(result); // 30
```

Mathematically, composition is represented using the symbol `∘`.

If:

\[
f(x) = x + 10
\]

\[
g(x) = x \times 2
\]

Then:

\[
(g \circ f)(x) = g(f(x))
\]

For an input of `5`:

\[
g(f(5)) = g(15) = 30
\]

The rightmost function executes first.

# 🗁 Composition Order

Function composition follows a specific execution order.

Consider:

```javascript
function addFive(number) {
  return number + 5;
}

function multiplyByTwo(number) {
  return number * 2;
}

const result = multiplyByTwo(addFive(10));

console.log(result); // 30
```

Execution order:

* `addFive(10)` returns `15`.
* `multiplyByTwo(15)` returns `30`.

Reversing the order produces a different result:

```javascript
const result = addFive(multiplyByTwo(10));

console.log(result); // 25
```

The order matters because function composition is generally not commutative.

# 🗁 Creating a compose() Function

A `compose()` utility combines functions and executes them from right to left.

Implementation:

```javascript
function compose(...functions) {
  return function (value) {
    return functions.reduceRight(
      (result, currentFunction) => {
        return currentFunction(result);
      },
      value
    );
  };
}
```

Usage:

```javascript
const addFive = (number) => number + 5;
const multiplyByTwo = (number) => number * 2;
const square = (number) => number ** 2;

const calculate = compose(
  square,
  multiplyByTwo,
  addFive
);

console.log(calculate(5)); // 400
```

Execution:

```text
5
 |
 v
addFive(5)       -> 10
 |
 v
multiplyByTwo(10) -> 20
 |
 v
square(20)       -> 400
```

The first function written in `compose()` executes last.

The implementation uses `reduceRight()` because functions must be applied from right to left.

# 🗁 Creating a pipe() Function

A `pipe()` utility works like `compose()`, but executes functions from left to right.

Implementation:

```javascript
function pipe(...functions) {
  return function (value) {
    return functions.reduce(
      (result, currentFunction) => {
        return currentFunction(result);
      },
      value
    );
  };
}
```

Usage:

```javascript
const addFive = (number) => number + 5;
const multiplyByTwo = (number) => number * 2;
const square = (number) => number ** 2;

const calculate = pipe(
  addFive,
  multiplyByTwo,
  square
);

console.log(calculate(5)); // 400
```

Execution:

```text
5
 |
 v
addFive(5)        -> 10
 |
 v
multiplyByTwo(10) -> 20
 |
 v
square(20)        -> 400
```

The first function written in `pipe()` executes first.

# 🗁 compose() vs pipe()

| Feature | compose() | pipe() |
| --- | --- | --- |
| Execution order | Right to left | Left to right |
| First listed function | Executes last | Executes first |
| Implementation | reduceRight() | reduce() |
| Reading direction | Inside-out | Top-to-bottom |

Both utilities solve the same problem. The main difference is execution order.

# 🗁 Composing Multiple Functions

Function composition becomes particularly useful when many small operations need to be combined.

Example:

```javascript
const trim = (value) => value.trim();

const lowercase = (value) => value.toLowerCase();

const removeSpaces = (value) => value.replace(/\s+/g, "");

const normalizeUsername = pipe(
  trim,
  lowercase,
  removeSpaces
);

console.log(normalizeUsername("  Alice Smith  "));
// alicesmith
```

Each function performs one specific transformation.

The final function combines these operations into a reusable pipeline.

# 🗁 Composition with String Transformations

Example:

```javascript
const trim = (value) => value.trim();

const uppercase = (value) => value.toUpperCase();

const addPrefix = (value) => `USER: ${value}`;

const formatName = pipe(
  trim,
  uppercase,
  addPrefix
);

console.log(formatName(" Alice "));
// USER: ALICE
```

This approach is useful for:

* Input normalization.
* Text formatting.
* Data sanitization.
* Display transformations.
* Reusable string processing.

# 🗁 Composition with Array Transformations

Array methods can be combined into reusable data-processing pipelines.

Example:

```javascript
const products = [
  { name: "Laptop", price: 1200, active: true },
  { name: "Mouse", price: 40, active: true },
  { name: "Keyboard", price: 80, active: false },
  { name: "Monitor", price: 300, active: true }
];

const getActiveProducts = (items) =>
  items.filter((item) => item.active);

const getAffordableProducts = (items) =>
  items.filter((item) => item.price < 500);

const getProductNames = (items) =>
  items.map((item) => item.name);

const processProducts = pipe(
  getActiveProducts,
  getAffordableProducts,
  getProductNames
);

console.log(processProducts(products));
```

Output:

```javascript
["Mouse", "Monitor"]
```

Each function handles a separate transformation.

# 🗁 Composition with Objects

Functions can be composed to transform object properties.

Example:

```javascript
const getFullName = (user) =>
  `${user.firstName} ${user.lastName}`;

const uppercase = (value) =>
  value.toUpperCase();

const addGreeting = (value) =>
  `Hello, ${value}!`;

const formatGreeting = pipe(
  getFullName,
  uppercase,
  addGreeting
);

const user = {
  firstName: "Alice",
  lastName: "Smith"
};

console.log(formatGreeting(user));
// Hello, ALICE SMITH!
```

The first function accepts an object, while subsequent functions operate on the returned string.

Composition does not require every function to accept the same kind of input. However, each function's output must be compatible with the next function's input.

# 🗁 Unary Functions and Composition

A unary function accepts exactly one argument.

Example:

```javascript
const double = (number) => number * 2;

const increment = (number) => number + 1;
```

Unary functions are especially convenient for composition because each function receives the previous function's output as its single input.

For example:

```javascript
const calculate = pipe(
  increment,
  double
);

console.log(calculate(5)); // 12
```

Functions with multiple arguments can still participate in composition, but they usually need to be adapted.

# 🗁 Adapting Multiple Arguments for Composition

Suppose a function accepts two arguments:

```javascript
function multiply(a, b) {
  return a * b;
}
```

A basic `pipe()` implementation passes one value to each function.

One solution is to adapt the function:

```javascript
const multiplyBy = (factor) => {
  return (number) => multiply(number, factor);
};

const calculate = pipe(
  (number) => number + 5,
  multiplyBy(3)
);

console.log(calculate(5)); // 30
```

The `multiplyBy()` function returns a unary function that can participate in the pipeline.

This technique is closely related to currying and partial application.

# 🗁 Composition and Partial Application

Partial application creates a new function by fixing some arguments of an existing function.

Example:

```javascript
function calculateTax(price, taxRate) {
  return price * taxRate;
}

const calculateVAT = (price) =>
  calculateTax(price, 0.15);

console.log(calculateVAT(100)); // 15
```

The tax rate is fixed in advance, producing a simpler function.

That specialized function can then participate in a composition pipeline.

```javascript
const addTax = (price) => price + calculateVAT(price);

const applyDiscount = (price) => price * 0.9;

const finalPrice = pipe(
  addTax,
  applyDiscount
);

console.log(finalPrice(100)); // 103.5
```

# 🗁 Composition with Function Factories

Function factories can create reusable functions for composition.

Example:

```javascript
const multiplyBy = (factor) =>
  (number) => number * factor;

const add = (amount) =>
  (number) => number + amount;

const calculate = pipe(
  add(10),
  multiplyBy(2)
);

console.log(calculate(5)); // 30
```

The generated functions are configured before the pipeline executes.

# 🗁 Composition with Conditional Logic

Conditional transformations can also participate in a pipeline.

Example:

```javascript
const normalize = (value) => value.trim().toLowerCase();

const validate = (value) => {
  if (value.length < 3) {
    throw new Error("Value is too short");
  }

  return value;
};

const format = (value) => `user-${value}`;

const processUsername = pipe(
  normalize,
  validate,
  format
);

console.log(processUsername(" Alice "));
// user-alice
```

If validation throws an error, the pipeline stops and the error propagates to the caller.

Composition does not automatically handle errors. Error-handling behavior must be designed explicitly.

# 🗁 Composition with Asynchronous Functions

Ordinary `compose()` and `pipe()` implementations work with synchronous functions.

When a function returns a Promise, the next function receives that Promise unless the pipeline explicitly waits for it.

Example:

```javascript
const fetchUser = async (id) => {
  const response = await fetch(`/api/users/${id}`);

  return response.json();
};

const getUserName = (user) => user.name;
```

A Promise-aware pipe can use `await` between operations.

```javascript
function pipeAsync(...functions) {
  return function (value) {
    return functions.reduce(
      (promise, currentFunction) => {
        return promise.then(currentFunction);
      },
      Promise.resolve(value)
    );
  };
}
```

Usage:

```javascript
const getName = pipeAsync(
  fetchUser,
  getUserName
);

getName(101)
  .then(console.log)
  .catch(console.error);
```

Execution:

```text
User ID
   |
   v
fetchUser()
   |
   v
Promise resolves with user
   |
   v
getUserName()
   |
   v
User name
```

Each operation receives the resolved output of the previous operation.

# 🗁 Error Handling in Async Composition

If any function in a Promise-based pipeline rejects or throws an error, the resulting Promise rejects.

Example:

```javascript
const validateUser = async (user) => {
  if (!user.active) {
    throw new Error("User is inactive");
  }

  return user;
};

const getName = (user) => user.name;

const processUser = pipeAsync(
  validateUser,
  getName
);

processUser({
  name: "Alice",
  active: false
}).catch((error) => {
  console.error(error.message);
});
```

Output:

```text
User is inactive
```

The rejection propagates through the pipeline until it is handled.

# 🗁 Practical Example: User Data Processing

Consider an API response:

```javascript
const apiUser = {
  first_name: "alice",
  last_name: "smith",
  email_address: " ALICE@EXAMPLE.COM ",
  account_status: "active"
};
```

Create individual transformation functions:

```javascript
const createFullName = (user) => ({
  ...user,
  fullName: `${user.first_name} ${user.last_name}`
});

const normalizeEmail = (user) => ({
  ...user,
  email: user.email_address.trim().toLowerCase()
});

const createStatus = (user) => ({
  ...user,
  isActive: user.account_status === "active"
});

const selectProperties = (user) => ({
  fullName: user.fullName,
  email: user.email,
  isActive: user.isActive
});
```

Compose the transformations:

```javascript
const transformUser = pipe(
  createFullName,
  normalizeEmail,
  createStatus,
  selectProperties
);

const result = transformUser(apiUser);

console.log(result);
```

Output:

```javascript
{
  fullName: "alice smith",
  email: "alice@example.com",
  isActive: true
}
```

The original API response remains unchanged.

# 🗁 Function Composition and Middleware

Middleware systems use a related pattern in which functions wrap or process requests before passing control to subsequent operations.

A simplified middleware composition example:

```javascript
function composeMiddleware(...middlewares) {
  return function (context, finalHandler) {
    let next = finalHandler;

    for (const middleware of [...middlewares].reverse()) {
      const current = middleware;
      const previousNext = next;

      next = () => current(context, previousNext);
    }

    return next();
  };
}
```

Usage:

```javascript
const firstMiddleware = (context, next) => {
  console.log("First middleware");

  return next();
};

const secondMiddleware = (context, next) => {
  console.log("Second middleware");

  return next();
};

const finalHandler = () => {
  console.log("Final handler");
};

const run = composeMiddleware(
  firstMiddleware,
  secondMiddleware
);

run({}, finalHandler);
```

Output:

```text
First middleware
Second middleware
Final handler
```

This is a simplified synchronous middleware composition model. Production middleware systems often support asynchronous execution, error propagation, and more sophisticated control flow.

# 🗁 Benefits of Function Composition

Function composition provides several advantages:

* Reusability: small functions can be reused in different pipelines.
* Modularity: each function handles one responsibility.
* Readability: transformations can be expressed as a sequence of operations.
* Testability: individual functions can be tested independently.
* Maintainability: changes to one transformation do not necessarily affect others.
* Predictability: pure functions make pipelines easier to reason about.
* Flexibility: pipelines can be assembled dynamically.

# 🗁 Common Mistakes

**Confusing compose() and pipe() execution order**

`compose()` executes from right to left, while `pipe()` executes from left to right.

**Passing incompatible function outputs**

If one function returns an object but the next expects a string, the pipeline may fail.

```javascript
const pipeline = pipe(
  (value) => ({ name: value }),
  (value) => value.toUpperCase()
);
```

The second function receives an object, not a string.

**Forgetting asynchronous behavior**

A synchronous pipeline does not automatically wait for Promises.

Use an asynchronous pipeline when each operation depends on the resolved result of the previous one.

**Creating excessively long pipelines**

Long pipelines can become difficult to debug. Break them into meaningful stages.

**Ignoring error propagation**

Exceptions and Promise rejections should be handled at appropriate boundaries.

**Using composition where direct calls are clearer**

Composition is a tool for improving structure, not a requirement for every function call.

# 🗁 Best Practices

* Keep composed functions small and focused.
* Prefer pure functions when possible.
* Use consistent input and output types.
* Choose `pipe()` when left-to-right execution is easier to read.
* Choose `compose()` when right-to-left composition fits the problem.
* Adapt multi-argument functions before composing them.
* Handle asynchronous operations explicitly.
* Define clear error-handling boundaries.
* Name complex pipeline stages meaningfully.
* Test individual functions and complete pipelines.

# 🗁 Summary

Function composition combines multiple functions into a new function, allowing the output of one operation to become the input of another.

Important concepts include:

* Mathematical function composition.
* Composition order.
* `compose()` and `pipe()`.
* Unary functions.
* Function factories.
* Partial application.
* Asynchronous composition.
* Error propagation.
* Middleware composition.
* Reusable transformation pipelines.

Function composition is a powerful technique for building modular, reusable, and maintainable JavaScript applications.
