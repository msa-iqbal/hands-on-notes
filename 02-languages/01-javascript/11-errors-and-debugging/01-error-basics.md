# JavaScript Error Basics

Errors are problems that occur while JavaScript code is being parsed, executed, or processed by the runtime.

Understanding errors is essential for writing reliable applications, identifying bugs, and maintaining predictable program behavior.

JavaScript errors can occur for several reasons:

* Invalid syntax.
* Accessing undefined variables.
* Calling non-function values.
* Using invalid operations.
* Network or API failures.
* Invalid user input.
* Unexpected application states.

JavaScript provides built-in error objects and mechanisms for detecting, reporting, and handling errors.

# 🗁 What Is an Error?

An error represents a condition that prevents an operation from completing as expected.

Consider this example:

```javascript
const user = {
  name: "Alice"
};

console.log(user.age.toString());
```

This code attempts to access `toString()` on `user.age`, which is `undefined`.

JavaScript throws a `TypeError` because the operation is invalid.

```text
TypeError: Cannot read properties of undefined
```

When an exception is thrown and not handled, normal execution of the current code path stops.

# 🗁 Error Categories

JavaScript errors can be broadly categorized by when and why they occur.

### 🖆 Syntax Errors

A syntax error occurs when JavaScript code violates the language's grammar.

The JavaScript engine cannot parse the invalid code.

```javascript
const name = "Alice"

if (name === "Alice" {
  console.log("Hello Alice");
}
```

The closing parenthesis is missing from the `if` condition.

The engine reports a `SyntaxError`.

Another example:

```javascript
const = 100;
```

This is invalid variable declaration syntax.

**Characteristics:**

* Occur during parsing or compilation.
* Prevent the affected script or module from executing.
* Usually caused by missing brackets, invalid declarations, or malformed expressions.

### 🖆 Runtime Errors

Runtime errors occur while JavaScript code is executing.

The code may be syntactically valid but performs an operation that cannot be completed.

```javascript
const user = null;

console.log(user.name);
```

This produces a `TypeError` because properties cannot be accessed directly on `null`.

Another example:

```javascript
function calculate() {
  return 10 / 0;
}

console.log(calculate());
// Infinity
```

This example does not throw an error. JavaScript returns `Infinity` for division of a nonzero number by zero.

Runtime errors can include:

* Accessing properties of `null` or `undefined`.
* Calling a value that is not a function.
* Using invalid object operations.
* Violating runtime constraints.

### 🖆 Logical Errors

Logical errors occur when code executes successfully but produces an incorrect result.

```javascript
function calculateTotal(price, quantity) {
  return price - quantity;
}

console.log(calculateTotal(100, 5));
// 95
```

The intended operation may have been multiplication:

```javascript
function calculateTotal(price, quantity) {
  return price * quantity;
}

console.log(calculateTotal(100, 5));
// 500
```

Logical errors are often harder to detect because JavaScript does not necessarily throw an exception.

They are commonly discovered through:

* Testing.
* Debugging.
* Code reviews.
* Assertions.
* Unexpected application behavior.

### 🖆 Error Category Comparison

| Category | When It Occurs | Example |
| --- | --- | --- |
| Syntax error | Parsing | Missing closing bracket |
| Runtime error | Execution | Accessing a property of `null` |
| Logical error | Incorrect program logic | Using subtraction instead of multiplication |

# 🗁 The Built-in Error Object

JavaScript provides the `Error` object as the foundation for representing errors.

### 🖆 Creating an Error

```javascript
const error = new Error("Something went wrong");

console.log(error.message);
// Something went wrong

console.log(error.name);
// Error
```

The `Error` constructor creates an error object but does not automatically throw it.

To throw the error:

```javascript
throw new Error("Something went wrong");
```

### 🖆 Error Properties

Common properties of an error object include:

| Property | Description |
| --- | --- |
| `name` | Error type or name |
| `message` | Description of the error |
| `stack` | Stack trace showing where the error originated |

Example:

```javascript
const error = new Error("Database connection failed");

console.log(error.name);
// Error

console.log(error.message);
// Database connection failed

console.log(error.stack);
// Stack trace information
```

The `stack` property is commonly supported by JavaScript runtimes, although its exact format is implementation-dependent.

### 🖆 Customizing Error Properties

```javascript
const error = new Error("Invalid user input");

error.name = "ValidationError";

console.log(error.name);
// ValidationError

console.log(error.message);
// Invalid user input
```

For reusable application-specific errors, defining custom error classes is generally preferable.

# 🗁 Built-in JavaScript Error Types

JavaScript provides several specialized error types.

### 🖆 `Error`

The base error type.

```javascript
throw new Error("Something went wrong");
```

Use it for general application errors that do not require a more specific built-in type.

### 🖆 `SyntaxError`

