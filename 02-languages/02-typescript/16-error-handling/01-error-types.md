# Error Types

Error handling is an important part of TypeScript application development. It allows applications to detect failures, communicate meaningful information, and recover from unexpected situations.

TypeScript uses JavaScript's runtime error system while providing static types to help developers handle errors more safely.

Understanding error types helps developers distinguish between different failures and implement appropriate recovery strategies.

# 🗁 What Is an Error?

An error represents an exceptional condition that interrupts the normal execution of a program.

JavaScript provides the built-in `Error` class for creating and representing errors.

```typescript
const error = new Error("Something went wrong");

console.log(error.name);
console.log(error.message);
```

Output:

```text
Error
Something went wrong
```

An error object commonly contains:

| Property | Description |
| --- | --- |
| `name` | Identifies the error type. |
| `message` | Describes the error. |
| `stack` | Provides a stack trace when supported by the runtime. |

# 🗁 Built-in JavaScript Error Types

JavaScript provides several built-in error types.

| Error Type | Description |
| --- | --- |
| `Error` | General-purpose error. |
| `TypeError` | Occurs when a value is used in an invalid way. |
| `ReferenceError` | Occurs when accessing an unavailable variable or reference. |
| `SyntaxError` | Occurs when JavaScript encounters invalid syntax or invalid parsed input. |
| `RangeError` | Occurs when a value is outside an allowed range. |
| `URIError` | Occurs when URI-related functions receive invalid URI data. |
| `EvalError` | Retained for compatibility; modern JavaScript does not commonly throw it. |
| `AggregateError` | Represents multiple errors grouped into one error. |

# 🗁 Error

The `Error` class is the base class for most built-in JavaScript errors.

```typescript
try {
  throw new Error("Unable to process request");
} catch (error) {
  console.log(error);
}
```

Use `Error` when no more specific built-in error type describes the failure.

# 🗁 TypeError

A `TypeError` occurs when an operation is performed on a value that does not support that operation.

```typescript
try {
  const value: unknown = null;

  console.log((value as { name: string }).name);
} catch (error) {
  console.log(error);
}
```

The runtime throws a `TypeError` because `null` cannot be accessed as an object.

Another example:

```typescript
try {
  const user = {
    name: "Alice"
  };

  (user as any).getName();
} catch (error) {
  console.log(error instanceof TypeError);
}
```

Output:

```text
true
```

TypeScript catches many invalid operations during compilation, but runtime values can still produce `TypeError` exceptions.

# 🗁 ReferenceError

A `ReferenceError` occurs when code references a variable that is not available in the current execution context.

```typescript
try {
  console.log(unknownVariable);
} catch (error) {
  console.log(error instanceof ReferenceError);
}
```

Output:

```text
true
```

TypeScript normally detects undeclared variables during compilation. Reference errors can still occur through dynamically evaluated code or JavaScript executed outside TypeScript's static checks.

# 🗁 SyntaxError

A `SyntaxError` occurs when JavaScript cannot parse source code or certain dynamically parsed data.

```typescript
try {
  JSON.parse("{ name: 'Alice' }");
} catch (error) {
  console.log(error instanceof SyntaxError);
}
```

Output:

```text
true
```

The JSON string is invalid because JSON requires double-quoted property names and string values.

# 🗁 RangeError

A `RangeError` occurs when a value is outside the valid range accepted by an operation.

```typescript
try {
  const values = new Array(-1);
} catch (error) {
  console.log(error instanceof RangeError);
}
```

Output:

```text
true
```

A negative array length is invalid.

# 🗁 URIError

A `URIError` occurs when URI encoding or decoding functions receive malformed URI data.

```typescript
try {
  decodeURIComponent("%");
} catch (error) {
  console.log(error instanceof URIError);
}
```

Output:

```text
true
```

The percent sign must be followed by a valid encoded byte sequence.

# 🗁 AggregateError

`AggregateError` represents multiple errors in one error object.

It is particularly useful when several asynchronous operations fail and their errors need to be reported together.

```typescript
const error = new AggregateError(
  [
    new Error("Database unavailable"),
    new Error("Cache unavailable")
  ],
  "Multiple services failed"
);

console.log(error.message);
console.log(error.errors);
```

Example with `Promise.any()`:

