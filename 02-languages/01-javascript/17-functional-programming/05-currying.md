# Currying

Currying is a functional programming technique that transforms a function accepting multiple arguments into a sequence of functions, each accepting one argument at a time.

Instead of passing all arguments simultaneously, arguments are supplied individually through nested function calls.

A normal function:

```javascript
function add(a, b, c) {
  return a + b + c;
}

console.log(add(2, 3, 4)); // 9
```

The curried version:

```javascript
function add(a) {
  return function (b) {
    return function (c) {
      return a + b + c;
    };
  };
}

console.log(add(2)(3)(4)); // 9
```

Each function accepts one argument and returns another function until all required arguments have been provided.

# 🗁 Understanding Currying

Consider a function that multiplies two numbers:

```javascript
function multiply(a, b) {
  return a * b;
}
```

A curried implementation:

```javascript
function multiply(a) {
  return function (b) {
    return a * b;
  };
}
```

Usage:

```javascript
const result = multiply(5)(4);

console.log(result); // 20
```

Execution:

```text
multiply(5)
    |
    v
Returns a function waiting for b
    |
    v
function (b) { return 5 * b }
    |
    v
multiply(5)(4)
    |
    v
20
```

The first function receives `5` and returns another function. The second function receives `4` and calculates the result.

# 🗁 Currying with Arrow Functions

Arrow functions make curried functions more concise.

Traditional implementation:

```javascript
function add(a) {
  return function (b) {
    return function (c) {
      return a + b + c;
    };
  };
}
```

Arrow function implementation:

```javascript
const add = (a) => (b) => (c) => a + b + c;

console.log(add(2)(3)(4)); // 9
```

The expression:

```javascript
(a) => (b) => (c) => a + b + c
```

is equivalent to three nested functions.

# 🗁 Currying vs Normal Functions

| Feature | Normal Function | Curried Function |
| --- | --- | --- |
| Arguments | Passed together | Passed individually |
| Function calls | Usually one call | Multiple chained calls |
| Return value | Usually final result | Often another function |
| Reusability | Depends on implementation | Easily specialized with partial application |
| Example | `add(2, 3)` | `add(2)(3)` |

Both approaches are valid. Currying is useful when functions need to be configured and reused.

# 🗁 Currying vs Partial Application

Currying and partial application are related but different techniques.

**Currying** transforms a multi-argument function into a sequence of single-argument functions.

**Partial application** fixes some arguments of a function and returns a new function that accepts the remaining arguments.

Normal function:

```javascript
function multiply(a, b, c) {
  return a * b * c;
}
```

Curried function:

```javascript
const multiplyCurried = (a) => (b) => (c) =>
  a * b * c;

console.log(multiplyCurried(2)(3)(4)); // 24
```

Partially applied function:

```javascript
function multiply(a, b, c) {
  return a * b * c;
}

const multiplyByTwo = (b, c) => multiply(2, b, c);

console.log(multiplyByTwo(3, 4)); // 24
```

The distinction:

| Currying | Partial Application |
| --- | --- |
| Converts a function into nested single-argument functions | Fixes some arguments of an existing function |
| Produces a sequence of functions | Produces a function with fewer remaining arguments |
| Commonly uses `f(a)(b)(c)` | Can use `f(a, b)` after fixing other arguments |

A curried function can also be partially applied by supplying only some of its arguments.

# 🗁 Creating Reusable Specialized Functions

Currying allows a general function to be specialized by providing some arguments in advance.

Example:

```javascript
const calculateTax = (taxRate) => (price) =>
  price * taxRate;

const calculateVAT = calculateTax(0.15);
const calculateIncomeTax = calculateTax(0.2);

console.log(calculateVAT(1000)); // 150
console.log(calculateIncomeTax(1000)); // 200
```

The first call configures the tax rate.

The returned function can then calculate tax for different prices.

This avoids repeatedly passing the same configuration argument.

# 🗁 Currying with Multiple Arguments

Consider a function that formats a message:

```javascript
function formatMessage(prefix, separator, message) {
  return `${prefix}${separator}${message}`;
}
```

Curried implementation:

```javascript
const formatMessage = (prefix) => (separator) => (message) =>
  `${prefix}${separator}${message}`;

console.log(formatMessage("INFO")(": ")("Server started"));
// INFO: Server started
```

Each argument is supplied through a separate function call.