Occurs when JavaScript encounters invalid syntax or invalid syntax in certain parsing operations.

```javascript
JSON.parse("{ invalid json }");
```

This throws a `SyntaxError` because the input is not valid JSON.

Another example:

```javascript
eval("const = 10;");
```

This also produces a `SyntaxError` when evaluated.

### 🖆 `ReferenceError`

Occurs when code references a variable or binding that does not exist in the current scope.

```javascript
console.log(username);
```

If `username` has not been declared in an accessible scope, JavaScript throws a `ReferenceError`.

Another example:

```javascript
function greet() {
  console.log(message);
}

greet();
```

If `message` is not declared in an accessible scope, the function throws a `ReferenceError`.

### 🖆 `TypeError`

Occurs when an operation is performed on a value that does not support that operation.

```javascript
const user = null;

console.log(user.name);
```

Another example:

```javascript
const value = 100;

value();
```

A number cannot be called as a function, so JavaScript throws a `TypeError`.

### 🖆 `RangeError`

Occurs when a value is outside an allowed range.

```javascript
const numbers = new Array(-1);
```

The array length cannot be negative, so JavaScript throws a `RangeError`.

Another example:

```javascript
const number = 1.5;

console.log(number.toFixed(101));
```

The requested precision exceeds the supported range.

### 🖆 `URIError`

Occurs when a URI-related encoding or decoding function receives an invalid input.

```javascript
decodeURIComponent("%");
```

The percent-encoded sequence is incomplete, so JavaScript throws a `URIError`.

### 🖆 `EvalError`

Represents an error related to the global `eval()` function.

It is retained for compatibility with the language specification but is rarely encountered in modern JavaScript applications.

Modern JavaScript engines generally do not throw `EvalError` for ordinary incorrect uses of `eval()`.

### 🖆 `AggregateError`

Represents multiple errors grouped into a single error object.

It is commonly used with operations such as `Promise.any()` when all input promises reject.

```javascript
const error = new AggregateError(
  [
    new Error("Server A failed"),
    new Error("Server B failed")
  ],
  "All servers failed"
);

console.log(error.name);
// AggregateError

console.log(error.message);
// All servers failed

console.log(error.errors.length);
// 2
```

### 🖆 Error Type Reference

| Error Type | Common Cause |
| --- | --- |
| `Error` | General application error |
| `SyntaxError` | Invalid syntax or parsing input |
| `ReferenceError` | Invalid variable or binding reference |
| `TypeError` | Invalid operation for a value |
| `RangeError` | Value outside an allowed range |
| `URIError` | Invalid URI encoding or decoding |
| `EvalError` | Legacy `eval()`-related error type |
| `AggregateError` | Multiple errors grouped together |

# 🗁 Throwing Errors

The `throw` statement allows developers to explicitly generate exceptions.

### 🖆 Basic `throw`

```javascript
throw new Error("Operation failed");
```

When JavaScript executes this statement, it throws an exception.

If no matching error handler exists, the exception propagates through the current call stack and may terminate the current execution path.

### 🖆 Throwing an Error in a Function

```javascript
function divide(a, b) {
  if (b === 0) {
    throw new Error("Division by zero is not allowed");
  }

  return a / b;
}

console.log(divide(10, 2));
// 5

console.log(divide(10, 0));
// Throws Error
```

### 🖆 Throwing Different Error Types

Choose an appropriate error type when possible.

```javascript
function getUser(id) {
  if (typeof id !== "number") {
    throw new TypeError("User ID must be a number");
  }

  if (id <= 0) {
    throw new RangeError("User ID must be positive");
  }

  return {
    id,
    name: "Alice"
  };
}
```

This makes errors easier to identify and handle.

# 🗁 Error Propagation

When an error is thrown, JavaScript searches for an appropriate error handler.

If the current function does not handle the error, it propagates to its caller.

### 🖆 Propagation Through Function Calls

```javascript
function first() {
  second();
}

function second() {
  third();
}

function third() {
  throw new Error("Something failed");
}

first();
```

The error originates in `third()` and propagates through:

```text
third()
  ↓
second()
  ↓
first()
  ↓
Uncaught Error
```

If none of these functions handles the error, it becomes an uncaught exception.

### 🖆 Handling an Error at a Higher Level

```javascript
function first() {
  second();
}

function second() {
  third();
}

function third() {
  throw new Error("Something failed");
}

try {
  first();
} catch (error) {
  console.log(error.message);
  // Something failed
}
```

The `try...catch` block handles the exception even though it originated in a deeply nested function.

This is useful for centralizing error handling at application boundaries.

# 🗁 Error Stack Trace

A stack trace shows the sequence of function calls leading to an error.

```javascript
function first() {
  second();
}

function second() {
  third();
}

function third() {
  throw new Error("Unexpected failure");
}

first();
```

