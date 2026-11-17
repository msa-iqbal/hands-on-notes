# Safe Error Handling

Safe error handling is the practice of detecting, processing, and reporting failures without exposing sensitive information or allowing unexpected errors to compromise application stability.

In TypeScript, safe error handling involves:

* Treating caught values as `unknown`.
* Narrowing error types before accessing properties.
* Handling expected and unexpected failures separately.
* Preserving original error information.
* Avoiding sensitive information in public responses.
* Using consistent error structures.
* Logging errors appropriately.
* Preventing unhandled promise rejections.

# 🗁 Why Safe Error Handling Matters

Applications encounter failures from many sources:

* Invalid user input.
* Network interruptions.
* Database connection failures.
* Authentication and authorization problems.
* External API failures.
* Unexpected programming errors.
* File system and resource access failures.

Poor error handling can cause:

* Application crashes.
* Unhandled promise rejections.
* Information leakage.
* Difficult debugging.
* Inconsistent API responses.
* Hidden failures that affect application behavior.

Safe error handling ensures failures are handled predictably while preserving enough information for debugging.

# 🗁 Using Unknown in Catch Blocks

TypeScript treats caught values as `unknown` when `useUnknownInCatchVariables` is enabled.

```typescript
try {
  throw new Error("Something went wrong");
} catch (error) {
  if (error instanceof Error) {
    console.error(error.message);
  } else {
    console.error("Unknown error:", error);
  }
}
```

This is safer than assuming every thrown value is an instance of `Error`.

JavaScript allows throwing any value:

```typescript
throw new Error("Request failed");
throw "Request failed";
throw 500;
throw { message: "Request failed" };
```

Always validate an unknown value before accessing its properties.

# 🗁 Creating a Safe Error Message Function

A reusable function can normalize unknown error values.

```typescript
function getErrorMessage(error: unknown): string {
  if (error instanceof Error) {
    return error.message;
  }

  if (typeof error === "string") {
    return error;
  }

  return "An unexpected error occurred";
}
```

Usage:

```typescript
try {
  throw new Error("Database connection failed");
} catch (error) {
  console.error(getErrorMessage(error));
}
```

This provides a consistent way to extract messages without assuming the shape of the caught value.

# 🗁 Handling Expected and Unexpected Errors

Not all errors should be handled in the same way.

**Expected errors** are known failure conditions that application logic can anticipate.

Examples:

* Invalid email addresses.
* Missing resources.
* Insufficient permissions.
* Duplicate records.
* Invalid request parameters.

**Unexpected errors** represent failures that the application did not explicitly anticipate.

Examples:

* Programming bugs.
* Unexpected runtime exceptions.
* Unrecognized third-party library failures.
* Unexpected infrastructure failures.

A custom error hierarchy can help distinguish these conditions.

```typescript
class AppError extends Error {
  constructor(
    message: string,
    public statusCode: number,
    public code: string,
    options?: ErrorOptions
  ) {
    super(message, options);

    this.name = new.target.name;
  }
}

class ValidationError extends AppError {
  constructor(message: string) {
    super(message, 400, "VALIDATION_ERROR");
  }
}

class NotFoundError extends AppError {
  constructor(message: string) {
    super(message, 404, "NOT_FOUND");
  }
}
```

A centralized handler can process known application errors differently from unexpected errors.

```typescript
function handleError(error: unknown) {
  if (error instanceof AppError) {
    return {
      statusCode: error.statusCode,
      body: {
        code: error.code,
        message: error.message
      }
    };
  }

  return {
    statusCode: 500,
    body: {
      code: "INTERNAL_SERVER_ERROR",
      message: "An unexpected error occurred"
    }
  };
}
```

The internal error can still be logged separately for investigation.

# 🗁 Avoiding Sensitive Information Leakage

Error messages sometimes contain sensitive information.

Examples include:

* Database connection strings.
* API keys.
* Access tokens.
* Passwords.
* Internal file paths.
* Database queries.
* Server configuration.
* Internal stack traces.

Do not expose these details in public API responses.

Unsafe example:

```typescript
catch (error) {
  return {
    statusCode: 500,
    body: {
      message: String(error)
    }
  };
}
```

A safer approach:

```typescript
catch (error) {
  console.error("Internal application error:", error);

  return {
    statusCode: 500,
    body: {
      code: "INTERNAL_SERVER_ERROR",
      message: "An unexpected error occurred"
    }
  };
}
```

Keep detailed diagnostics in controlled server-side logs and return a safe, predictable response to clients.

# 🗁 Preserving Original Errors with Cause

When an error is caught and replaced with a higher-level error, preserve the original error using the `cause` property.

```typescript
class ServiceError extends Error {
  constructor(
    message: string,
    options?: ErrorOptions
  ) {
    super(message, options);

    this.name = "ServiceError";
  }
}
```

Example:

```typescript
async function getUsers() {
  try {
    throw new Error("Database connection refused");
  } catch (error) {
    throw new ServiceError(
      "Unable to retrieve users",
      { cause: error }
    );
  }
}
```

The original failure can be inspected later.

```typescript
try {
  await getUsers();
} catch (error) {
  if (error instanceof ServiceError) {
    console.error(error.message);
    console.error("Original cause:", error.cause);
  }
}
```

This preserves the relationship between high-level application failures and their underlying causes.

# 🗁 Safe Error Handling in Async Functions

Asynchronous operations can fail through rejected promises.

Use `try...catch` with `async` and `await` to handle these failures.

```typescript
async function fetchUsers() {
  try {
    const response = await fetch("/api/users");

    if (!response.ok) {
      throw new Error(
        `Request failed with status ${response.status}`
      );
    }

    return await response.json();
  } catch (error) {
    console.error("Unable to fetch users:", error);

    throw error;
  }
}
```

