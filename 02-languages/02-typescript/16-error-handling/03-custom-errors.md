# Custom Errors

Custom errors allow developers to define application-specific error types that represent particular failure conditions.

JavaScript provides the built-in `Error` class, but real applications often need more descriptive errors than a generic error message.

TypeScript allows custom error classes to extend `Error`, making it possible to create structured, reusable, and type-safe error handling.

Common use cases include:

* Authentication failures
* Authorization failures
* Validation errors
* Database errors
* API errors
* Resource not found errors
* Business rule violations
* Payment processing failures

# 🗁 What Are Custom Errors?

A custom error is a class that extends the built-in `Error` class.

Example:

```typescript
class ValidationError extends Error {
  constructor(message: string) {
    super(message);

    this.name = "ValidationError";
  }
}

throw new ValidationError("Email is required");
```

The custom error inherits the properties and methods of `Error`, including:

* `name`
* `message`
* `stack`
* `cause`

It can also define additional properties and methods.

# 🗁 Creating a Custom Error Class

The basic structure of a custom error class is:

```typescript
class CustomError extends Error {
  constructor(message: string) {
    super(message);

    this.name = "CustomError";
  }
}
```

Usage:

```typescript
try {
  throw new CustomError("Something went wrong");
} catch (error) {
  if (error instanceof CustomError) {
    console.log(error.name);
    console.log(error.message);
  }
}
```

Output:

```text
CustomError
Something went wrong
```

The `super()` call initializes the parent `Error` class.

Setting `this.name` gives the custom error a meaningful name.

# 🗁 Why Extend the Error Class?

Extending `Error` provides several benefits:

* Standard error properties are inherited.
* Errors can be identified using `instanceof`.
* Stack traces are available when supported by the runtime.
* Errors integrate naturally with JavaScript exception handling.
* Additional application-specific information can be attached.

Without extending `Error`, an object may represent a failure, but it will not behave like a standard JavaScript error.

# 🗁 Adding Additional Properties

Custom errors can include information beyond the error message.

```typescript
class HttpError extends Error {
  constructor(
    message: string,
    public statusCode: number,
    public code: string
  ) {
    super(message);

    this.name = "HttpError";
  }
}

const error = new HttpError(
  "User not found",
  404,
  "USER_NOT_FOUND"
);

console.log(error.message);
console.log(error.statusCode);
console.log(error.code);
```

Output:

```text
User not found
404
USER_NOT_FOUND
```

Additional properties make errors more useful for application logic and API responses.

# 🗁 Using Error Options and Cause

The `Error` constructor supports an options object containing `cause`.

This allows a custom error to preserve the original error that caused the failure.

```typescript
class DatabaseError extends Error {
  constructor(
    message: string,
    options?: ErrorOptions
  ) {
    super(message, options);

    this.name = "DatabaseError";
  }
}
```

Usage:

```typescript
try {
  throw new Error("Connection refused");
} catch (error) {
  throw new DatabaseError(
    "Unable to retrieve users",
    { cause: error }
  );
}
```

The original error remains accessible through `cause`.

```typescript
try {
  throw new DatabaseError(
    "Unable to retrieve users",
    {
      cause: new Error("Connection refused")
    }
  );
} catch (error) {
  if (error instanceof DatabaseError) {
    console.log(error.message);
    console.log(error.cause);
  }
}
```

This is useful when translating low-level infrastructure errors into higher-level application errors.

# 🗁 Creating a Custom Error Hierarchy

Large applications often define a hierarchy of custom errors.

A base application error can contain common properties shared by all application-specific errors.

```typescript
class AppError extends Error {
  constructor(
    message: string,
    public code: string,
    public statusCode: number,
    options?: ErrorOptions
  ) {
    super(message, options);

    this.name = new.target.name;
  }
}
```

Specific errors can extend this base class.

```typescript
class ValidationError extends AppError {
  constructor(message: string) {
    super(
      message,
      "VALIDATION_ERROR",
      400
    );
  }
}

class NotFoundError extends AppError {
  constructor(message: string) {
    super(
      message,
      "NOT_FOUND",
      404
    );
  }
}

class UnauthorizedError extends AppError {
  constructor(message: string) {
    super(
      message,
      "UNAUTHORIZED",
      401
    );
  }
}
```

Usage:

```typescript
const error = new NotFoundError("User not found");

console.log(error.name);
console.log(error.code);
console.log(error.statusCode);
```

Output:

```text
NotFoundError
NOT_FOUND
404
```

This approach provides consistent error structures throughout an application.

# 🗁 Using instanceof with Custom Errors