# 🗁 Currying with String Processing

Currying can create reusable string transformation functions.

Example:

```javascript
const replace = (searchValue) => (replacement) => (text) =>
  text.replaceAll(searchValue, replacement);

const removeSpaces = replace(" ")("");
const replaceHyphens = replace("-")("_");

console.log(removeSpaces("Hello World"));
// HelloWorld

console.log(replaceHyphens("user-profile"));
// user_profile
```

The search value and replacement value are configured once.

The returned functions can process multiple strings.

# 🗁 Currying with Array Methods

Currying works well with array transformation functions.

Example:

```javascript
const filterBy = (property) => (value) => (items) =>
  items.filter((item) => item[property] === value);

const filterByStatus = filterBy("status");

const getActiveUsers = filterByStatus("active");
const getInactiveUsers = filterByStatus("inactive");

const users = [
  { name: "Alice", status: "active" },
  { name: "Bob", status: "inactive" },
  { name: "Charlie", status: "active" }
];

console.log(getActiveUsers(users));
```

Output:

```javascript
[
  { name: "Alice", status: "active" },
  { name: "Charlie", status: "active" }
]
```

The function is configured in stages:

* Select the property.
* Select the expected value.
* Pass the array to filter.

# 🗁 Currying with Event Handlers

Currying can help create reusable event handlers with predefined configuration.

Example:

```javascript
const createClickHandler = (message) => (event) => {
  console.log(message);
  console.log(event.target);
};

const handleSave = createClickHandler("Save button clicked");
const handleDelete = createClickHandler("Delete button clicked");

document
  .querySelector("#save")
  .addEventListener("click", handleSave);

document
  .querySelector("#delete")
  .addEventListener("click", handleDelete);
```

Each handler is configured with a message and receives the event when triggered.

This separates configuration from event execution.

# 🗁 Generic Currying Function

A generic currying utility can transform functions with multiple declared parameters into curried functions.

Implementation:

```javascript
function curry(fn) {
  return function curried(...args) {
    if (args.length >= fn.length) {
      return fn(...args);
    }

    return (...nextArgs) => curried(...args, ...nextArgs);
  };
}
```

Usage:

```javascript
function add(a, b, c) {
  return a + b + c;
}

const curriedAdd = curry(add);

console.log(curriedAdd(2)(3)(4)); // 9
console.log(curriedAdd(2, 3)(4)); // 9
console.log(curriedAdd(2)(3, 4)); // 9
console.log(curriedAdd(2, 3, 4)); // 9
```

This implementation supports supplying arguments one at a time or in groups.

The utility uses `fn.length` to determine how many declared parameters are required.

# 🗁 Understanding fn.length

The `length` property of a function indicates the number of parameters before the first parameter with a default value.

Example:

```javascript
function add(a, b, c) {
  return a + b + c;
}

console.log(add.length); // 3
```

Default parameters affect this value:

```javascript
function calculate(a, b = 10, c) {
  return a + b + c;
}

console.log(calculate.length); // 1
```

This matters because the generic `curry()` implementation uses `fn.length` to determine when to execute the original function.

It is not a universal currying solution for every JavaScript function.

# 🗁 Limitations of Generic Currying

The previous implementation has several limitations:

* Default parameters can make `fn.length` inaccurate for currying purposes.
* Rest parameters do not contribute to `fn.length`.
* It does not support placeholder arguments.
* It assumes arguments are supplied from left to right.
* Functions whose required argument count differs from `fn.length` need explicit configuration.
* It does not preserve the original function's metadata or all invocation semantics.

For production applications, use a well-tested functional programming utility when advanced currying behavior is required.

# 🗁 Currying with Placeholders

Some functional programming libraries support placeholders.

A placeholder represents an argument that will be supplied later.

Conceptual example:

```javascript
const divide = (a, b, c) => a / b / c;

// Conceptual placeholder-based API:
const divideByTwo = curry(divide)(_, 2, _);

console.log(divideByTwo(100, 5)); // 10
```

Here, `_` represents an argument that has not yet been supplied.

JavaScript does not provide a built-in placeholder-based currying mechanism. This behavior must be implemented or provided by a library.

# 🗁 Currying with Function Composition

Curried functions can work naturally with composition utilities.

Example:

