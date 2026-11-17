# Promises

A Promise is a JavaScript object that represents the eventual completion or failure of an asynchronous operation.

Promises provide a structured way to handle asynchronous results without relying on deeply nested callbacks.

They are commonly used for:

* HTTP requests.
* Database operations.
* File system operations.
* Timers.
* Background tasks.
* Asynchronous data processing.

Promises are a fundamental part of modern JavaScript asynchronous programming.

# 🗁 What Is a Promise?

A Promise represents a value that may become available in the future.

A Promise can be in one of three states:

* `pending`: The operation is still in progress.
* `fulfilled`: The operation completed successfully.
* `rejected`: The operation failed.

Once a Promise is fulfilled or rejected, it is considered settled.

Promise state transition:

```text
             Pending
             /    \
            /      \
           v        v
      Fulfilled  Rejected
```

A Promise can settle only once. Once fulfilled or rejected, its state cannot change again.

# 🗁 Creating a Promise

JavaScript provides the `Promise` constructor to create a Promise.

Syntax:

```javascript
const promise = new Promise((resolve, reject) => {
  // Asynchronous operation
});
```

The constructor accepts an executor function with two parameters:

* `resolve`: Fulfills the Promise with a value.
* `reject`: Rejects the Promise with a reason.

Example:

```javascript
const promise = new Promise((resolve, reject) => {
  resolve("Operation successful");
});

console.log(promise);
```

The Promise is fulfilled with the value `"Operation successful"`.

# 🗁 Promise Resolution

The `resolve()` function fulfills a Promise with a value.

Example:

```javascript
const promise = new Promise((resolve) => {
  resolve("Data loaded successfully");
});

promise.then(result => {
  console.log(result);
});
```

Output:

```text
Data loaded successfully
```

The `.then()` method registers a fulfillment handler that receives the resolved value.

# 🗁 Promise Rejection

The `reject()` function rejects a Promise with a reason.

Example:

```javascript
const promise = new Promise((resolve, reject) => {
  reject(new Error("Something went wrong"));
});

promise.catch(error => {
  console.error(error.message);
});
```

Output:

```text
Something went wrong
```

It is generally preferable to reject Promises with `Error` objects because they provide useful error messages and stack traces.

# 🗁 Promise with setTimeout()

Promises are frequently used to represent operations that complete asynchronously.

Example:

```javascript
const promise = new Promise((resolve) => {
  setTimeout(() => {
    resolve("Task completed");
  }, 2000);
});

promise.then(result => {
  console.log(result);
});
```

Output after approximately two seconds:

```text
Task completed
```

The Promise starts in the pending state and becomes fulfilled when `resolve()` is called.

# 🗁 Promise Executor Function

The executor function is passed to the `Promise` constructor.

It executes immediately when the Promise is created.

Example:

```javascript
console.log("Start");

const promise = new Promise((resolve) => {
  console.log("Executor executed");

  resolve("Completed");
});

console.log("End");

promise.then(result => {
  console.log(result);
});
```

Output:

```text
Start
Executor executed
End
Completed
```

Important points:

* The executor runs synchronously.
* `resolve()` changes the Promise state.
* `.then()` callbacks execute asynchronously as Promise reactions.
* Promise reactions are processed through the microtask queue.

# 🗁 Handling Promise Results

Promises provide methods for handling their outcomes.

The most commonly used methods are:

* `.then()`
* `.catch()`
* `.finally()`

# 🗁 then()

The `.then()` method registers handlers for Promise fulfillment and optionally rejection.

Syntax:

```javascript
promise.then(onFulfilled, onRejected);
```

Example:

```javascript
const promise = Promise.resolve("Hello JavaScript");

promise.then(result => {
  console.log(result);
});
```

Output:

```text
Hello JavaScript
```

The first callback handles fulfillment.

The optional second callback handles rejection:

```javascript
const promise = Promise.reject(new Error("Request failed"));

promise.then(
  result => {
    console.log(result);
  },
  error => {
    console.error(error.message);
  }
);
```

Although the second argument is supported, `.catch()` is often preferred for clearer rejection handling.

# 🗁 catch()

The `.catch()` method handles Promise rejections.

Syntax:

```javascript
promise.catch(onRejected);
```

Example:

```javascript
const promise = new Promise((resolve, reject) => {
  reject(new Error("Database connection failed"));
});

promise
  .then(result => {
    console.log(result);
  })
  .catch(error => {
    console.error(error.message);
  });
```

Output:

```text
Database connection failed
```

`.catch()` is effectively a convenient form of:

```javascript
promise.then(undefined, onRejected);
```

# 🗁 finally()

The `.finally()` method registers a callback that executes when a Promise settles, regardless of whether it fulfills or rejects.

