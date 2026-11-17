# Error Narrowing

Error narrowing is the process of checking an error value at runtime to determine its actual type and safely access its properties.

In TypeScript, values caught inside a `catch` block are treated as `unknown` when `useUnknownInCatchVariables` is enabled (which is the default under `strict` mode).

Because an `unknown` value can represent anything, TypeScript requires type checking before accessing its properties or methods.

# 🗁 Understanding Error Narrowing

Consider the following example:

```typescript
try {
  throw new Error("Something went wrong");
} catch (error) {
  console.log(error.message);
}
```

With strict error handling, TypeScript reports an error because `error` has type `unknown`.

You cannot directly access `.message` until the value has been narrowed.

# 🗁 Narrowing with instanceof

The `instanceof` operator checks whether a value is an instance of a particular class.

It is one of the most common ways to narrow error values.

```typescript
try {
  throw new Error("Something went wrong");
} catch (error) {
  if (error instanceof Error) {
    console.log(error.message);
    console.log(error.name);
    console.log(error.stack);
  }
}
```

Inside the `if` block, TypeScript knows that `error` is an `Error`.

You can also narrow custom error classes.

```typescript
class ValidationError extends Error {
  constructor(message: string) {
    super(message);

    this.name = "ValidationError";
  }
}

try {
  throw new ValidationError("Invalid email");
} catch (error) {
  if (error instanceof ValidationError) {
    console.log(error.message);
  }
}
```

# 🗁 Narrowing with typeof

The `typeof` operator identifies primitive types.

It is useful when a thrown value is not necessarily an `Error` instance.

JavaScript allows throwing any value:

```typescript
throw "Something went wrong";
throw 404;
throw { message: "Request failed" };
```

A safe handler can check the primitive type first.

```typescript
function handleError(error: unknown): void {
  if (typeof error === "string") {
    console.log("String error:", error);
  } else if (typeof error === "number") {
    console.log("Numeric error:", error);
  } else if (error instanceof Error) {
    console.log("Error:", error.message);
  }
}
```

`typeof` is especially useful for narrowing strings, numbers, booleans, symbols, bigints, functions, and objects.

# 🗁 Narrowing with the in Operator

The `in` operator checks whether a property exists on an object or anywhere in its prototype chain.

It can help narrow unknown object values.

```typescript
function getErrorMessage(error: unknown): string {
  if (
    typeof error === "object" &&
    error !== null &&
    "message" in error
  ) {
    return String(error.message);
  }

  return "Unknown error";
}
```

The checks are important:

* `typeof error === "object"` ensures the value is an object.
* `error !== null` excludes `null`, which JavaScript also reports as an object.
* `"message" in error` verifies that the property exists.

The `in` operator does not guarantee that the property contains a string. Converting it with `String()` ensures the function returns a string.

For stricter validation, check the property type:

```typescript
function getErrorMessage(error: unknown): string {
  if (
    typeof error === "object" &&
    error !== null &&
    "message" in error &&
    typeof error.message === "string"
  ) {
    return error.message;
  }

  return "Unknown error";
}
```

# 🗁 Narrowing with Type Predicates

A type predicate is a function that tells TypeScript whether a value belongs to a particular type.

It uses the `value is Type` return annotation.

```typescript
function isError(value: unknown): value is Error {
  return value instanceof Error;
}
```

Usage:

```typescript
try {
  throw new Error("Database unavailable");
} catch (error) {
  if (isError(error)) {
    console.log(error.message);
  }
}
```

Type predicates are useful when the same narrowing logic is required in multiple places.

# 🗁 Creating a Custom Error Guard

You can create reusable type guards for custom error classes.

```typescript
class ApiError extends Error {
  constructor(
    message: string,
    public statusCode: number
  ) {
    super(message);

    this.name = "ApiError";
  }
}

function isApiError(error: unknown): error is ApiError {
  return error instanceof ApiError;
}
```

Usage:

```typescript
try {
  throw new ApiError("User not found", 404);
} catch (error) {
  if (isApiError(error)) {
    console.log(error.statusCode);
    console.log(error.message);
  }
}
```

The type guard allows TypeScript to recognize `error` as `ApiError` inside the conditional block.

