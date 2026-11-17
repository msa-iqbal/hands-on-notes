# Try...Catch

JavaScript uses `try...catch` to handle runtime errors without abruptly terminating the normal execution flow.

It allows you to:

* Execute code that might throw an error.
* Catch and inspect errors.
* Recover from failures.
* Keep an application running when an operation fails.
* Separate error-handling logic from normal application logic.

# 🗁 Basic Syntax

```javascript
try {
  // Code that might throw an error
} catch (error) {
  // Handle the error
}
```

The `try` block contains code that may fail.

The `catch` block executes only when an exception is thrown inside the corresponding `try` block.

```javascript
try {
  const result = 10 / 0;

  console.log(result);
} catch (error) {
  console.log("An error occurred:", error.message);
}
```

Division by zero does not throw an error in JavaScript. It produces `Infinity`, so the `catch` block does not execute.

A proper example:

```javascript
try {
  const user = JSON.parse("{ invalid json }");

  console.log(user);
} catch (error) {
  console.log("Failed to parse JSON:", error.message);
}
```

# 🗁 How It Works

JavaScript executes the `try` block sequentially.

* If no exception occurs, the `catch` block is skipped.
* If an exception occurs, execution of the remaining statements in the `try` block stops.
* JavaScript transfers control to the `catch` block.
* The caught error becomes available through the catch parameter.
* After the `catch` block finishes, execution continues after the entire `try...catch` statement.

```javascript
console.log("Start");

try {
  console.log("Inside try");

  throw new Error("Something went wrong");

  console.log("This will not execute");
} catch (error) {
  console.log("Inside catch:", error.message);
}

console.log("End");
```

Output:

```text
Start
Inside try
Inside catch: Something went wrong
End
```

# 🗁 The Catch Parameter

The `catch` block receives the thrown value through its parameter.

By convention, developers commonly name it `error` or `err`.

```javascript
try {
  throw new Error("Database connection failed");
} catch (error) {
  console.log(error);
  console.log(error.name);
  console.log(error.message);
}
```

The parameter can contain any JavaScript value, not necessarily an `Error` object.

```javascript
try {
  throw "Something went wrong";
} catch (error) {
  console.log(error);
}
```

JavaScript allows throwing strings, numbers, objects, and other values. However, throwing an `Error` object is generally recommended because it provides useful properties such as `name`, `message`, and `stack`.

# 🗁 Optional Catch Binding

Modern JavaScript allows you to omit the catch parameter when you do not need the error value.

```javascript
try {
  JSON.parse("{ invalid json }");
} catch {
  console.log("Invalid JSON data");
}
```

This feature is called optional catch binding.

It is useful when the application only needs to respond to an error without inspecting its details.

# 🗁 The Finally Block

The `finally` block executes after the `try` and `catch` processing, regardless of whether an exception occurred.

Syntax:

```javascript
try {
  // Code that might fail
} catch (error) {
  // Handle the error
} finally {
  // Always execute cleanup logic
}
```

Example:

```javascript
try {
  console.log("Opening a resource");

  throw new Error("Operation failed");
} catch (error) {
  console.log("Error:", error.message);
} finally {
  console.log("Closing the resource");
}
```

Output:

```text
Opening a resource
Error: Operation failed
Closing the resource
```

When no error occurs:

```javascript
try {
  console.log("Operation successful");
} catch (error) {
  console.log("Error:", error.message);
} finally {
  console.log("Cleanup completed");
}
```

Output:

```text
Operation successful
Cleanup completed
```

# 🗁 Practical Use Cases for Finally

The `finally` block is particularly useful for cleanup operations.

Examples include:

* Closing files or connections.
* Releasing locks.
* Hiding loading indicators.
* Resetting temporary state.
* Cleaning up resources.

```javascript
let isLoading = true;

try {
  const data = JSON.parse('{"name": "John"}');

  console.log(data);
} catch (error) {
  console.error("Failed to process data:", error.message);
} finally {
  isLoading = false;

  console.log("Loading completed");
}
```

# 🗁 Error Propagation

If an error occurs inside a `try` block and the corresponding `catch` handles it, the error does not automatically propagate to an outer scope.

However, if an error is not caught, it propagates through the call stack until a suitable handler is found.

```javascript
function getUser() {
  throw new Error("User not found");
}

function getProfile() {
  return getUser();
}

try {
  getProfile();
} catch (error) {
  console.log("Handled:", error.message);
}
```

Output:

```text
Handled: User not found
```

The error originates in `getUser()`, passes through `getProfile()`, and is handled by the outer `try...catch`.

# 🗁 Nested Try...Catch

A `try...catch` statement can be placed inside another `try` or `catch` block.