This example logs the failure and rethrows it.

Rethrowing allows the calling function to decide how the failure should affect the application.

Do not silently swallow errors unless ignoring the failure is an intentional part of the application logic.

# 🗁 Handling Errors at the Correct Layer

Error handling should happen at the layer that has enough context to make a meaningful decision.

Consider a typical application architecture:

```text
Controller
    |
    v
Service
    |
    v
Repository
    |
    v
Database
```

Responsibilities:

* **Repository:** Detect and report database operation failures.
* **Service:** Apply business rules and translate lower-level failures when appropriate.
* **Controller:** Convert application outcomes into HTTP responses.
* **Global error handler:** Handle errors that reach the application boundary.

Avoid catching the same error repeatedly without adding useful behavior.

For example, a repository should not convert every database failure into a successful empty result. Doing so hides the actual failure from higher layers.

# 🗁 Safe Error Handling in Promise Chains

Promises can use `.catch()` to handle rejected operations.

```typescript
fetch("/api/users")
  .then(response => {
    if (!response.ok) {
      throw new Error("Request failed");
    }

    return response.json();
  })
  .then(users => {
    console.log(users);
  })
  .catch(error => {
    console.error(getErrorMessage(error));
  });
```

A rejection handler catches errors from earlier promise operations in the chain, including errors thrown inside preceding `.then()` callbacks.

If the rejection handler itself throws or returns a rejected promise, the resulting promise remains rejected.

# 🗁 Handling Multiple Independent Operations

`Promise.all()` rejects when any input promise rejects.

If each operation should be handled independently, use `Promise.allSettled()`.

```typescript
const results = await Promise.allSettled([
  fetch("/api/users"),
  fetch("/api/products"),
  fetch("/api/orders")
]);

for (const result of results) {
  if (result.status === "fulfilled") {
    console.log("Success:", result.value);
  } else {
    console.error("Failure:", result.reason);
  }
}
```

`Promise.allSettled()` waits until every operation settles, regardless of whether it fulfills or rejects.

This is useful when partial success is acceptable.

# 🗁 Logging Errors Safely

Logging helps developers investigate application failures.

A useful error log may contain:

* Error name.
* Error message.
* Stack trace.
* Timestamp.
* Request identifier.
* Operation name.
* Relevant non-sensitive context.

Example:

```typescript
function logError(
  error: unknown,
  context: string
): void {
  if (error instanceof Error) {
    console.error({
      context,
      name: error.name,
      message: error.message,
      stack: error.stack,
      cause: error.cause
    });
  } else {
    console.error({
      context,
      message: "Non-Error value was thrown",
      value: error
    });
  }
}
```

In production, use a structured logging system and sanitize sensitive values before writing logs.

Avoid logging passwords, access tokens, session identifiers, or other secrets.

# 🗁 Avoiding Empty Catch Blocks

An empty `catch` block silently discards errors.

```typescript
try {
  await saveUser();
} catch (error) {
}
```

This makes failures difficult to detect and can leave the application in an unexpected state.

Handle the failure explicitly:

```typescript
try {
  await saveUser();
} catch (error) {
  logError(error, "saveUser");

  throw error;
}
```

If a failure is intentionally ignored, document why and ensure the application remains in a valid state.

# 🗁 Using Result Types Instead of Exceptions

Exceptions are not the only way to represent failure.

For operations where failure is an expected outcome, a discriminated union can provide an explicit result.

```typescript
type Result<T, E> =
  | {
      success: true;
      data: T;
    }
  | {
      success: false;
      error: E;
    };
```

Example:

```typescript
type User = {
  id: number;
  name: string;
};

type UserError = {
  code: "NOT_FOUND" | "INVALID_ID";
  message: string;
};

function findUser(id: number): Result<User, UserError> {
  if (id <= 0) {
    return {
      success: false,
      error: {
        code: "INVALID_ID",
        message: "User ID must be positive"
      }
    };
  }

  if (id !== 1) {
    return {
      success: false,
      error: {
        code: "NOT_FOUND",
        message: "User not found"
      }
    };
  }

  return {
    success: true,
    data: {
      id: 1,
      name: "John"
    }
  };
}
```

Usage:

```typescript
const result = findUser(2);

if (result.success) {
  console.log(result.data.name);
} else {
  console.log(result.error.message);
}
```

The discriminant `success` allows TypeScript to narrow the result automatically.

Result types are useful for expected business outcomes, while exceptions remain appropriate for exceptional failures and operations that naturally use exception-based APIs.

# 🗁 Best Practices

* Treat caught values as `unknown`.
* Narrow error values before accessing their properties.
* Distinguish expected errors from unexpected failures.
* Use custom error classes for meaningful application failures.
* Preserve original errors with `cause` when wrapping them.
* Handle promise rejections explicitly.
* Avoid empty catch blocks and silent failures.
* Do not expose stack traces or internal details to API consumers.
* Use centralized error handling at application boundaries.
* Log useful diagnostic information securely.
* Avoid catching errors repeatedly without adding value.
* Consider Result types for expected business-level failures.
* Keep error handling responsibilities clear across application layers.

# 🗁 Summary

Safe error handling makes applications more reliable, predictable, and maintainable.

Key concepts:

* Use `unknown` for caught error values.
* Narrow errors before accessing their properties.
* Separate expected and unexpected failures.
* Preserve original errors using `cause`.
* Handle asynchronous failures explicitly.
* Avoid exposing sensitive information.
* Use structured logging for debugging.
* Handle errors at the appropriate application layer.
* Use Result types when explicit failure values are preferable to exceptions.
