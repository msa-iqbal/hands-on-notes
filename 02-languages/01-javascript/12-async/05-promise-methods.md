# Promise Methods

JavaScript provides several static methods on the `Promise` constructor to work with multiple promises or create promises with specific outcomes.

These methods help coordinate asynchronous operations, handle groups of requests, and control how results are collected.

The most commonly used Promise methods are:

* `Promise.all()`
* `Promise.allSettled()`
* `Promise.race()`
* `Promise.any()`
* `Promise.resolve()`
* `Promise.reject()`
* `Promise.withResolvers()`

# 🗁 Promise.all()

`Promise.all()` accepts an iterable of promises and returns a new promise.

* Fulfills when every input promise fulfills.
* Rejects as soon as one input promise rejects.
* Returns fulfilled values in the same order as the input iterable, regardless of completion order.
* An empty iterable fulfills immediately with an empty array.

**Syntax**

```javascript
Promise.all(iterable);
```

**Example**

```javascript
const userPromise = fetch("/api/users").then(response => response.json());
const postsPromise = fetch("/api/posts").then(response => response.json());
const commentsPromise = fetch("/api/comments").then(response => response.json());

Promise.all([
  userPromise,
  postsPromise,
  commentsPromise
])
  .then(([users, posts, comments]) => {
    console.log(users);
    console.log(posts);
    console.log(comments);
  })
  .catch(error => {
    console.error("One or more requests failed:", error);
  });
```

All three requests run concurrently. The result is available only after all three fulfill.

**Fail-fast behavior**

```javascript
const first = Promise.resolve("First");
const second = Promise.reject(new Error("Something failed"));
const third = Promise.resolve("Third");

Promise.all([first, second, third])
  .then(results => console.log(results))
  .catch(error => console.error(error.message));

// Something failed
```

`Promise.all()` does not automatically cancel the remaining operations when one rejects. They may continue running in the background.

**When to use**

* Loading multiple independent resources.
* Running parallel API requests.
* Waiting for several asynchronous tasks before continuing.
* Combining results that are all required.

# 🗁 Promise.allSettled()

`Promise.allSettled()` waits until every input promise has settled, regardless of whether it fulfilled or rejected.

Unlike `Promise.all()`, it never rejects because an input promise rejects.

Each result contains:

* `status`: Either `"fulfilled"` or `"rejected"`.
* `value`: The fulfilled result, when successful.
* `reason`: The rejection reason, when failed.

**Syntax**

```javascript
Promise.allSettled(iterable);
```

**Example**

```javascript
const tasks = [
  Promise.resolve("User loaded"),
  Promise.reject(new Error("Posts failed")),
  Promise.resolve("Comments loaded")
];

Promise.allSettled(tasks).then(results => {
  results.forEach(result => {
    if (result.status === "fulfilled") {
      console.log("Success:", result.value);
    } else {
      console.error("Failure:", result.reason.message);
    }
  });
});
```

**Result structure**

```javascript
[
  {
    status: "fulfilled",
    value: "User loaded"
  },
  {
    status: "rejected",
    reason: Error("Posts failed")
  },
  {
    status: "fulfilled",
    value: "Comments loaded"
  }
]
```

**When to use**

* Processing multiple independent API requests.
* Running batch operations where partial success is acceptable.
* Collecting every task's outcome.
* Generating reports about successful and failed operations.

# 🗁 Promise.race()

`Promise.race()` returns a promise that settles as soon as the first input promise settles.

The first promise can either fulfill or reject.

**Syntax**

```javascript
Promise.race(iterable);
```

**Example**

```javascript
const fastTask = new Promise(resolve => {
  setTimeout(() => resolve("Fast task completed"), 1000);
});

const slowTask = new Promise(resolve => {
  setTimeout(() => resolve("Slow task completed"), 3000);
});

Promise.race([fastTask, slowTask])
  .then(result => console.log(result));

// Fast task completed
```

The first settled promise determines the outcome.

**Example: Timeout pattern**

```javascript
const request = fetch("/api/users");

const timeout = new Promise((_, reject) => {
  setTimeout(() => {
    reject(new Error("Request timed out"));
  }, 5000);
});

Promise.race([request, timeout])
  .then(response => response.json())
  .then(data => console.log(data))
  .catch(error => console.error(error.message));
```

This pattern stops waiting for the request after the timeout promise rejects, but it does not cancel the underlying network request.

For actual request cancellation, use `AbortController`.

**When to use**

* Implementing timeout behavior.
* Choosing the first completed asynchronous operation.
* Racing multiple data sources.
* Selecting the fastest available response.

