# Throw

JavaScript uses the `throw` statement to explicitly generate an exception.

When an exception is thrown, normal execution stops at that point, and JavaScript searches for an appropriate exception handler.

The `throw` statement is useful when:

* An input value is invalid.
* A required resource is missing.
* A business rule is violated.
* An operation cannot continue safely.
* A function encounters an unexpected condition.

# 🗁 Basic Syntax

```javascript
throw expression;
```

The expression can be any JavaScript value, including:

* String
* Number
* Boolean
* Object
* Error instance
* `null`
* `undefined`

Example:

```javascript
throw new Error("Something went wrong");
```

# 🗁 Throwing an Error Object

The recommended approach is to throw an instance of `Error`.

```javascript
function withdraw(balance, amount) {
  if (amount > balance) {
    throw new Error("Insufficient balance");
  }

  return balance - amount;
}

console.log(withdraw(1000, 1500));
```

Output:

```text
Uncaught Error: Insufficient balance
```

The exception prevents the function from returning a result.

An `Error` object provides useful properties:

```javascript
const error = new Error("Something went wrong");

console.log(error.name);
console.log(error.message);
console.log(error.stack);
```

* `name`: Identifies the error type.
* `message`: Describes the error.
* `stack`: Contains stack trace information when supported.

# 🗁 Throwing Different Values

JavaScript permits throwing values other than `Error` objects.

Throwing a string:

```javascript
throw "Invalid operation";
```

Throwing a number:

```javascript
throw 404;
```

Throwing an object:

```javascript
throw {
  code: 400,
  message: "Invalid request"
};
```

These are valid JavaScript, but they are generally discouraged because they do not provide the standard error information and stack trace associated with `Error` objects.

Prefer:

```javascript
throw new Error("Invalid operation");
```

# 🗁 Throw with Try...Catch

A thrown exception can be handled using `try...catch`.

```javascript
try {
  throw new Error("Something went wrong");
} catch (error) {
  console.log(error.message);
}
```

Output:

```text
Something went wrong
```

When the exception is caught, execution continues after the `try...catch` statement.

```javascript
console.log("Before");

try {
  throw new Error("Operation failed");

  console.log("This will not execute");
} catch (error) {
  console.log(error.message);
}

console.log("After");
```

Output:

```text
Before
Operation failed
After
```

# 🗁 Throwing Errors from Functions

Functions can throw exceptions when they receive invalid arguments or encounter conditions they cannot handle.

```javascript
function calculateAge(birthYear) {
  if (typeof birthYear !== "number") {
    throw new TypeError("Birth year must be a number");
  }

  if (!Number.isInteger(birthYear)) {
    throw new TypeError("Birth year must be an integer");
  }

  const currentYear = new Date().getFullYear();

  if (birthYear > currentYear) {
    throw new RangeError("Birth year cannot be in the future");
  }

  return currentYear - birthYear;
}

try {
  console.log(calculateAge(2035));
} catch (error) {
  console.log(error.name);
  console.log(error.message);
}
```

The function validates its input and throws an appropriate exception when a condition is violated.

# 🗁 Using Built-in Error Types

JavaScript provides several built-in error constructors.

**Error**

Used for general application errors.

```javascript
throw new Error("Something went wrong");
```

**TypeError**

Used when a value has an inappropriate type or an operation cannot be performed on that type.

```javascript
throw new TypeError("Expected a string");
```

**RangeError**

Used when a value is outside an acceptable range.

```javascript
throw new RangeError("Value must be between 1 and 100");
```

**ReferenceError**

Used when referencing a variable that does not exist in the current scope.

```javascript
throw new ReferenceError("User variable is not defined");
```

**SyntaxError**

Used when source code or parsed input contains invalid syntax.

```javascript
throw new SyntaxError("Invalid JSON format");
```

**URIError**

Used when a URI-related function receives an invalid URI sequence.

```javascript
throw new URIError("Invalid URI sequence");
```

These constructors create standard error objects. JavaScript also generates many of these error types automatically when corresponding language operations fail.

# 🗁 Conditional Throwing

You can throw an exception when a specific condition is met.

```javascript
function registerUser(username, password) {
  if (!username) {
    throw new Error("Username is required");
  }

  if (!password) {
    throw new Error("Password is required");
  }

  if (password.length < 8) {
    throw new Error("Password must contain at least 8 characters");
  }

  return {
    username,
    message: "Registration successful"
  };
}

try {
  const user = registerUser("john", "123");

  console.log(user);
} catch (error) {
  console.error(error.message);
}
```

Conditional throwing is useful for enforcing validation rules and business constraints.

# 🗁 Rethrowing an Error

Sometimes a `catch` block needs to inspect an error but cannot fully handle it.

In that situation, you can rethrow the same error.

```javascript
function processOrder() {
  throw new Error("Payment processing failed");
}

try {
  processOrder();
} catch (error) {
  console.log("Logging error:", error.message);

  throw error;
}
```

The error is logged and then propagated to an outer exception handler.