The `instanceof` operator identifies specific custom error types.

```typescript
function handleError(error: unknown): void {
  if (error instanceof ValidationError) {
    console.log("Validation failed:", error.message);
  } else if (error instanceof NotFoundError) {
    console.log("Resource missing:", error.message);
  } else if (error instanceof AppError) {
    console.log("Application error:", error.code);
  } else if (error instanceof Error) {
    console.log("Unexpected error:", error.message);
  } else {
    console.log("Unknown failure");
  }
}
```

Check more specific subclasses before their parent classes.

Otherwise, a parent-class check may match before the more specific handling logic is reached.

# 🗁 Custom Validation Errors

Validation errors represent invalid user input or data that violates application rules.

```typescript
class ValidationError extends Error {
  constructor(
    message: string,
    public field: string
  ) {
    super(message);

    this.name = "ValidationError";
  }
}

function validateUser(email: string): void {
  if (!email.includes("@")) {
    throw new ValidationError(
      "Invalid email address",
      "email"
    );
  }
}

try {
  validateUser("invalid-email");
} catch (error) {
  if (error instanceof ValidationError) {
    console.log(error.field);
    console.log(error.message);
  }
}
```

Output:

```text
email
Invalid email address
```

The `field` property identifies which input caused the validation failure.

# 🗁 Custom API Errors

Backend applications often need errors that represent HTTP response conditions.

```typescript
class ApiError extends Error {
  constructor(
    message: string,
    public statusCode: number,
    public details?: unknown
  ) {
    super(message);

    this.name = "ApiError";
  }
}
```

Example:

```typescript
function getUser(id: number) {
  if (id <= 0) {
    throw new ApiError(
      "Invalid user ID",
      400
    );
  }

  const user = null;

  if (!user) {
    throw new ApiError(
      "User not found",
      404,
      { id }
    );
  }
}
```

A centralized error handler can convert these errors into appropriate HTTP responses.

```typescript
function handleApiError(error: unknown) {
  if (error instanceof ApiError) {
    return {
      status: error.statusCode,
      body: {
        message: error.message,
        details: error.details
      }
    };
  }

  return {
    status: 500,
    body: {
      message: "Internal server error"
    }
  };
}
```

In production applications, internal error details should not automatically be exposed to API consumers.

# 🗁 Custom Database Errors

Database operations can fail for different reasons.

Custom errors can represent these failures separately.

```typescript
class DatabaseError extends Error {
  constructor(
    message: string,
    public operation: string,
    options?: ErrorOptions
  ) {
    super(message, options);

    this.name = "DatabaseError";
  }
}
```

Usage:

```typescript
async function findUser(id: number) {
  try {
    throw new Error("Database connection lost");
  } catch (error) {
    throw new DatabaseError(
      "Unable to find user",
      "findUser",
      { cause: error }
    );
  }
}
```

The `operation` property identifies which database operation failed, while `cause` preserves the underlying error.

# 🗁 Preserving the Prototype Chain

When extending built-in classes such as `Error`, modern JavaScript targets generally handle the prototype chain correctly.

For older JavaScript targets or unusual transpilation environments, prototype restoration may be necessary.

```typescript
class CustomError extends Error {
  constructor(message: string) {
    super(message);

    this.name = "CustomError";

    Object.setPrototypeOf(
      this,
      new.target.prototype
    );
  }
}
```

This helps ensure that `instanceof CustomError` works correctly in environments where built-in subclassing is not handled properly.

For modern projects targeting ES2015 or later, this additional step is generally unnecessary.

# 🗁 Best Practices

* Extend the built-in `Error` class when creating custom errors.
* Set a meaningful error name.
* Use specific error classes for distinct failure conditions.
* Create a shared base error for common application properties.
* Include useful error codes and structured details when needed.
* Preserve the original error using `cause`.
* Use `unknown` when accepting arbitrary caught values.
* Check specific error subclasses before their parent classes.
* Avoid exposing internal implementation details in public responses.
* Keep error classes focused on representing failures rather than handling them.
* Use a centralized error handler when an application has many error types.

# 🗁 Summary

Custom errors provide structured representations of application-specific failures.

Key concepts:

* Custom errors extend the built-in `Error` class.
* `super()` initializes the parent error.
* `this.name` identifies the custom error type.
* Additional properties provide structured error information.
* `ErrorOptions` supports preserving the original error through `cause`.
* Error hierarchies organize related failures.
* `instanceof` identifies custom error classes.
* Custom errors are useful for validation, APIs, databases, authentication, and business logic.
