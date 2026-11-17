# Async/Await

`async` and `await` are JavaScript keywords that make asynchronous code easier to write, read, and maintain.

They provide a cleaner syntax for working with Promises without requiring long chains of `.then()` and `.catch()`.

Async/await does not replace Promises. It is built on top of the Promise mechanism.

**Without async/await**

```javascript
function getUser() {
  return fetch("/api/users/1")
    .then(response => response.json())
    .then(user => {
      console.log(user);
      return user;
    })
    .catch(error => {
      console.error(error);
    });
}
```

**With async/await**

```javascript
async function getUser() {
  try {
    const response = await fetch("/api/users/1");
    const user = await response.json();

    console.log(user);

    return user;
  } catch (error) {
    console.error(error);
  }
}
```

The second version reads more like synchronous code while remaining asynchronous.

# 🗁 The async Keyword

The `async` keyword declares an asynchronous function.

An async function always returns a Promise.

**Syntax**

```javascript
async function functionName() {
  // Asynchronous operations
}
```

**Example**

```javascript
async function greet() {
  return "Hello, JavaScript!";
}

const result = greet();

console.log(result);

// Promise { "Hello, JavaScript!" }
```

Even though the function returns a regular string, JavaScript automatically wraps the value in a fulfilled Promise.

To access the result:

```javascript
greet().then(message => {
  console.log(message);
});

// Hello, JavaScript!
```

**Returning a value**

```javascript
async function getUser() {
  return {
    id: 1,
    name: "John"
  };
}

getUser().then(user => {
  console.log(user.name);
});

// John
```

**Returning an existing Promise**

```javascript
async function getData() {
  return Promise.resolve("Data loaded");
}

getData().then(data => {
  console.log(data);
});

// Data loaded
```

The async function adopts the state of the returned Promise.

# 🗁 Async Function Expressions

Async functions can also be written as function expressions.

```javascript
const getMessage = async function () {
  return "Hello from an async function";
};

getMessage().then(message => {
  console.log(message);
});
```

# 🗁 Async Arrow Functions

Arrow functions can use the `async` keyword.

```javascript
const getMessage = async () => {
  return "Hello from an async arrow function";
};

getMessage().then(message => {
  console.log(message);
});
```

For a concise return:

```javascript
const getNumber = async () => 100;

getNumber().then(number => {
  console.log(number);
});

// 100
```

# 🗁 The await Keyword

The `await` keyword pauses the execution of the surrounding async function until the awaited Promise settles.

If the Promise fulfills, `await` evaluates to its fulfillment value.

If the Promise rejects, `await` throws the rejection reason as an exception.

**Syntax**

```javascript
const result = await promise;
```

**Example**

```javascript
function getData() {
  return new Promise(resolve => {
    setTimeout(() => {
      resolve("Data loaded");
    }, 2000);
  });
}

async function displayData() {
  console.log("Loading...");

  const result = await getData();

  console.log(result);
}

displayData();

// Loading...
// Data loaded
```

The async function waits for `getData()` to fulfill before executing the next statement.

# 🗁 await with Promise.resolve()

`await` can work with any value, not only explicitly created Promises.

When given a non-Promise value, JavaScript behaves as if the value were wrapped in a fulfilled Promise.

```javascript
async function example() {
  const result = await 100;

  console.log(result);
}

example();

// 100
```

Using `Promise.resolve()`:

```javascript
async function example() {
  const result = await Promise.resolve("Success");

  console.log(result);
}

example();

// Success
```

# 🗁 Sequential Execution

When multiple `await` expressions are written sequentially, each operation waits for the previous one to finish.

```javascript
function wait(milliseconds) {
  return new Promise(resolve => {
    setTimeout(resolve, milliseconds);
  });
}

async function processTasks() {
  console.log("Task 1 started");

  await wait(1000);

  console.log("Task 1 completed");

  await wait(1000);

  console.log("Task 2 completed");
}

processTasks();
```

The second task begins only after the first wait completes.

This is useful when operations depend on the results of previous operations.

**Example: Dependent API requests**

