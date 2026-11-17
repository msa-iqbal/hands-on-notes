# Async Error Handling

Error handling is an essential part of asynchronous JavaScript programming.

Asynchronous operations can fail for many reasons:

* Network connection failures.
* HTTP errors.
* Database connection failures.
* Invalid API responses.
* Rejected Promises.
* Timeouts.
* Unexpected exceptions during asynchronous execution.

JavaScript provides several mechanisms for handling these failures:

* `.catch()` for handling Promise rejections.
* `try...catch` for handling exceptions in async functions.
* `finally` for cleanup operations.
* `throw` for generating errors.
* `Promise.allSettled()` for collecting multiple operation outcomes.
* `AbortController` for cancelling supported asynchronous operations.

# 🗁 Promise Rejection

A Promise can be fulfilled or rejected.

A rejected Promise represents an asynchronous operation that failed.

```javascript
const promise = new Promise((resolve, reject) => {
  reject(new Error("Something went wrong"));
});

promise.catch(error => {
  console.error(error.message);
});

// Something went wrong
```

The rejection reason is passed to the rejection handler.

# 🗁 Handling Errors with catch()

The `.catch()` method handles a rejected Promise.

**Syntax**

```javascript
promise.catch(error => {
  // Handle error
});
```

**Example**

```javascript
function getUser() {
  return fetch("/api/users/1");
}

getUser()
  .then(response => response.json())
  .catch(error => {
    console.error("Request failed:", error.message);
  });
```

If any Promise in the chain rejects, the rejection can be handled by the nearest applicable `.catch()`.

**Handling errors in a Promise chain**

```javascript
fetch("/api/users/1")
  .then(response => {
    if (!response.ok) {
      throw new Error(`HTTP Error: ${response.status}`);
    }

    return response.json();
  })
  .then(user => {
    console.log(user);
  })
  .catch(error => {
    console.error("Failed:", error.message);
  });
```

An exception thrown inside a `.then()` callback automatically rejects the Promise returned by that callback.

# 🗁 Handling Errors with try...catch

When using async/await, `try...catch` provides a straightforward way to handle rejected Promises.

**Syntax**

```javascript
try {
  // Code that may fail
} catch (error) {
  // Handle error
}
```

**Example**

```javascript
async function getUser() {
  try {
    const response = await fetch("/api/users/1");

    if (!response.ok) {
      throw new Error("Failed to fetch user");
    }

    const user = await response.json();

    return user;
  } catch (error) {
    console.error(error.message);
  }
}

getUser();
```

When an awaited Promise rejects, JavaScript throws the rejection reason inside the async function.

The `catch` block handles that exception.

# 🗁 Handling Errors with finally

The `finally` block executes after `try` and `catch`, regardless of whether an error occurred.

It is commonly used for cleanup operations.

**Syntax**

```javascript
try {
  // Main operation
} catch (error) {
  // Error handling
} finally {
  // Cleanup
}
```

**Example**

```javascript
async function loadUsers() {
  try {
    console.log("Loading users...");

    const response = await fetch("/api/users");

    if (!response.ok) {
      throw new Error("Unable to load users");
    }

    return await response.json();
  } catch (error) {
    console.error(error.message);
  } finally {
    console.log("Loading process finished");
  }
}

loadUsers();
```

Common uses include:

* Hiding loading indicators.
* Closing connections.
* Releasing resources.
* Resetting temporary state.
* Cleaning up event listeners.

# 🗁 Throwing Errors in Async Functions

The `throw` statement generates an exception.

Inside an async function, a thrown error causes the function's returned Promise to reject.

```javascript
async function getUser(userId) {
  if (!userId) {
    throw new Error("User ID is required");
  }

  return {
    id: userId,
    name: "John"
  };
}

getUser(null).catch(error => {
  console.error(error.message);
});
```

The async function converts the thrown exception into a rejected Promise.

# 🗁 Rethrowing Errors

Sometimes an error should be handled partially and then passed to the caller.

This is called rethrowing an error.

```javascript
async function getUser() {
  try {
    const response = await fetch("/api/users/1");

    if (!response.ok) {
      throw new Error("User request failed");
    }

    return await response.json();
  } catch (error) {
    console.error("Logging error:", error.message);

    throw error;
  }
}

getUser().catch(error => {
  console.error("Caller received:", error.message);
});
```

Rethrowing preserves the failure instead of silently converting it into a successful result.

# 🗁 Custom Error Handling

Custom errors allow applications to represent different failure conditions with meaningful error types.