```typescript
async function loadData() {
  try {
    await Promise.any([
      Promise.reject(new Error("Server A failed")),
      Promise.reject(new Error("Server B failed"))
    ]);
  } catch (error) {
    if (error instanceof AggregateError) {
      console.log("All operations failed");

      for (const item of error.errors) {
        console.log(item);
      }
    }
  }
}

loadData();
```

`Promise.any()` rejects with an `AggregateError` when every input Promise rejects.

# 🗁 Custom Error Types

Applications often need errors that represent specific business or application failures.

A custom error class can extend the built-in `Error` class.

```typescript
class ValidationError extends Error {
  constructor(message: string) {
    super(message);

    this.name = "ValidationError";
  }
}

throw new ValidationError("Email is required");
```

Custom errors provide meaningful names and allow errors to be handled according to their purpose.

# 🗁 Custom Error with Additional Properties

Custom errors can contain additional information.

```typescript
class HttpError extends Error {
  constructor(
    message: string,
    public statusCode: number
  ) {
    super(message);

    this.name = "HttpError";
  }
}

const error = new HttpError(
  "Resource not found",
  404
);

console.log(error.message);
console.log(error.statusCode);
```

Output:

```text
Resource not found
404
```

This pattern is useful for HTTP clients, APIs, and backend services.

# 🗁 Error Cause

JavaScript supports the `cause` option when creating an error.

It allows a higher-level error to preserve the original error that caused the failure.

```typescript
try {
  throw new Error("Database connection failed");
} catch (error) {
  throw new Error("Unable to load users", {
    cause: error
  });
}
```

The original error can be accessed through the `cause` property.

```typescript
try {
  throw new Error("Unable to load users", {
    cause: new Error("Database connection failed")
  });
} catch (error) {
  if (error instanceof Error) {
    console.log(error.message);
    console.log(error.cause);
  }
}
```

Error causes are useful when translating low-level errors into higher-level application errors without losing diagnostic information.

# 🗁 Error Types in TypeScript

TypeScript allows developers to use built-in error classes as types.

```typescript
function handleError(error: Error): void {
  console.log(error.name);
  console.log(error.message);
}

handleError(new Error("Something failed"));
handleError(new TypeError("Invalid operation"));
```

However, the `catch` variable is typed as `unknown` by default when `useUnknownInCatchVariables` is enabled, which is included in strict mode.

This is because JavaScript allows throwing any value, not only instances of `Error`.

# 🗁 Throwing Different Values

JavaScript technically allows any value to be thrown.

```typescript
throw new Error("Something failed");

throw "Something failed";

throw 404;

throw null;

throw {
  code: "INVALID_INPUT"
};
```

Although these are all valid JavaScript, throwing arbitrary values makes error handling less predictable.

Prefer throwing instances of `Error` or its subclasses.

# 🗁 Error Handling with instanceof

The `instanceof` operator can identify specific error classes.

```typescript
function handleError(error: unknown): void {
  if (error instanceof TypeError) {
    console.log("Type error:", error.message);
  } else if (error instanceof RangeError) {
    console.log("Range error:", error.message);
  } else if (error instanceof Error) {
    console.log("General error:", error.message);
  } else {
    console.log("Unknown error:", error);
  }
}
```

This approach provides type narrowing and allows different error types to be handled separately.

# 🗁 Error Type Best Practices

* Prefer throwing `Error` instances instead of strings or arbitrary values.
* Use specific built-in error classes when appropriate.
* Create custom error classes for meaningful application-specific failures.
* Include useful error messages.
* Preserve original errors through the `cause` property when wrapping errors.
* Use `unknown` when accepting arbitrary error values.
* Avoid exposing sensitive internal details in public error responses.
* Keep error messages useful for debugging and application monitoring.
* Handle expected failures explicitly rather than relying on generic catch blocks.

# 🗁 Summary

JavaScript provides several built-in error types that TypeScript can use for structured error handling.

Key concepts:

* `Error` is the base error class.
* `TypeError` represents invalid operations on values.
* `ReferenceError` represents unavailable variable references.
* `SyntaxError` represents parsing failures.
* `RangeError` represents out-of-range values.
* `URIError` represents malformed URI operations.
* `AggregateError` groups multiple errors.
* Custom error classes represent application-specific failures.
* The `cause` property preserves the original error.
* TypeScript uses `unknown` for catch variables in strict configurations.