# 🗁 Promise.any()

`Promise.any()` fulfills as soon as the first input promise fulfills.

Unlike `Promise.race()`, it ignores rejected promises while waiting for a successful result.

It rejects with an `AggregateError` only when every input promise rejects.

**Syntax**

```javascript
Promise.any(iterable);
```

**Example**

```javascript
const primaryServer = Promise.reject(
  new Error("Primary server unavailable")
);

const secondaryServer = Promise.resolve("Secondary server response");

const backupServer = Promise.resolve("Backup server response");

Promise.any([
  primaryServer,
  secondaryServer,
  backupServer
])
  .then(result => console.log(result))
  .catch(error => console.error(error));

// Secondary server response
```

**All promises rejected**

```javascript
const tasks = [
  Promise.reject(new Error("Server A failed")),
  Promise.reject(new Error("Server B failed")),
  Promise.reject(new Error("Server C failed"))
];

Promise.any(tasks)
  .then(result => console.log(result))
  .catch(error => {
    console.log(error instanceof AggregateError); // true
    console.log(error.errors);
  });
```

`AggregateError.errors` contains the rejection reasons from the input promises, in input order.

**When to use**

* Trying multiple API endpoints or servers.
* Implementing fallback services.
* Retrieving the first successful response.
* Supporting redundant asynchronous operations.

# 🗁 Promise.resolve()

`Promise.resolve()` creates a fulfilled promise with the provided value.

If the value is already a promise or thenable, the returned promise adopts its eventual state rather than simply wrapping it as an ordinary value.

**Syntax**

```javascript
Promise.resolve(value);
```

**Example**

```javascript
const promise = Promise.resolve("Operation successful");

promise.then(result => {
  console.log(result);
});

// Operation successful
```

**Resolving an existing promise**

```javascript
const originalPromise = Promise.resolve(100);

const newPromise = Promise.resolve(originalPromise);

newPromise.then(value => console.log(value));

// 100
```

**Resolving a thenable**

```javascript
const thenable = {
  then(resolve) {
    resolve("Thenable resolved");
  }
};

Promise.resolve(thenable).then(value => {
  console.log(value);
});
```

**When to use**

* Converting ordinary values into promises.
* Returning a consistent promise-based interface.
* Normalizing synchronous and asynchronous return values.
* Simplifying asynchronous function composition.

# 🗁 Promise.reject()

`Promise.reject()` creates a rejected promise with the provided reason.

**Syntax**

```javascript
Promise.reject(reason);
```

**Example**

```javascript
const promise = Promise.reject(
  new Error("Something went wrong")
);

promise.catch(error => {
  console.error(error.message);
});

// Something went wrong
```

**Using Promise.reject() in functions**

```javascript
function getUser(userId) {
  if (!userId) {
    return Promise.reject(
      new Error("User ID is required")
    );
  }

  return fetch(`/api/users/${userId}`);
}

getUser(null).catch(error => {
  console.error(error.message);
});
```

**When to use**

* Returning rejected promises from functions.
* Representing asynchronous validation failures.
* Creating consistent promise-based error handling.
* Rejecting operations before starting asynchronous work.

# 🗁 Promise.withResolvers()

`Promise.withResolvers()` creates a promise and exposes its `resolve` and `reject` functions alongside it.

It returns an object containing:

* `promise`: The newly created promise.
* `resolve`: The function used to fulfill or resolve the promise.
* `reject`: The function used to reject the promise.

**Syntax**

```javascript
const { promise, resolve, reject } = Promise.withResolvers();
```

**Example**

```javascript
const {
  promise,
  resolve,
  reject
} = Promise.withResolvers();

promise
  .then(value => console.log(value))
  .catch(error => console.error(error.message));

resolve("Promise completed");

// Promise completed
```

**Rejecting the promise**

```javascript
const {
  promise,
  resolve,
  reject
} = Promise.withResolvers();

promise.catch(error => {
  console.error(error.message);
});

reject(new Error("Operation failed"));

// Operation failed
```

**Practical example: Event-based completion**

```javascript
function waitForEvent(target, eventName) {
  const { promise, resolve, reject } = Promise.withResolvers();

  function handleEvent(event) {
    cleanup();
    resolve(event);
  }

  function handleError() {
    cleanup();
    reject(new Error("Event failed"));
  }

  function cleanup() {
    target.removeEventListener(eventName, handleEvent);
    target.removeEventListener("error", handleError);
  }

  target.addEventListener(eventName, handleEvent, { once: true });
  target.addEventListener("error", handleError, { once: true });

  return promise;
}

const button = document.querySelector("#submit");

waitForEvent(button, "click").then(event => {
  console.log("Button clicked:", event);
});
```