```javascript
class ApiError extends Error {
  constructor(message, status) {
    super(message);

    this.name = "ApiError";
    this.status = status;
  }
}

async function getUser() {
  const response = await fetch("/api/users/1");

  if (!response.ok) {
    throw new ApiError(
      "Failed to retrieve user",
      response.status
    );
  }

  return response.json();
}

getUser().catch(error => {
  if (error instanceof ApiError) {
    console.error("API error:", error.status);
  } else {
    console.error("Unexpected error:", error);
  }
});
```

Custom errors are useful when different failures require different recovery strategies.

# 🗁 Handling Multiple Promises

When multiple asynchronous operations run together, their error-handling behavior depends on the Promise combinator being used.

**Promise.all()**

`Promise.all()` rejects when any input Promise rejects.

```javascript
async function loadDashboard() {
  try {
    const [users, posts, comments] = await Promise.all([
      getUsers(),
      getPosts(),
      getComments()
    ]);

    return { users, posts, comments };
  } catch (error) {
    console.error("Dashboard loading failed:", error);
    throw error;
  }
}
```

Use it when every operation must succeed.

**Promise.allSettled()**

`Promise.allSettled()` waits for every Promise to settle, even if some operations fail.

```javascript
async function loadResources() {
  const results = await Promise.allSettled([
    getUsers(),
    getPosts(),
    getComments()
  ]);

  for (const result of results) {
    if (result.status === "fulfilled") {
      console.log("Success:", result.value);
    } else {
      console.error("Failure:", result.reason);
    }
  }
}

loadResources();
```

Use it when partial success is acceptable and every outcome matters.

# 🗁 Handling Errors in Promise Chains

Errors can occur at different stages of a Promise chain.

```javascript
fetch("/api/users/1")
  .then(response => {
    if (!response.ok) {
      throw new Error("HTTP request failed");
    }

    return response.json();
  })
  .then(user => {
    if (!user.name) {
      throw new Error("Invalid user data");
    }

    return user;
  })
  .then(user => {
    console.log("User:", user);
  })
  .catch(error => {
    console.error("Operation failed:", error.message);
  })
  .finally(() => {
    console.log("Operation completed");
  });
```

A single `.catch()` near the end can handle failures from earlier stages.

For more precise recovery, individual `.catch()` handlers can be added where needed.

# 🗁 Handling Errors from Nested Async Functions

An async function can call another async function.

Errors from the inner function propagate to the caller when they are not handled internally.

```javascript
async function getUser() {
  const response = await fetch("/api/users/1");

  if (!response.ok) {
    throw new Error("Unable to fetch user");
  }

  return response.json();
}

async function displayUser() {
  try {
    const user = await getUser();

    console.log(user);
  } catch (error) {
    console.error("Display operation failed:", error.message);
  }
}

displayUser();
```

This approach allows lower-level functions to report failures while higher-level functions decide how to handle them.

# 🗁 Avoiding Unhandled Promise Rejections

An unhandled Promise rejection occurs when a Promise rejects and no rejection handler is attached.

```javascript
async function getData() {
  throw new Error("Data loading failed");
}

getData();
```

No handler is attached to the Promise returned by `getData()`.

A better approach is:

```javascript
getData().catch(error => {
  console.error(error.message);
});
```

Or use an async function with `try...catch`:

```javascript
async function main() {
  try {
    await getData();
  } catch (error) {
    console.error(error.message);
  }
}

main();
```

Always ensure that rejected Promises are handled at an appropriate application boundary.

# 🗁 Global Promise Rejection Handling

Browsers and Node.js provide mechanisms for observing unhandled Promise rejections.

**Browser**

```javascript
window.addEventListener("unhandledrejection", event => {
  console.error("Unhandled rejection:", event.reason);
});
```

The event exposes the rejection reason through `event.reason`.

Calling `event.preventDefault()` can suppress the browser's default reporting behavior for that event, but it does not repair the failed operation.

**Node.js**

```javascript
process.on("unhandledRejection", reason => {
  console.error("Unhandled rejection:", reason);
});
```

Global handlers are useful for logging and diagnostics.

They should not replace local error handling or application-level recovery logic.

# 🗁 Cancelling Asynchronous Operations

Some asynchronous operations support cancellation through `AbortController`.

This is useful when:

* A user navigates away from a page.
* A request takes too long.
* A newer request makes an older request unnecessary.
* An operation is no longer relevant.

**Example**

