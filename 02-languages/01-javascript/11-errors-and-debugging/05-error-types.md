# Error Types

JavaScript provides several built-in error types to identify different categories of runtime and language errors.

An error type describes the nature of a failure and helps developers understand what went wrong.

Understanding error types is important for:

* Debugging application failures.
* Validating input correctly.
* Handling exceptions appropriately.
* Identifying programming mistakes.
* Building reliable error-handling systems.

JavaScript's built-in error hierarchy is based on the `Error` object.

# 🗁 Error Hierarchy

The primary built-in error types inherit from `Error`.

```text
Error
├── EvalError
├── RangeError
├── ReferenceError
├── SyntaxError
├── TypeError
└── URIError
```

JavaScript also provides `AggregateError`, which represents multiple errors grouped into a single error object.

These error constructors are globally available in JavaScript.

# 🗁 Error

`Error` represents a general error condition.

It is commonly used when an operation fails for a reason that does not require a more specific built-in error type.

```javascript
throw new Error("Something went wrong");
```

Handling an Error:

```javascript
try {
  throw new Error("Database connection failed");
} catch (error) {
  console.log(error.name);
  console.log(error.message);
}
```

Output:

```text
Error
Database connection failed
```

Common use cases:

* General application failures.
* Failed operations.
* Unexpected conditions.
* Custom application error inheritance.

# 🗁 EvalError

`EvalError` represents errors related to the global `eval()` function.

It is retained primarily for backward compatibility.

Modern JavaScript engines generally do not throw `EvalError` for ordinary `eval()` failures.

```javascript
const error = new EvalError("Invalid eval operation");

console.log(error.name);
console.log(error.message);
```

Output:

```text
EvalError
Invalid eval operation
```

In modern JavaScript, `SyntaxError` and other error types are more commonly associated with problems involving evaluated code.

# 🗁 RangeError

`RangeError` occurs when a value is outside an acceptable range.

Example:

```javascript
try {
  const number = 1;

  number.toFixed(101);
} catch (error) {
  console.log(error.name);
  console.log(error.message);
}
```

`toFixed()` supports a limited number of fractional digits, so passing `101` causes a `RangeError`.

Another example:

```javascript
try {
  const array = new Array(-1);
} catch (error) {
  console.log(error.name);
}
```

Output:

```text
RangeError
```

Common use cases:

* Invalid numeric ranges.
* Invalid array lengths.
* Excessive recursion depth.
* Invalid precision arguments.

# 🗁 ReferenceError

`ReferenceError` occurs when JavaScript cannot resolve a variable or reference in the current execution context.

Example:

```javascript
try {
  console.log(username);
} catch (error) {
  console.log(error.name);
  console.log(error.message);
}
```

Output:

```text
ReferenceError
username is not defined
```

Another example involving the temporal dead zone:

```javascript
try {
  console.log(age);

  let age = 25;
} catch (error) {
  console.log(error.name);
}
```

Output:

```text
ReferenceError
```

Common use cases:

* Accessing undeclared variables.
* Accessing lexical variables before initialization.
* Referencing unavailable identifiers.

# 🗁 SyntaxError

`SyntaxError` occurs when JavaScript encounters invalid syntax or attempts to parse invalid JavaScript source or data.

Example using JSON:

```javascript
try {
  JSON.parse("{ invalid json }");
} catch (error) {
  console.log(error.name);
  console.log(error.message);
}
```

Output:

```text
SyntaxError
```

Another example using `eval()`:

```javascript
try {
  eval("const = 10");
} catch (error) {
  console.log(error.name);
}
```

Output:

```text
SyntaxError
```

Syntax errors in ordinary JavaScript source code are generally detected during parsing, before the affected code executes. Consequently, a `try...catch` inside that same invalid script cannot catch its own parse-time syntax error.

Common use cases:

* Invalid JSON.
* Invalid dynamically evaluated source.
* Invalid dynamically constructed code.
* Parsing failures.

# 🗁 TypeError

`TypeError` occurs when an operation is performed on a value that does not support that operation or when a value is used in an invalid way.

Example:

```javascript
try {
  const user = null;

  console.log(user.name);
} catch (error) {
  console.log(error.name);
  console.log(error.message);
}
```

Output:

```text
TypeError
```

Another example:

```javascript
try {
  const number = 10;

  number();
} catch (error) {
  console.log(error.name);
}
```

Output:

```text
TypeError
```

Common use cases:

* Calling a non-function value.
* Accessing properties on `null` or `undefined`.
* Assigning to a non-writable property.
* Using an operation with an incompatible value.

# 🗁 URIError

`URIError` occurs when a URI-related encoding or decoding function receives an invalid URI sequence.

Example:

```javascript
try {
  decodeURIComponent("%");
} catch (error) {
  console.log(error.name);
  console.log(error.message);
}
```

Output:

```text
URIError
```

Other URI-related functions include:

* `encodeURI()`
* `encodeURIComponent()`
* `decodeURI()`
* `decodeURIComponent()`

Common use cases:

* Invalid percent-encoded sequences.
* Malformed URI components.
* Invalid encoded character sequences.

# 🗁 AggregateError

`AggregateError` represents multiple errors grouped into one error object.

It is particularly useful when multiple asynchronous operations fail and their errors need to be reported together.