```javascript
async function getUserPosts() {
  const userResponse = await fetch("/api/users/1");
  const user = await userResponse.json();

  const postsResponse = await fetch(`/api/users/${user.id}/posts`);
  const posts = await postsResponse.json();

  return posts;
}
```

The posts request depends on the user information, so sequential execution is appropriate.

# 🗁 Parallel Execution with Promise.all()

Independent asynchronous operations do not always need to run sequentially.

Using `await` separately can unnecessarily increase total execution time.

**Sequential approach**

```javascript
async function loadData() {
  const users = await fetch("/api/users").then(response => response.json());

  const posts = await fetch("/api/posts").then(response => response.json());

  return { users, posts };
}
```

The posts request starts only after the users request completes.

**Parallel approach**

```javascript
async function loadData() {
  const [usersResponse, postsResponse] = await Promise.all([
    fetch("/api/users"),
    fetch("/api/posts")
  ]);

  const [users, posts] = await Promise.all([
    usersResponse.json(),
    postsResponse.json()
  ]);

  return { users, posts };
}
```

Both network requests start concurrently.

Use `Promise.all()` when multiple operations are independent and all results are required.

# 🗁 Async/Await with try...catch

Rejected Promises can be handled using ordinary `try...catch` blocks.

When an awaited Promise rejects, JavaScript throws its rejection reason.

**Example**

```javascript
async function getUser() {
  try {
    const response = await fetch("/api/users/1");
    const user = await response.json();

    return user;
  } catch (error) {
    console.error("Failed to load user:", error.message);
  }
}

getUser();
```

The `catch` block handles errors from the awaited operations.

**Using finally**

The `finally` block executes whether the operation succeeds or fails.

```javascript
async function loadData() {
  try {
    console.log("Loading...");

    const response = await fetch("/api/users");
    const users = await response.json();

    return users;
  } catch (error) {
    console.error("Request failed:", error);
  } finally {
    console.log("Loading process finished");
  }
}

loadData();
```

This is useful for cleanup operations, such as hiding loading indicators.

# 🗁 Error Handling with HTTP Responses

An important detail when using `fetch()` is that HTTP error responses do not automatically reject the Promise.

For example, HTTP status codes such as `404` and `500` normally produce fulfilled fetch Promises containing a `Response` object.

Check `response.ok` to detect unsuccessful HTTP responses.

```javascript
async function getUser() {
  try {
    const response = await fetch("/api/users/1");

    if (!response.ok) {
      throw new Error(`HTTP Error: ${response.status}`);
    }

    const user = await response.json();

    return user;
  } catch (error) {
    console.error(error.message);
    throw error;
  }
}
```

The explicit `throw` converts an unsuccessful HTTP response into an exception that can be handled by `catch`.

# 🗁 Async Methods in Objects

Object methods can also be asynchronous.

```javascript
const userService = {
  async getUser(id) {
    const response = await fetch(`/api/users/${id}`);

    if (!response.ok) {
      throw new Error("Failed to fetch user");
    }

    return response.json();
  }
};

userService.getUser(1)
  .then(user => console.log(user))
  .catch(error => console.error(error));
```

# 🗁 Async Methods in Classes

Class methods can use `async` and `await`.

```javascript
class UserService {
  async getUser(id) {
    const response = await fetch(`/api/users/${id}`);

    if (!response.ok) {
      throw new Error("User not found");
    }

    return response.json();
  }
}

const userService = new UserService();

async function displayUser() {
  const user = await userService.getUser(1);

  console.log(user);
}

displayUser();
```

# 🗁 Async Callbacks

Callbacks can also be asynchronous.

```javascript
const numbers = [1, 2, 3];

numbers.forEach(async number => {
  await new Promise(resolve => setTimeout(resolve, 1000));

  console.log(number);
});
```

However, `forEach()` does not wait for asynchronous callbacks to finish.

If you need to wait for all operations, use `Promise.all()` with `map()`:

```javascript
const numbers = [1, 2, 3];

async function processNumbers() {
  await Promise.all(
    numbers.map(async number => {
      await new Promise(resolve => setTimeout(resolve, 1000));

      console.log(number);
    })
  );

  console.log("All tasks completed");
}

processNumbers();
```

# 🗁 Async Iteration with for...of