Example:

```javascript
const promise = Promise.resolve("Data loaded");

promise
  .then(result => {
    console.log(result);
  })
  .finally(() => {
    console.log("Operation finished");
  });
```

Output:

```text
Data loaded
Operation finished
```

A common use case is cleanup logic:

```javascript
function loadData() {
  showLoader();

  return fetch("/api/users")
    .then(response => {
      if (!response.ok) {
        throw new Error(`HTTP error: ${response.status}`);
      }

      return response.json();
    })
    .finally(() => {
      hideLoader();
    });
}
```

The `finally()` callback normally does not receive the fulfillment value or rejection reason.

# 🗁 Promise Chaining

Promise chaining allows multiple asynchronous operations to execute in sequence.

Each `.then()` returns a new Promise.

Example:

```javascript
Promise.resolve(10)
  .then(value => {
    console.log(value);

    return value * 2;
  })
  .then(value => {
    console.log(value);

    return value + 5;
  })
  .then(value => {
    console.log(value);
  });
```

Output:

```text
10
20
25
```

The value returned from one `.then()` handler becomes the fulfillment value of the next Promise in the chain.

# 🗁 Returning Values from then()

When a `.then()` handler returns a normal value, the next Promise in the chain fulfills with that value.

Example:

```javascript
Promise.resolve(5)
  .then(value => {
    return value * 2;
  })
  .then(result => {
    console.log(result);
  });
```

Output:

```text
10
```

If a handler does not explicitly return a value, its Promise fulfills with `undefined`.

Example:

```javascript
Promise.resolve(5)
  .then(value => {
    console.log(value);
  })
  .then(result => {
    console.log(result);
  });
```

Output:

```text
5
undefined
```

# 🗁 Returning Another Promise

A `.then()` handler can return another Promise.

The next step waits for that Promise to settle.

Example:

```javascript
function getUser() {
  return Promise.resolve({
    id: 1,
    name: "John"
  });
}

function getOrders(userId) {
  return Promise.resolve([
    { id: 101, userId },
    { id: 102, userId }
  ]);
}

getUser()
  .then(user => {
    return getOrders(user.id);
  })
  .then(orders => {
    console.log(orders);
  });
```

The Promise chain adopts the state and eventual value of the returned Promise.

# 🗁 Promise Error Propagation

An error thrown inside a `.then()` handler rejects the Promise returned by that handler.

The rejection can be handled by a later `.catch()`.

Example:

```javascript
Promise.resolve()
  .then(() => {
    throw new Error("Processing failed");
  })
  .then(() => {
    console.log("This will not execute");
  })
  .catch(error => {
    console.error(error.message);
  });
```

Output:

```text
Processing failed
```

A rejection also propagates through subsequent Promise handlers until it is handled.

# 🗁 Promise States in Practice

Example:

```javascript
const promise = new Promise((resolve, reject) => {
  const success = true;

  if (success) {
    resolve("Success");
  } else {
    reject(new Error("Failure"));
  }
});

promise
  .then(result => {
    console.log(result);
  })
  .catch(error => {
    console.error(error.message);
  });
```

When `success` is `true`, the Promise fulfills.

When `success` is `false`, the Promise rejects.

Calling `resolve()` or `reject()` settles the Promise only once.

# 🗁 Promise.resolve()

`Promise.resolve()` creates a fulfilled Promise from a value or adopts the state of a Promise-like object.

Example:

```javascript
const promise = Promise.resolve("Hello");

promise.then(value => {
  console.log(value);
});
```

Output:

```text
Hello
```

It can also accept an existing Promise:

```javascript
const original = Promise.resolve(100);

const result = Promise.resolve(original);

result.then(value => {
  console.log(value);
});
```

The returned Promise adopts the original Promise's eventual state.

# 🗁 Promise.reject()

`Promise.reject()` creates an immediately rejected Promise.

Example:

```javascript
const promise = Promise.reject(new Error("Operation failed"));

promise.catch(error => {
  console.error(error.message);
});
```

Output:

```text
Operation failed
```

The rejection handler executes asynchronously as a Promise reaction.

# 🗁 Promise Constructor vs Promise.resolve()

Both can produce fulfilled Promises, but their behavior differs.

Using the constructor:

```javascript
const promise = new Promise(resolve => {
  resolve("Success");
});
```

Using `Promise.resolve()`:

```javascript
const promise = Promise.resolve("Success");
```

Use the Promise constructor when defining custom asynchronous behavior or adapting callback-based operations.

Use `Promise.resolve()` when an existing value needs to be represented as a Promise.

# 🗁 Promise and Callback Comparison

Callbacks and Promises both support asynchronous programming, but their APIs are different.

