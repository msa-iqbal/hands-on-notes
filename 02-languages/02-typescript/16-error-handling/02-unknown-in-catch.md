# Unknown in Catch

In TypeScript, variables declared inside a `catch` clause are treated as `unknown` by default when `useUnknownInCatchVariables` is enabled.

This behavior encourages developers to inspect and narrow error values before accessing their properties.

It is an important part of TypeScript's type-safe error handling.

# 🗁 What Is Unknown in Catch?

JavaScript allows any value to be thrown, not just instances of the `Error` class.

For example:

```typescript
try {
  throw new Error("Something went wrong");
} catch (error) {
  console.log(error.message);
}
```

The thrown value is an `Error` instance, but JavaScript also allows:

```typescript
throw "Something went wrong";

throw 404;

throw null;

throw {
  message: "Something went wrong"
};
```

Because the thrown value can have any type, TypeScript treats the catch variable as `unknown`.

# 🗁 Why Catch Variables Use Unknown

Consider this example:

```typescript
try {
  throw "Something went wrong";
} catch (error) {
  console.log(error.message);
}
```

TypeScript reports an error because `error` has type `unknown`.

```text
'error' is of type 'unknown'.
```

The compiler cannot guarantee that the thrown value contains a `message` property.

Using `unknown` forces developers to verify the value before accessing its properties.

# 🗁 Unknown vs Any in Catch

The difference between `unknown` and `any` is important.

| Feature | `unknown` | `any` |
| --- | --- | --- |
| Accepts any value | Yes | Yes |
| Allows direct property access | No | Yes |
| Requires type narrowing | Yes | No |
| Provides type safety | Yes | No |
| Recommended for catch variables | Yes | No |

Example with `any`:

```typescript
try {
  throw "Something went wrong";
} catch (error: any) {
  console.log(error.message);
}
```

This compiles, but accessing `message` on a string produces `undefined`.

Example with `unknown`:

```typescript
try {
  throw "Something went wrong";
} catch (error: unknown) {
  if (error instanceof Error) {
    console.log(error.message);
  }
}
```

The second approach verifies the value before accessing its properties.

# 🗁 useUnknownInCatchVariables

The `useUnknownInCatchVariables` compiler option controls the type of catch variables.

It is enabled automatically when `strict` mode is enabled.

Example configuration:

```json
{
  "compilerOptions": {
    "strict": true,
    "useUnknownInCatchVariables": true
  }
}
```

You can also enable it independently:

```json
{
  "compilerOptions": {
    "useUnknownInCatchVariables": true
  }
}
```

To disable it:

```json
{
  "compilerOptions": {
    "strict": true,
    "useUnknownInCatchVariables": false
  }
}
```

Disabling this option makes catch variables default to `any`, reducing type safety.

# 🗁 Narrowing with instanceof

The most common way to handle caught errors is using `instanceof`.

```typescript
try {
  throw new Error("Database connection failed");
} catch (error) {
  if (error instanceof Error) {
    console.log(error.message);
    console.log(error.name);
  }
}
```

The `instanceof` check narrows `error` from `unknown` to `Error`.

You can also check for specific error subclasses:

```typescript
try {
  throw new TypeError("Invalid value");
} catch (error) {
  if (error instanceof TypeError) {
    console.log("Type error:", error.message);
  } else if (error instanceof Error) {
    console.log("General error:", error.message);
  }
}
```

# 🗁 Handling Non-Error Values

Because JavaScript allows arbitrary thrown values, error handling should account for values that are not instances of `Error`.

```typescript
function handleError(error: unknown): void {
  if (error instanceof Error) {
    console.log(error.message);
  } else if (typeof error === "string") {
    console.log(error);
  } else {
    console.log("An unknown error occurred");
  }
}

try {
  throw "Something went wrong";
} catch (error) {
  handleError(error);
}
```

This approach handles both standard errors and string values.

# 🗁 Narrowing with typeof

The `typeof` operator can identify primitive values.

```typescript
function handleError(error: unknown): void {
  if (typeof error === "string") {
    console.log("String error:", error);
  } else if (typeof error === "number") {
    console.log("Numeric error:", error);
  } else if (typeof error === "boolean") {
    console.log("Boolean error:", error);
  } else {
    console.log("Other error:", error);
  }
}
```

This is useful when working with APIs or JavaScript libraries that may throw non-Error values.

# 🗁 Narrowing Object Errors

Sometimes an application or third-party library throws an object that is not an `Error` instance.

You can inspect its properties using type guards.

```typescript
function hasMessage(
  error: unknown
): error is { message: string } {
  return (
    typeof error === "object" &&
    error !== null &&
    "message" in error &&
    typeof error.message === "string"
  );
}

function handleError(error: unknown): void {
  if (hasMessage(error)) {
    console.log(error.message);
  } else {
    console.log("Unknown error");
  }
}
```

The `hasMessage()` function verifies that the value is a non-null object containing a string `message` property.

# 🗁 Creating a Reusable Error Message Helper

Applications often need a consistent way to convert unknown error values into readable messages.

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
  throw new Error("Unable to load users");
} catch (error) {
  console.log(getErrorMessage(error));
}
```

Output:

```text
Unable to load users
```

This helper can be reused across services, controllers, and application layers.

# 🗁 Handling Errors in Async Functions

The same `unknown` behavior applies to asynchronous operations.

```typescript
async function getUsers(): Promise<void> {
  try {
    const response = await fetch("/api/users");

    if (!response.ok) {
      throw new Error("Failed to fetch users");
    }

    const users = await response.json();

    console.log(users);
  } catch (error) {
    if (error instanceof Error) {
      console.error(error.message);
    } else {
      console.error("Unknown failure:", error);
    }
  }
}
```

The `catch` variable remains `unknown`, even when the error originates from a rejected Promise.

# 🗁 Using Error Causes

When wrapping an unknown error, you can preserve the original value through the `cause` property.

```typescript
async function loadUsers(): Promise<void> {
  try {
    throw new Error("Database unavailable");
  } catch (error) {
    throw new Error("Unable to load users", {
      cause: error
    });
  }
}
```

The original error can later be inspected:

```typescript
try {
  await loadUsers();
} catch (error) {
  if (error instanceof Error) {
    console.log(error.message);
    console.log(error.cause);
  }
}
```

This preserves the original failure while providing a higher-level error message.

# 🗁 Best Practices

* Keep catch variables as `unknown`.
* Use `instanceof Error` before accessing standard error properties.
* Use `typeof` for primitive thrown values.
* Create reusable type guards for structured error objects.
* Avoid using `any` in catch clauses.
* Do not assume every thrown value has a `message` property.
* Preserve original errors when wrapping them.
* Use a centralized error-message helper when appropriate.
* Handle asynchronous failures with the same narrowing techniques.

# 🗁 Summary

TypeScript uses `unknown` for catch variables to prevent unsafe access to arbitrary thrown values.

Key concepts:

* JavaScript allows any value to be thrown.
* `unknown` prevents direct property access.
* `instanceof` narrows standard error objects.
* `typeof` narrows primitive values.
* Custom type guards validate object-shaped errors.
* `useUnknownInCatchVariables` controls catch-variable typing.
* The option is enabled automatically by `strict` mode.
* Reusable error helpers improve consistency.
* Error causes preserve the original failure.