# 🗁 Narrowing Unknown Object Errors

Not every error originates from a JavaScript `Error` instance.

Third-party libraries, external APIs, and application code may throw plain objects.

You can define a structural type for such values.

```typescript
type ErrorLike = {
  message: string;
};

function isErrorLike(value: unknown): value is ErrorLike {
  return (
    typeof value === "object" &&
    value !== null &&
    "message" in value &&
    typeof value.message === "string"
  );
}
```

Usage:

```typescript
try {
  throw {
    message: "External service failed"
  };
} catch (error) {
  if (isErrorLike(error)) {
    console.log(error.message);
  }
}
```

This approach checks the actual structure rather than requiring a particular class instance.

# 🗁 Handling Multiple Error Types

An application may need to handle several error categories differently.

```typescript
class ValidationError extends Error {
  constructor(message: string) {
    super(message);
    this.name = "ValidationError";
  }
}

class NotFoundError extends Error {
  constructor(message: string) {
    super(message);
    this.name = "NotFoundError";
  }
}

class DatabaseError extends Error {
  constructor(message: string) {
    super(message);
    this.name = "DatabaseError";
  }
}
```

A handler can narrow each type separately.

```typescript
function handleError(error: unknown): void {
  if (error instanceof ValidationError) {
    console.log("Validation:", error.message);
  } else if (error instanceof NotFoundError) {
    console.log("Not found:", error.message);
  } else if (error instanceof DatabaseError) {
    console.log("Database:", error.message);
  } else if (error instanceof Error) {
    console.log("Unexpected error:", error.message);
  } else {
    console.log("Unknown error:", error);
  }
}
```

Check specific error classes before the general `Error` class.

# 🗁 Creating a Reusable Error Message Function

A reusable function can normalize different error values into a string.

```typescript
function getErrorMessage(error: unknown): string {
  if (error instanceof Error) {
    return error.message;
  }

  if (typeof error === "string") {
    return error;
  }

  if (
    typeof error === "object" &&
    error !== null &&
    "message" in error &&
    typeof error.message === "string"
  ) {
    return error.message;
  }

  return "An unexpected error occurred";
}
```

Usage:

```typescript
try {
  throw new Error("Connection failed");
} catch (error) {
  console.log(getErrorMessage(error));
}
```

This function handles:

* Standard JavaScript errors.
* Custom errors extending `Error`.
* String values.
* Plain objects containing a string `message` property.
* Other unknown values.

# 🗁 Error Narrowing with Discriminated Unions

Sometimes an application uses structured error objects instead of throwing class instances.

Discriminated unions provide another way to identify error categories.

```typescript
type AppError =
  | {
      type: "validation";
      message: string;
      field: string;
    }
  | {
      type: "not-found";
      message: string;
      resource: string;
    }
  | {
      type: "database";
      message: string;
      operation: string;
    };
```

A handler can narrow the union using its discriminant.

```typescript
function handleAppError(error: AppError): void {
  switch (error.type) {
    case "validation":
      console.log(error.field);
      break;

    case "not-found":
      console.log(error.resource);
      break;

    case "database":
      console.log(error.operation);
      break;
  }

  console.log(error.message);
}
```

This pattern is useful for predictable application error data.

These objects are structured error values, not necessarily instances of JavaScript's built-in `Error` class.

# 🗁 Best Practices

* Treat caught values as `unknown`.
* Use `instanceof Error` before accessing standard error properties.
* Use `typeof` for primitive error values.
* Check for `null` before inspecting unknown objects.
* Use the `in` operator to verify object properties.
* Validate property types instead of assuming their values.
* Create reusable type guards for repeated checks.
* Check specific custom errors before general error classes.
* Use discriminated unions for structured application error results.
* Avoid blindly converting arbitrary objects into error messages.

# 🗁 Summary

Error narrowing makes it possible to safely inspect unknown error values.

Key concepts:

* `unknown` requires runtime checks before property access.
* `instanceof` narrows class-based errors.
* `typeof` narrows primitive values.
* `in` checks whether an object contains a property.
* Type predicates create reusable custom type guards.
* Structural checks support plain object errors.
* Discriminated unions organize structured error values.
* Specific error checks should come before general error checks.