Use `for...of` with `await` when asynchronous operations must run sequentially.

```javascript
const users = [1, 2, 3];

async function processUsers() {
  for (const id of users) {
    const response = await fetch(`/api/users/${id}`);
    const user = await response.json();

    console.log(user);
  }
}

processUsers();
```

Each request waits for the previous iteration to complete.

For independent requests, use `Promise.all()` instead.

# 🗁 Top-Level await

Top-level `await` allows `await` to be used outside an async function in JavaScript modules.

It is supported in ECMAScript modules, not ordinary classic scripts or CommonJS modules.

**Example**

```javascript
// app.js

const response = await fetch("/api/users");

const users = await response.json();

console.log(users);
```

The module's evaluation waits for the awaited operation to complete.

Top-level await is useful for module initialization, but it can delay modules that depend on the module being evaluated.

# 🗁 Common Async/Await Mistakes

**Forgetting await**

```javascript
async function getUser() {
  const user = fetch("/api/users/1");

  console.log(user);
}
```

The variable contains a Promise rather than the resolved response.

Correct approach:

```javascript
async function getUser() {
  const response = await fetch("/api/users/1");

  console.log(response);
}
```

**Using await unnecessarily in independent operations**

```javascript
async function loadData() {
  const users = await getUsers();
  const posts = await getPosts();

  return { users, posts };
}
```

If both operations are independent, use `Promise.all()`:

```javascript
async function loadData() {
  const [users, posts] = await Promise.all([
    getUsers(),
    getPosts()
  ]);

  return { users, posts };
}
```

**Forgetting to return a value**

```javascript
async function getUser() {
  fetch("/api/users/1");
}
```

This function returns a Promise that fulfills with `undefined`, because the fetch result is not returned.

Correct approach:

```javascript
async function getUser() {
  return fetch("/api/users/1");
}
```

**Using forEach() when awaiting completion**

```javascript
async function processItems(items) {
  items.forEach(async item => {
    await processItem(item);
  });

  console.log("Finished");
}
```

The message may appear before the asynchronous operations finish.

Use `for...of` for sequential execution or `Promise.all()` for concurrent execution.

**Ignoring rejected Promises**

```javascript
async function loadData() {
  const response = await fetch("/api/users");

  return response.json();
}

loadData();
```

If the operation rejects and no handler is attached, the rejection may become unhandled.

Handle it with `try...catch` or attach a rejection handler at the call site.

# 🗁 Async/Await vs Promise Chains

| Feature | Promise chains | Async/Await |
| --- | --- | --- |
| Syntax | `.then()` and `.catch()` | `await`, `try...catch` |
| Readability | Can become deeply chained | Often reads sequentially |
| Error handling | `.catch()` | `try...catch` |
| Return value | Promise | Always Promise from async functions |
| Parallel execution | Promise combinators | Promise combinators with `await` |
| Use case | Promise composition | Sequential asynchronous workflows |

Both approaches use the same underlying Promise mechanism.

# 🗁 Best Practices

* Use `async` for functions that need to return Promise-based results.
* Use `await` when the next operation depends on the previous result.
* Use `Promise.all()` for independent operations that should run concurrently.
* Use `try...catch` for handling asynchronous failures.
* Check `response.ok` when using `fetch()`.
* Always handle rejected Promises at an appropriate boundary.
* Avoid unnecessary sequential waits.
* Do not use `forEach()` when you need to wait for asynchronous callbacks.
* Use `AbortController` when an asynchronous operation needs cancellation.
* Keep asynchronous functions focused on a clear responsibility.
* Avoid making every function async when it does not provide a practical benefit.

# 🗁 Key Takeaways

* `async` declares a function that always returns a Promise.
* `await` waits for a Promise to settle and produces its fulfillment value or throws its rejection reason.
* Async/await improves the readability of Promise-based code.
* Sequential `await` is appropriate for dependent operations.
* `Promise.all()` allows independent operations to execute concurrently.
* `try...catch` handles rejected Promises.
* `finally` is useful for cleanup.
* Top-level await is available in ECMAScript modules.
* Async/await does not eliminate Promises; it provides a cleaner way to work with them.