Example:

```javascript
const errors = [
  new Error("Database unavailable"),
  new Error("API request failed")
];

const aggregateError = new AggregateError(
  errors,
  "Multiple operations failed"
);

console.log(aggregateError.name);
console.log(aggregateError.message);
console.log(aggregateError.errors);
```

Output:

```text
AggregateError
Multiple operations failed
[
  Error: Database unavailable,
  Error: API request failed
]
```

A practical example using `Promise.any()`:

```javascript
const promises = [
  Promise.reject(new Error("Server A failed")),
  Promise.reject(new Error("Server B failed")),
  Promise.reject(new Error("Server C failed"))
];

Promise.any(promises).catch(error => {
  console.log(error.name);

  for (const reason of error.errors) {
    console.log(reason.message);
  }
});
```

Because every Promise rejects, `Promise.any()` rejects with an `AggregateError` containing the individual rejection reasons.

# 🗁 Error Object Properties

Standard JavaScript error objects expose several useful properties.

| Property | Description |
| --- | --- |
| `name` | Identifies the error type |
| `message` | Describes the error |
| `stack` | Provides stack trace information when supported |
| `cause` | Preserves the underlying reason when supplied |
| `errors` | Contains individual errors in an `AggregateError` |

Example:

```javascript
try {
  throw new Error("Operation failed", {
    cause: new Error("Connection timeout")
  });
} catch (error) {
  console.log(error.name);
  console.log(error.message);
  console.log(error.cause.message);
  console.log(error.stack);
}
```

The `stack` property is widely implemented, but its exact format is not standardized across JavaScript environments.

# 🗁 Identifying Error Types

Use `instanceof` to identify the type of an error.

```javascript
try {
  JSON.parse("{ invalid json }");
} catch (error) {
  if (error instanceof SyntaxError) {
    console.log("Invalid syntax or JSON");
  } else if (error instanceof TypeError) {
    console.log("Invalid operation");
  } else if (error instanceof RangeError) {
    console.log("Value out of range");
  } else {
    console.log("Unexpected error");
  }
}
```

For application-specific errors, checking against a custom error class is also useful.

```javascript
class ValidationError extends Error {
  constructor(message) {
    super(message);

    this.name = "ValidationError";
  }
}

try {
  throw new ValidationError("Invalid email");
} catch (error) {
  if (error instanceof ValidationError) {
    console.log("Validation failed:", error.message);
  }
}
```

# 🗁 Error Handling by Type

Different error types often require different handling strategies.

```javascript
function handleError(error) {
  if (error instanceof TypeError) {
    console.error("Check the value and its type");
  } else if (error instanceof ReferenceError) {
    console.error("Check variable declarations and scope");
  } else if (error instanceof SyntaxError) {
    console.error("Check the syntax or parsed data");
  } else if (error instanceof RangeError) {
    console.error("Check the allowed value range");
  } else if (error instanceof URIError) {
    console.error("Check URI encoding or decoding");
  } else {
    console.error("An unexpected error occurred");
  }
}
```

Usage:

```javascript
try {
  JSON.parse("{ invalid json }");
} catch (error) {
  handleError(error);
}
```

This approach helps separate error classification from error-handling logic.

# 🗁 Common Mistakes

**Treating every error as a TypeError**

Different errors represent different failure conditions. Identify the actual error type instead of assuming every exception is a `TypeError`.

**Checking only the error message**

```javascript
if (error.message === "Invalid input") {
  // Handle error
}
```

Messages can change between implementations or application versions.

Prefer checking the error class:

```javascript
if (error instanceof TypeError) {
  // Handle error
}
```

**Assuming every thrown value is an Error**

JavaScript allows throwing arbitrary values.

```javascript
try {
  throw "Something failed";
} catch (error) {
  console.log(error);
}
```

When accepting errors from external or unknown code, do not assume that every thrown value has standard Error properties.

**Confusing syntax errors with runtime errors**

Syntax errors in ordinary source code are generally detected before execution. Runtime errors occur while code is executing.

**Ignoring the original cause**

When wrapping an error, preserve its cause to retain useful debugging information.

# 🗁 Best Practices

* Use the most appropriate built-in error type.
* Prefer `Error` objects over primitive thrown values.
* Use `instanceof` to classify known error types.
* Preserve original errors when wrapping them.
* Include useful context in error messages.
* Avoid relying on exact error-message strings.
* Handle expected failures at the appropriate application layer.
* Log sufficient diagnostic information for unexpected failures.
* Avoid exposing internal stack traces to end users.
* Use custom error classes when application-specific classification is necessary.

# 🗁 Key Takeaways

* JavaScript provides several built-in error types.
* `Error` represents general errors.
* `EvalError` is primarily retained for backward compatibility.
* `RangeError` represents values outside acceptable ranges.
* `ReferenceError` represents invalid variable or reference access.
* `SyntaxError` represents parsing and syntax failures.
* `TypeError` represents invalid operations involving values.
* `URIError` represents invalid URI encoding or decoding.
* `AggregateError` groups multiple errors into one object.
* Error properties such as `name`, `message`, `stack`, and `cause` help with diagnosis.
* `instanceof` allows reliable classification of known error classes within the same JavaScript realm.