**Callback approach**

```javascript
function getData(callback) {
  setTimeout(() => {
    callback(null, { name: "John" });
  }, 1000);
}

getData((error, data) => {
  if (error) {
    console.error(error);

    return;
  }

  console.log(data);
});
```

**Promise approach**

```javascript
function getData() {
  return new Promise(resolve => {
    setTimeout(() => {
      resolve({ name: "John" });
    }, 1000);
  });
}

getData()
  .then(data => {
    console.log(data);
  })
  .catch(error => {
    console.error(error);
  });
```

Promises provide chaining and a standardized way to propagate failures.

# 🗁 Promise and Async/Await Comparison

`async` and `await` are built on Promises.

Promise chaining:

```javascript
function getUser() {
  return Promise.resolve({
    id: 1,
    name: "John"
  });
}

getUser()
  .then(user => {
    console.log(user);
  })
  .catch(error => {
    console.error(error);
  });
```

Async/await:

```javascript
async function displayUser() {
  try {
    const user = await getUser();

    console.log(user);
  } catch (error) {
    console.error(error);
  }
}

displayUser();
```

Both approaches work with Promises. Choose the style that makes the workflow easiest to understand.

# 🗁 Promise Object Properties

A Promise has internal state and result information managed by the JavaScript engine.

These internal details are not directly accessible through ordinary public properties.

For example:

```javascript
const promise = Promise.resolve("Hello");

console.log(promise);
```

Developer tools may display information such as the Promise state and result.

However, there is no standard public property such as `promise.state` or `promise.value` for inspecting those internal details.

Use `.then()`, `.catch()`, and `.finally()` to work with Promise outcomes.

# 🗁 Common Promise Mistakes

**Forgetting to return a Promise**

Incorrect:

```javascript
function getUser() {
  fetch("/api/user");
}

getUser().then(user => {
  console.log(user);
});
```

The function returns `undefined`, not the Fetch Promise.

Correct:

```javascript
function getUser() {
  return fetch("/api/user");
}
```

**Forgetting to return from then()**

Incorrect:

```javascript
getUser()
  .then(user => {
    getOrders(user.id);
  })
  .then(orders => {
    console.log(orders);
  });
```

The first handler does not return the Promise from `getOrders()`.

Correct:

```javascript
getUser()
  .then(user => {
    return getOrders(user.id);
  })
  .then(orders => {
    console.log(orders);
  });
```

**Creating unnecessary nested Promises**

Unnecessary:

```javascript
function getUser() {
  return new Promise(resolve => {
    resolve(fetch("/api/user"));
  });
}
```

Better:

```javascript
function getUser() {
  return fetch("/api/user");
}
```

The Fetch API already returns a Promise.

**Ignoring Promise rejections**

Incorrect:

```javascript
fetch("/api/users")
  .then(response => response.json());
```

If the request or JSON parsing fails, the resulting rejection is not handled in this example.

Correct:

```javascript
fetch("/api/users")
  .then(response => {
    if (!response.ok) {
      throw new Error(`HTTP error: ${response.status}`);
    }

    return response.json();
  })
  .then(users => {
    console.log(users);
  })
  .catch(error => {
    console.error(error.message);
  });
```

**Assuming Promise executors are asynchronous**

Incorrect assumption:

```javascript
console.log("Start");

new Promise(resolve => {
  console.log("Executor");

  resolve();
});

console.log("End");
```

Output:

```text
Start
Executor
End
```

The executor runs synchronously.

# 🗁 Best Practices

* Return Promises from functions that perform asynchronous work.
* Return values from `.then()` handlers when the next step needs them.
* Use `Error` objects for rejection reasons.
* Handle rejections with `.catch()` or an appropriate error-handling strategy.
* Use `.finally()` for cleanup operations.
* Avoid unnecessary Promise construction.
* Avoid deeply nested Promise chains.
* Use `async` and `await` when they improve readability.
* Do not assume that Promise executors execute asynchronously.
* Remember that Fetch does not reject merely because the server returns an HTTP error status.

# 🗁 Key Takeaways

* A Promise represents the eventual result of an asynchronous operation.
* Promises have three states: pending, fulfilled, and rejected.
* The Promise executor runs synchronously.
* `resolve()` fulfills a Promise, while `reject()` rejects it.
* `.then()` handles fulfillment and optionally rejection.
* `.catch()` handles rejection.
* `.finally()` executes after settlement.
* Promise chaining connects asynchronous operations.
* Returning another Promise allows asynchronous steps to be coordinated.
* Errors propagate through Promise chains.
* `Promise.resolve()` and `Promise.reject()` create settled Promises.
* `async` and `await` provide cleaner syntax for working with Promises.