```javascript
const add = (a) => (b) => a + b;

const multiply = (a) => (b) => a * b;

const addFive = add(5);
const multiplyByTwo = multiply(2);

const result = addFive(10);

console.log(result); // 15
console.log(multiplyByTwo(result)); // 30
```

For a conventional `pipe()` utility that passes one result into the next function, configure each curried function first:

```javascript
const pipe = (...functions) => (value) =>
  functions.reduce(
    (result, fn) => fn(result),
    value
  );

const addFive = (value) => value + 5;
const multiplyByTwo = (value) => value * 2;
const square = (value) => value ** 2;

const calculate = pipe(
  addFive,
  multiplyByTwo,
  square
);

console.log(calculate(5)); // 400
```

Currying and composition complement each other: currying configures functions, while composition combines functions into pipelines.

# 🗁 Currying with Asynchronous Functions

Curried functions can also return Promises.

Example:

```javascript
const fetchUser = (baseUrl) => (userId) =>
  fetch(`${baseUrl}/users/${userId}`)
    .then((response) => {
      if (!response.ok) {
        throw new Error("Failed to fetch user");
      }

      return response.json();
    });

const fetchFromAPI = fetchUser("https://api.example.com");

fetchFromAPI(101)
  .then(console.log)
  .catch(console.error);
```

The API base URL is configured once. The returned function can fetch multiple users.

# 🗁 Practical Example: Configurable Logger

A logger can use currying to configure log levels and messages.

```javascript
const logger = (level) => (message) => {
  console.log(`[${level}] ${message}`);
};

const info = logger("INFO");
const warning = logger("WARNING");
const error = logger("ERROR");

info("Application started");
warning("Storage is almost full");
error("Database connection failed");
```

Output:

```text
[INFO] Application started
[WARNING] Storage is almost full
[ERROR] Database connection failed
```

The log level is configured once, and each specialized function accepts only the message.

# 🗁 Practical Example: Permission Checking

Currying can create reusable authorization checks.

```javascript
const hasPermission = (permission) => (user) =>
  user.permissions.includes(permission);

const canRead = hasPermission("read");
const canWrite = hasPermission("write");
const canDelete = hasPermission("delete");

const user = {
  name: "Alice",
  permissions: ["read", "write"]
};

console.log(canRead(user));   // true
console.log(canWrite(user));  // true
console.log(canDelete(user)); // false
```

Each permission checker is a specialized function.

This pattern can be useful in authorization logic, filtering, and application-level business rules.

# 🗁 Benefits of Currying

Currying provides several advantages:

* Reusability: create specialized functions from general functions.
* Configuration: separate function configuration from execution.
* Composition: make functions easier to combine in pipelines.
* Readability: express multi-stage operations clearly when the pattern fits.
* Modularity: isolate parameters that change from parameters that remain constant.
* Functional programming: encourage small functions with predictable inputs and outputs.

# 🗁 Common Mistakes

**Confusing currying with partial application**

Currying transforms a function into nested functions. Partial application fixes some arguments and returns a function with fewer remaining arguments.

**Calling a curried function with all arguments at once**

```javascript
const add = (a) => (b) => a + b;

console.log(add(2, 3)); // Returns a function, not 5
```

The function expects the second argument through another call.

**Overusing nested functions**

Currying can make simple operations harder to understand when applied unnecessarily.

**Ignoring function arity**

Generic currying utilities based on `fn.length` may behave unexpectedly with default and rest parameters.

**Forgetting that functions are returned**

Each intermediate call may return another function rather than the final result.

# 🗁 Best Practices

* Use currying when some arguments should be configured once and reused.
* Prefer clear arrow function syntax for simple curried functions.
* Keep each stage focused on one argument or configuration step.
* Use meaningful names for specialized functions.
* Combine currying with composition when it improves readability.
* Avoid unnecessary currying in ordinary application code.
* Understand the limitations of generic currying utilities.
* Use established libraries when placeholder support or advanced currying behavior is required.

# 🗁 Summary

Currying transforms a function that accepts multiple arguments into a sequence of functions that accept arguments individually.

Important concepts include:

* Curried functions.
* Nested functions.
* Arrow function currying.
* Currying vs partial application.
* Specialized reusable functions.
* Function composition.
* Generic currying utilities.
* Function arity and `fn.length`.
* Placeholder-based currying.
* Asynchronous currying.

Currying is particularly useful when building reusable, configurable functions and functional programming pipelines.