```javascript
try {
  try {
    throw new Error("Database unavailable");
  } catch (error) {
    console.log("Inner handler:", error.message);

    throw error;
  }
} catch (error) {
  console.log("Outer handler:", error.message);
}
```

Output:

```text
Inner handler: Database unavailable
Outer handler: Database unavailable
```

Rethrowing preserves the original error object and its stack trace.

# 🗁 Throwing a New Error While Handling Another Error

You can create a new error to add meaningful context.

```javascript
try {
  JSON.parse("{ invalid json }");
} catch (error) {
  throw new Error("Unable to load application configuration", {
    cause: error
  });
}
```

The `cause` property preserves the original error as the underlying reason for the new error.

This is useful when translating low-level errors into higher-level application errors.

You can inspect the cause:

```javascript
try {
  try {
    JSON.parse("{ invalid json }");
  } catch (error) {
    throw new Error("Configuration loading failed", {
      cause: error
    });
  }
} catch (error) {
  console.log(error.message);
  console.log(error.cause.message);
}
```

Output:

```text
Configuration loading failed
Expected property name or '}' in JSON at position 2
```

The exact parser message depends on the JavaScript runtime.

# 🗁 Throw in Asynchronous Functions

An exception thrown inside an `async` function causes the function's returned Promise to reject.

```javascript
async function getUser(id) {
  if (!id) {
    throw new Error("User ID is required");
  }

  return {
    id,
    name: "John"
  };
}
```

Handle the rejection with `try...catch`:

```javascript
async function main() {
  try {
    const user = await getUser(null);

    console.log(user);
  } catch (error) {
    console.log(error.message);
  }
}

main();
```

You can also handle the rejected Promise using `.catch()`:

```javascript
getUser(null).catch(error => {
  console.log(error.message);
});
```

# 🗁 Throw in Promise Executors

Throwing an error inside a Promise executor automatically rejects that Promise.

```javascript
const promise = new Promise((resolve, reject) => {
  throw new Error("Promise operation failed");
});

promise.catch(error => {
  console.log(error.message);
});
```

Output:

```text
Promise operation failed
```

The Promise constructor converts a synchronous exception thrown by its executor into a rejection.

# 🗁 Throw vs Return

`throw` and `return` have different purposes.

| Feature | throw | return |
| --- | --- | --- |
| Purpose | Signals an exceptional condition | Provides a function result |
| Control flow | Transfers control to an exception handler | Exits the current function |
| Error handling | Requires an appropriate handler or propagation | Does not require a handler |
| Result | Raises an exception | Returns a value |

Example using `return`:

```javascript
function divide(a, b) {
  if (b === 0) {
    return null;
  }

  return a / b;
}
```

Example using `throw`:

```javascript
function divide(a, b) {
  if (b === 0) {
    throw new Error("Division by zero");
  }

  return a / b;
}
```

Use `return` when a condition is part of the function's normal result contract. Use `throw` when the function cannot complete normally because of an exceptional condition.

# 🗁 Common Mistakes

**Forgetting the Error constructor**

```javascript
throw Error;
```

This throws the constructor function itself rather than an error instance.

Prefer:

```javascript
throw new Error("Something went wrong");
```

**Throwing primitive values**

```javascript
throw "Invalid input";
```

This works, but it does not provide the standard error structure.

Prefer:

```javascript
throw new Error("Invalid input");
```

**Continuing execution after throw**

```javascript
throw new Error("Failed");

console.log("This will never execute");
```

Statements after an executed `throw` in the same control-flow path are unreachable.

**Throwing errors for normal control flow**

Avoid using exceptions for expected conditions that can be represented naturally through return values.

**Losing the original error**

```javascript
try {
  performOperation();
} catch (error) {
  throw new Error("Operation failed");
}
```

This creates a new error without preserving the original cause.

Prefer:

```javascript
try {
  performOperation();
} catch (error) {
  throw new Error("Operation failed", {
    cause: error
  });
}
```

# 🗁 Best Practices

* Prefer `Error` instances over primitive thrown values.
* Use specific built-in error types when appropriate.
* Write descriptive error messages.
* Validate function arguments before performing operations.
* Throw errors when an operation cannot satisfy its contract.
* Use `try...catch` at a layer capable of handling the failure.
* Rethrow errors when the current layer cannot recover.
* Preserve original errors using the `cause` option when adding context.
* Avoid using exceptions for ordinary conditional logic.
* Do not expose sensitive internal details through user-facing error messages.

# 🗁 Key Takeaways

* `throw` explicitly generates an exception.
* Throwing an exception immediately stops normal execution in the current control-flow path.
* JavaScript allows throwing any value, but `Error` objects are recommended.
* Exceptions can be handled using `try...catch`.
* Functions can throw errors to communicate invalid input or failed operations.
* Built-in error types provide more specific error classification.
* Rethrowing allows errors to propagate to higher-level handlers.
* The `cause` option preserves the underlying error when creating a new error.
* Exceptions thrown inside `async` functions reject their returned Promises.
* Throwing inside a Promise executor automatically rejects that Promise.
