# Synchronous vs Asynchronous JavaScript

JavaScript supports both synchronous and asynchronous programming.

These execution models determine how operations are performed and how the program handles tasks that take time to complete.

* **Synchronous programming:** Operations execute sequentially. Each operation must finish before the next operation begins.
* **Asynchronous programming:** Operations can be initiated without blocking the execution of subsequent code. Their results are handled later when they become available.

Understanding these models is essential for working with API requests, timers, file operations, database queries, user interactions, and other time-consuming tasks.

# 🗁 Synchronous JavaScript

Synchronous JavaScript executes statements sequentially, following the order in which they appear.

Each statement must complete before the next statement executes.

Example:

```javascript
console.log("First");

console.log("Second");

console.log("Third");
```

Output:

```text
First
Second
Third
```

Execution flow:

```text
Start
  |
  v
Execute First
  |
  v
Execute Second
  |
  v
Execute Third
  |
  v
End
```

The JavaScript engine executes each statement in sequence.

# 🗁 Blocking Execution

A synchronous operation can block the execution of subsequent JavaScript code when it takes a long time to complete.

Example:

```javascript
console.log("Start");

function blockExecution() {
  const start = Date.now();

  while (Date.now() - start < 3000) {
    // Blocking for approximately 3 seconds
  }
}

blockExecution();

console.log("End");
```

Output:

```text
Start
End
```

The `End` message appears only after the blocking operation finishes.

During this time, the JavaScript thread cannot process other JavaScript tasks in the same execution context.

In a browser, blocking the main thread can make the page unresponsive.

# 🗁 Asynchronous JavaScript

Asynchronous JavaScript allows certain operations to start without requiring the program to wait for their completion.

The program can continue executing other code while the asynchronous operation is pending.

Example:

```javascript
console.log("Start");

setTimeout(() => {
  console.log("Timer completed");
}, 2000);

console.log("End");
```

Output:

```text
Start
End
Timer completed
```

The timer callback executes later, after the timer delay has elapsed and the callback becomes eligible to run.

The important distinction is that `setTimeout()` schedules a callback instead of blocking the current execution.

# 🗁 Non-Blocking Execution

Non-blocking execution allows JavaScript to continue processing other work while an asynchronous operation is waiting.

Example:

```javascript
console.log("Start");

setTimeout(() => {
  console.log("Background operation completed");
}, 3000);

console.log("Continue executing");
```

Output:

```text
Start
Continue executing
Background operation completed
```

The timer does not stop the synchronous execution of the remaining statements.

In browsers, asynchronous operations may involve browser APIs. In Node.js, asynchronous operations may involve runtime APIs and the operating system.

# 🗁 Synchronous vs Asynchronous

| Feature | Synchronous | Asynchronous |
| --- | --- | --- |
| Execution | Sequential | Can continue while work is pending |
| Blocking | May block execution | Designed to avoid waiting synchronously |
| Result handling | Immediately available after operation | Often handled later |
| Execution order | Follows statement order | Depends on scheduling and completion |
| Complexity | Usually simpler | Requires asynchronous coordination |
| Common examples | Calculations, assignments | Fetch, timers, event callbacks |
| Error handling | `try...catch` | Depends on callback, Promise, or async function |

# 🗁 JavaScript Is Single-Threaded

JavaScript traditionally executes application code using a single main thread within a given execution context.

This means one JavaScript task runs at a time on that thread.

However, JavaScript environments provide mechanisms that allow asynchronous operations to progress without blocking the main thread.

These mechanisms include:

* JavaScript engine.
* Call stack.
* Browser Web APIs or Node.js runtime APIs.
* Task queues.
* Microtask queue.
* Event loop.

The JavaScript engine handles JavaScript execution, while the surrounding runtime provides additional capabilities.

# 🗁 The Call Stack

The call stack tracks function execution.

When a function is called, a new execution frame is pushed onto the stack. When the function returns, its frame is removed.

Example:

```javascript
function first() {
  second();
}

function second() {
  third();
}

function third() {
  console.log("Hello");
}

first();
```

Conceptual execution:

```text
Call first()
  |
  v
Call second()
  |
  v
Call third()
  |
  v
Execute console.log()
  |
  v
Return from third()
  |
  v
Return from second()
  |
  v
Return from first()
```