```javascript
try {
  console.log("Outer try");

  try {
    throw new Error("Inner operation failed");
  } catch (error) {
    console.log("Inner catch:", error.message);
  }

  console.log("Outer try continues");
} catch (error) {
  console.log("Outer catch:", error.message);
}
```

Output:

```text
Outer try
Inner catch: Inner operation failed
Outer try continues
```

The inner `catch` handles the exception, so it does not reach the outer `catch`.

If the inner block does not handle an error, the exception can propagate to the outer handler.

```javascript
try {
  try {
    throw new Error("Something failed");
  } catch (error) {
    console.log("Inner handler");

    throw error;
  }
} catch (error) {
  console.log("Outer handler:", error.message);
}
```

# 🗁 Try...Catch with Functions

A `try...catch` statement can handle errors thrown by functions called synchronously inside its `try` block.

```javascript
function divide(a, b) {
  if (b === 0) {
    throw new Error("Division by zero is not allowed");
  }

  return a / b;
}

try {
  const result = divide(10, 0);

  console.log(result);
} catch (error) {
  console.log(error.message);
}
```

Output:

```text
Division by zero is not allowed
```

The handler catches the exception even though it originates inside another function.

# 🗁 Try...Catch and Asynchronous Code

A synchronous `try...catch` does not automatically catch errors thrown later inside asynchronous callbacks.

For example:

```javascript
try {
  setTimeout(() => {
    throw new Error("Async operation failed");
  }, 1000);
} catch (error) {
  console.log("Caught:", error.message);
}
```

The error occurs after the synchronous `try...catch` execution has finished. Therefore, the `catch` block does not handle it.

For Promise-based operations, use `.catch()` or `async/await` with `try...catch`.

Using Promise `.catch()`:

```javascript
fetch("/api/users")
  .then(response => response.json())
  .catch(error => {
    console.error("Request failed:", error);
  });
```

Using `async/await`:

```javascript
async function getUsers() {
  try {
    const response = await fetch("/api/users");

    const users = await response.json();

    console.log(users);
  } catch (error) {
    console.error("Request failed:", error);
  }
}
```

The `await` expression allows Promise rejections to be handled through the surrounding `try...catch`.

# 🗁 Common Mistakes

**Using try...catch around code that cannot throw**

```javascript
try {
  const result = 10 + 20;
} catch (error) {
  console.log(error);
}
```

There is no meaningful error-handling requirement here. Avoid unnecessary `try...catch` blocks.

**Ignoring errors completely**

```javascript
try {
  performOperation();
} catch (error) {
  // Do nothing
}
```

Silently swallowing errors makes debugging and troubleshooting difficult.

**Assuming every thrown value is an Error object**

```javascript
try {
  throw "Failed";
} catch (error) {
  console.log(error.message);
}
```

Here, `error` is a string, so `error.message` is `undefined`.

Prefer throwing `Error` objects.

**Expecting try...catch to catch asynchronous callback errors**

```javascript
try {
  setTimeout(() => {
    throw new Error("Failed");
  }, 1000);
} catch (error) {
  console.log(error);
}
```

The callback executes later, outside the original synchronous execution flow.

# 🗁 Best Practices

* Catch errors only when you can handle them meaningfully.
* Prefer `Error` objects instead of throwing primitive values.
* Use descriptive error messages.
* Preserve useful error information when rethrowing.
* Use `finally` for cleanup operations.
* Avoid deeply nested `try...catch` blocks when simpler control flow is possible.
* Do not silently ignore unexpected errors.
* Use `async/await` with `try...catch` for asynchronous Promise-based operations.
* Keep error handling close to the layer that can make a meaningful recovery decision.
* Avoid exposing internal implementation details in user-facing error messages.

# 🗁 Try...Catch vs Throw

| Feature | try...catch | throw |
| --- | --- | --- |
| Purpose | Handles exceptions | Raises an exception |
| Responsibility | Error handling | Error generation |
| Execution | Defines a protected execution region | Immediately transfers control to an applicable handler |
| Common usage | Recovery and reporting | Signaling exceptional conditions |

Example combining both:

```javascript
function getUser(id) {
  if (!id) {
    throw new Error("User ID is required");
  }

  return {
    id,
    name: "John"
  };
}

try {
  const user = getUser(null);

  console.log(user);
} catch (error) {
  console.error("Unable to get user:", error.message);
}
```

# 🗁 Key Takeaways

* `try` contains code that may throw an exception.
* `catch` handles exceptions thrown inside the corresponding `try` block.
* The catch parameter receives the thrown value.
* Optional catch binding allows omitting the parameter.
* `finally` executes during normal completion and exception handling.
* Errors can propagate through function calls until handled.
* Nested `try...catch` blocks support multiple levels of error handling.
* Synchronous `try...catch` does not automatically catch errors from later asynchronous callbacks.
* Promise rejections can be handled with `.catch()` or `async/await` and `try...catch`.