**When to use**

* Managing event-driven asynchronous operations.
* Separating promise creation from its settlement.
* Building custom asynchronous abstractions.
* Integrating callback-based APIs with promises.

`Promise.withResolvers()` is available in modern JavaScript runtimes. Check the target runtime when supporting older environments.

# 🗁 Promise Methods Comparison

| Method | Fulfills when | Rejects when |
| --- | --- | --- |
| `Promise.all()` | All promises fulfill | Any promise rejects |
| `Promise.allSettled()` | All promises settle | Never due to input rejection |
| `Promise.race()` | First promise fulfills | First promise rejects |
| `Promise.any()` | First promise fulfills | All promises reject |
| `Promise.resolve()` | Immediately adopts the supplied value's state | The supplied promise or thenable rejects |
| `Promise.reject()` | Never | Immediately |
| `Promise.withResolvers()` | Its exposed `resolve()` settles it successfully | Its exposed `reject()` settles it unsuccessfully |

# 🗁 Promise Combinators: Quick Comparison

The four main promise combinators behave differently when multiple asynchronous operations are involved.

| Scenario | `all()` | `allSettled()` | `race()` | `any()` |
| --- | --- | --- | --- | --- |
| All fulfill | Fulfills | Fulfills | First result wins | First fulfillment wins |
| One rejects | Rejects | Waits for all | First settlement wins | Waits for another fulfillment |
| All reject | Rejects | Fulfills with outcomes | First rejection wins | Rejects with `AggregateError` |
| Empty iterable | Fulfills with `[]` | Fulfills with `[]` | Remains pending | Rejects with `AggregateError` |

# 🗁 Practical Example: Loading Multiple Resources

Consider a dashboard that needs user information, notifications, and analytics.

```javascript
async function loadDashboard() {
  try {
    const [
      userResponse,
      notificationResponse,
      analyticsResponse
    ] = await Promise.all([
      fetch("/api/user"),
      fetch("/api/notifications"),
      fetch("/api/analytics")
    ]);

    if (
      !userResponse.ok ||
      !notificationResponse.ok ||
      !analyticsResponse.ok
    ) {
      throw new Error("Failed to load dashboard resources");
    }

    const [user, notifications, analytics] = await Promise.all([
      userResponse.json(),
      notificationResponse.json(),
      analyticsResponse.json()
    ]);

    return {
      user,
      notifications,
      analytics
    };
  } catch (error) {
    console.error("Dashboard loading failed:", error);
    throw error;
  }
}

loadDashboard()
  .then(data => console.log(data))
  .catch(error => console.error(error.message));
```

The first `Promise.all()` coordinates the network requests. The second coordinates JSON parsing.

This approach allows independent operations to run concurrently instead of waiting for each operation sequentially.

# 🗁 Choosing the Right Promise Method

Use the following decision guide:

* Use `Promise.all()` when every operation must succeed.
* Use `Promise.allSettled()` when every operation's outcome matters.
* Use `Promise.race()` when the first settlement determines the result.
* Use `Promise.any()` when the first successful result is sufficient.
* Use `Promise.resolve()` to normalize a value into a promise.
* Use `Promise.reject()` to return an immediately rejected promise.
* Use `Promise.withResolvers()` when the promise's settlement functions need to be exposed separately.

# 🗁 Best Practices

* Prefer `Promise.all()` for independent operations that are all required.
* Use `Promise.allSettled()` when partial success is acceptable.
* Do not confuse `Promise.race()` with `Promise.any()`.
* Remember that promise combinators do not automatically cancel unfinished operations.
* Use `AbortController` when an asynchronous operation needs actual cancellation.
* Handle rejected promises to avoid unhandled rejection errors.
* Avoid wrapping an existing promise unnecessarily with `new Promise()`.
* Keep related asynchronous operations organized and readable.
* Consider limiting concurrency when processing large collections of requests.

# 🗁 Key Takeaways

* Promise combinators coordinate multiple asynchronous operations.
* `Promise.all()` requires every promise to fulfill.
* `Promise.allSettled()` collects every outcome.
* `Promise.race()` returns the outcome of the first settled promise.
* `Promise.any()` returns the first fulfilled result.
* `Promise.resolve()` creates or adopts a fulfilled promise.
* `Promise.reject()` creates a rejected promise.
* `Promise.withResolvers()` exposes a promise and its settlement functions.
* Choosing the correct method depends on the success and failure requirements of the application.