JavaScript executes synchronous function calls using the call stack.

# 🗁 Runtime Environment

The JavaScript runtime provides facilities beyond the JavaScript language itself.

In browsers, these include:

* DOM APIs.
* Timer APIs.
* Fetch API.
* Event handling.
* Web Storage.
* Web Workers.

In Node.js, these include:

* File system APIs.
* Networking APIs.
* Timers.
* Event emitters.
* Streams.
* Runtime-managed asynchronous I/O.

For example:

```javascript
setTimeout(() => {
  console.log("Executed later");
}, 1000);
```

The timer is managed by the runtime environment rather than being implemented as a blocking JavaScript operation.

# 🗁 Event Loop

The event loop coordinates when queued asynchronous callbacks can execute.

It monitors the execution state and coordinates queued work with the call stack.

A simplified model:

```text
+----------------------+
|    JavaScript Code   |
+----------------------+
           |
           v
+----------------------+
|      Call Stack      |
+----------------------+
           |
           v
+----------------------+
|   Runtime APIs       |
|                      |
| Timers / Fetch / I/O |
+----------------------+
           |
           v
+----------------------+
|     Task Queues      |
|                      |
| Tasks / Microtasks   |
+----------------------+
           |
           v
+----------------------+
|      Event Loop      |
+----------------------+
           |
           v
      Call Stack
```

This is a conceptual representation. Actual browser and Node.js scheduling details differ.

The event loop is covered in detail in the dedicated event loop documentation.

# 🗁 setTimeout()

`setTimeout()` schedules a callback to execute after a specified minimum delay.

Syntax:

```javascript
setTimeout(callback, delay);
```

Example:

```javascript
console.log("Start");

setTimeout(() => {
  console.log("Executed after the delay");
}, 2000);

console.log("End");
```

Output:

```text
Start
End
Executed after the delay
```

The delay is not an exact execution guarantee. The callback runs only when its timer has expired and the runtime can schedule it for execution.

# 🗁 setInterval()

`setInterval()` schedules a callback to execute repeatedly at approximately the specified interval.

Example:

```javascript
let count = 0;

const intervalId = setInterval(() => {
  count++;

  console.log(`Count: ${count}`);

  if (count === 3) {
    clearInterval(intervalId);
  }
}, 1000);
```

Output:

```text
Count: 1
Count: 2
Count: 3
```

The callback is scheduled repeatedly until the interval is cleared.

# 🗁 Fetch API

The Fetch API performs asynchronous HTTP requests.

Example:

```javascript
console.log("Start");

fetch("https://jsonplaceholder.typicode.com/users")
  .then(response => response.json())
  .then(users => {
    console.log("Users:", users);
  })
  .catch(error => {
    console.error("Request failed:", error);
  });

console.log("End");
```

The HTTP request does not block the synchronous execution of the remaining statements.

The request result is handled later through Promise callbacks.

# 🗁 Promises

A Promise represents the eventual completion or failure of an asynchronous operation.

A Promise has three states:

* `pending`: The operation has not settled.
* `fulfilled`: The operation completed successfully.
* `rejected`: The operation failed.

Example:

```javascript
const promise = new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve("Operation completed");
  }, 1000);
});

promise.then(result => {
  console.log(result);
});
```

Promises provide a structured way to coordinate asynchronous operations.

# 🗁 Async and Await

`async` and `await` provide syntax for writing asynchronous code in a sequential style.

Example:

```javascript
async function getUsers() {
  const response = await fetch(
    "https://jsonplaceholder.typicode.com/users"
  );

  if (!response.ok) {
    throw new Error(`HTTP error: ${response.status}`);
  }

  const users = await response.json();

  return users;
}

getUsers()
  .then(users => {
    console.log(users);
  })
  .catch(error => {
    console.error(error);
  });
```

The `await` keyword pauses the execution of the surrounding async function until the awaited Promise settles.

It does not block the JavaScript thread.

# 🗁 Synchronous and Asynchronous Examples

**Synchronous example**

```javascript
function calculate() {
  return 10 + 20;
}

const result = calculate();

console.log(result);
console.log("Finished");
```

Output:

```text
30
Finished
```

**Asynchronous example**