A simplified stack trace may look like:

```text
Error: Unexpected failure
    at third (...)
    at second (...)
    at first (...)
```

### 🖆 Understanding Stack Traces

A stack trace helps developers identify:

* The error message.
* The function where the error originated.
* The sequence of function calls.
* The source file and line number, when available.

### 🖆 Why Stack Traces Matter

Stack traces are especially useful when debugging:

* Deeply nested function calls.
* API request failures.
* Complex application workflows.
* Unexpected runtime exceptions.
* Errors occurring in production environments.

Stack trace formats vary between browsers and JavaScript runtimes.

# 🗁 Preventing Common Errors

Many errors can be avoided through defensive programming and appropriate validation.

### 🖆 Check for `null` and `undefined`

Incorrect:

```javascript
function getUserName(user) {
  return user.name;
}

getUserName(null);
```

Correct:

```javascript
function getUserName(user) {
  if (user == null) {
    return "Guest";
  }

  return user.name;
}
```

The `== null` comparison intentionally matches both `null` and `undefined`.

### 🖆 Use Optional Chaining

Optional chaining safely accesses nested properties.

```javascript
const user = null;

console.log(user?.name);
// undefined
```

For nested objects:

```javascript
const user = {
  profile: {
    address: {
      city: "Dhaka"
    }
  }
};

console.log(user?.profile?.address?.city);
// Dhaka
```

If an intermediate value is `null` or `undefined`, the expression short-circuits and returns `undefined`.

### 🖆 Use Nullish Coalescing

The nullish coalescing operator (`??`) provides a fallback when a value is `null` or `undefined`.

```javascript
const username = null;

const displayName = username ?? "Guest";

console.log(displayName);
// Guest
```

Unlike the logical OR operator (`||`), it preserves valid falsy values such as `0`, `false`, and an empty string.

```javascript
console.log(0 || 100);
// 100

console.log(0 ?? 100);
// 0
```

### 🖆 Validate Function Arguments

```javascript
function calculateTotal(price, quantity) {
  if (typeof price !== "number" || !Number.isFinite(price)) {
    throw new TypeError("Price must be a finite number");
  }

  if (typeof quantity !== "number" || !Number.isInteger(quantity)) {
    throw new TypeError("Quantity must be an integer");
  }

  if (price < 0 || quantity < 0) {
    throw new RangeError("Price and quantity cannot be negative");
  }

  return price * quantity;
}

console.log(calculateTotal(100, 5));
// 500
```

### 🖆 Use Safe Property Access

```javascript
const user = {
  profile: {
    name: "Alice"
  }
};

const city = user.profile?.address?.city ?? "Unknown";

console.log(city);
// Unknown
```

# 🗁 Error Handling Principles

Good error handling makes applications easier to maintain and more reliable.

### 🖆 Fail Fast

Detect invalid conditions as early as possible.

```javascript
function createUser(name) {
  if (!name) {
    throw new Error("Name is required");
  }

  return {
    name
  };
}
```

### 🖆 Use Meaningful Error Messages

Prefer messages that describe what went wrong.

Less useful:

```javascript
throw new Error("Failed");
```

More useful:

```javascript
throw new Error("Unable to save user because the email address is missing");
```

### 🖆 Use Appropriate Error Types

Use `TypeError` for invalid value types, `RangeError` for invalid ranges, and custom error classes for application-specific conditions.

### 🖆 Avoid Silently Ignoring Errors

Avoid empty error handlers:

```javascript
try {
  performOperation();
} catch (error) {
}
```

An empty handler hides failures and makes debugging difficult.

Instead, handle the error, report it, recover where possible, or propagate it.

### 🖆 Separate Expected and Unexpected Errors

Expected application conditions may include:

* Invalid user input.
* Missing resources.
* Authentication failures.
* Business-rule violations.

Unexpected failures may include:

* Programming bugs.
* Broken assumptions.
* Unexpected runtime states.
* Infrastructure failures.

Handling these conditions separately makes application behavior more predictable.

# 🗁 Summary

JavaScript errors represent problems that occur during parsing, execution, or application processing.

Important concepts:

* Syntax errors prevent invalid code from being parsed.
* Runtime errors occur while code executes.
* Logical errors produce incorrect results without necessarily throwing exceptions.
* The `Error` object represents exceptions.
* JavaScript provides specialized error types such as `TypeError`, `ReferenceError`, `SyntaxError`, and `RangeError`.
* The `throw` statement explicitly generates exceptions.
* Errors propagate through function calls until handled.
* Stack traces help identify where errors originate.
* Defensive programming helps prevent common runtime failures.

Understanding error fundamentals provides the foundation for using `try...catch`, throwing custom errors, debugging applications, and building reliable JavaScript systems.