```javascript
async function getUsers() {
  const controller = new AbortController();

  const timeoutId = setTimeout(() => {
    controller.abort();
  }, 5000);

  try {
    const response = await fetch("/api/users", {
      signal: controller.signal
    });

    if (!response.ok) {
      throw new Error(`HTTP Error: ${response.status}`);
    }

    return await response.json();
  } catch (error) {
    if (error.name === "AbortError") {
      console.error("Request was cancelled");
    } else {
      console.error("Request failed:", error.message);
    }

    throw error;
  } finally {
    clearTimeout(timeoutId);
  }
}
```

`AbortController` provides cancellation for APIs that support its `AbortSignal`.

Aborting a request does not guarantee that a remote server has stopped processing work it already received.

# 🗁 Error Handling Strategy

A well-structured application should handle errors at appropriate layers.

**Service layer**

The service layer performs API requests and reports failures.

```javascript
async function getUser(id) {
  const response = await fetch(`/api/users/${id}`);

  if (!response.ok) {
    throw new Error(`Failed to fetch user: ${response.status}`);
  }

  return response.json();
}
```

**Application layer**

The application layer decides how to respond to failures.

```javascript
async function loadUserProfile(id) {
  try {
    const user = await getUser(id);

    console.log("User profile:", user);
  } catch (error) {
    console.error("Unable to load profile:", error.message);
  }
}
```

**Presentation layer**

The presentation layer displays appropriate feedback to users.

```javascript
async function handleProfilePage(id) {
  const messageElement = document.querySelector("#message");

  try {
    messageElement.textContent = "Loading profile...";

    const user = await getUser(id);

    messageElement.textContent = `Welcome, ${user.name}`;
  } catch (error) {
    messageElement.textContent = "Unable to load your profile.";
  }
}
```

This separation keeps API logic, application decisions, and user interface behavior independent.

# 🗁 Common Error Handling Mistakes

**Swallowing errors**

```javascript
async function getUser() {
  try {
    return await fetch("/api/users/1");
  } catch (error) {
    console.error(error);
  }
}
```

The function resolves with `undefined` after logging the error.

If the caller needs to know about the failure, rethrow it.

```javascript
async function getUser() {
  try {
    return await fetch("/api/users/1");
  } catch (error) {
    console.error(error);
    throw error;
  }
}
```

**Assuming fetch rejects for HTTP errors**

```javascript
const response = await fetch("/api/users/1");

const user = await response.json();
```

A `404` or `500` response does not normally reject the fetch Promise.

Check `response.ok` before processing the response body.

**Returning inside finally**

```javascript
async function example() {
  try {
    return "Success";
  } finally {
    return "Finished";
  }
}
```

The `finally` return overrides the earlier return value.

The function fulfills with `"Finished"`.

Avoid returning or throwing from `finally` unless overriding the previous outcome is intentional.

**Not awaiting a Promise inside try...catch**

```javascript
async function getData() {
  try {
    return fetch("/api/users");
  } catch (error) {
    console.error(error);
  }
}
```

The `try...catch` does not catch a later rejection from the returned Promise because it is not awaited inside the block.

Correct approach:

```javascript
async function getData() {
  try {
    return await fetch("/api/users");
  } catch (error) {
    console.error(error);
    throw error;
  }
}
```

Alternatively, return the Promise and handle its rejection at the caller.

# 🗁 Best Practices

* Handle errors at the layer that has enough context to respond appropriately.
* Use meaningful error messages.
* Throw `Error` objects rather than arbitrary strings.
* Preserve the original error when rethrowing.
* Check HTTP response status when using `fetch()`.
* Use `Promise.allSettled()` when partial success is acceptable.
* Use `AbortController` for supported cancellable operations.
* Avoid silently swallowing errors.
* Keep cleanup logic inside `finally`.
* Use custom error classes when error categories need different handling.
* Log useful diagnostic information without exposing sensitive data.
* Use global rejection handlers for monitoring, not as a replacement for local handling.

# 🗁 Key Takeaways

* Promise rejections represent asynchronous failures.
* `.catch()` handles Promise rejections.
* `try...catch` handles exceptions from awaited operations.
* `finally` executes cleanup code regardless of success or failure.
* `throw` generates an exception and rejects an async function's returned Promise.
* Errors can propagate through nested async functions.
* `Promise.all()` rejects when an input Promise rejects.
* `Promise.allSettled()` collects all outcomes.
* `AbortController` cancels supported asynchronous operations.
* Proper error handling improves application reliability and maintainability.