```javascript
function calculateAsync() {
  return new Promise(resolve => {
    setTimeout(() => {
      resolve(30);
    }, 1000);
  });
}

console.log("Start");

calculateAsync().then(result => {
  console.log(result);
});

console.log("Finished");
```

Output:

```text
Start
Finished
30
```

The second example returns a Promise, and its result becomes available later.

# 🗁 When to Use Synchronous Operations

Synchronous operations are appropriate when work is short, immediate, and does not require waiting for external resources.

Common examples:

* Variable assignments.
* Mathematical calculations.
* Simple string operations.
* Object property access.
* Array transformations.
* Small data validation operations.
* Pure function execution.

Example:

```javascript
const price = 1000;
const discount = 20;

const finalPrice = price - (price * discount / 100);

console.log(finalPrice);
```

There is no reason to introduce asynchronous complexity for this calculation.

# 🗁 When to Use Asynchronous Operations

Asynchronous operations are useful when work may take time or depends on external resources.

Common examples:

* HTTP requests.
* Database queries.
* File system operations.
* Timers.
* User event handling.
* Network communication.
* Streaming data.
* Background processing.

Example:

```javascript
async function loadProducts() {
  try {
    const response = await fetch("/api/products");

    if (!response.ok) {
      throw new Error(`HTTP error: ${response.status}`);
    }

    const products = await response.json();

    console.log(products);
  } catch (error) {
    console.error("Unable to load products:", error.message);
  }
}

loadProducts();
```

# 🗁 Important Distinction: Asynchronous Does Not Mean Parallel

Asynchronous execution and parallel execution are different concepts.

* **Asynchronous:** An operation can progress without requiring the current JavaScript execution to wait synchronously.
* **Parallel:** Multiple operations execute simultaneously on different processing resources.

For example:

```javascript
console.log("Start");

setTimeout(() => {
  console.log("Timer callback");
}, 1000);

console.log("End");
```

The timer callback is asynchronous, but this does not mean JavaScript executes the callback in parallel with the synchronous code on the same thread.

JavaScript can also perform parallel work through mechanisms such as Web Workers in browsers and worker threads in Node.js.

# 🗁 Common Mistakes

**Assuming setTimeout executes immediately**

```javascript
setTimeout(() => {
  console.log("Timer");
}, 0);

console.log("Hello");
```

Output:

```text
Hello
Timer
```

A zero-millisecond delay does not make the callback execute synchronously.

**Assuming await blocks the entire application**

```javascript
async function loadData() {
  console.log("Before");

  await new Promise(resolve => {
    setTimeout(resolve, 1000);
  });

  console.log("After");
}

loadData();

console.log("Outside");
```

Output:

```text
Before
Outside
After
```

Only the execution of `loadData()` is suspended at `await`. Other JavaScript code can continue executing.

**Assuming asynchronous operations always finish in their starting order**

```javascript
setTimeout(() => {
  console.log("First operation");
}, 2000);

setTimeout(() => {
  console.log("Second operation");
}, 500);
```

Output:

```text
Second operation
First operation
```

Completion and scheduling determine the callback order, not simply the order in which operations were started.

# 🗁 Best Practices

* Keep synchronous operations short on the browser main thread.
* Use asynchronous APIs for operations that involve waiting.
* Prefer Promises and `async`/`await` for modern asynchronous workflows.
* Handle rejected Promises.
* Check HTTP response status when using Fetch.
* Avoid unnecessary synchronous blocking operations.
* Use Web Workers for suitable CPU-intensive tasks.
* Do not confuse asynchronous execution with parallel execution.
* Understand the event loop before debugging complex execution-order problems.

# 🗁 Key Takeaways

* Synchronous JavaScript executes operations sequentially.
* A synchronous operation must complete before the next statement executes.
* Long-running synchronous operations can block the JavaScript thread.
* Asynchronous JavaScript allows execution to continue while certain operations are pending.
* The runtime environment provides APIs for timers, networking, and other asynchronous operations.
* The call stack manages synchronous function execution.
* The event loop coordinates queued callbacks with the call stack.
* Promises represent eventual asynchronous results.
* `async` and `await` simplify Promise-based programming.
* Asynchronous execution does not automatically mean parallel execution.
