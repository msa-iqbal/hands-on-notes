# Event Loop

The JavaScript Event Loop is the mechanism that coordinates synchronous code execution, asynchronous callbacks, and task scheduling.

JavaScript is commonly described as single-threaded because its main execution thread processes one piece of JavaScript code at a time.

However, JavaScript applications can perform asynchronous operations such as:

* Network requests.
* Timers.
* File operations in Node.js.
* User interactions.
* Event handling.
* Promise callbacks.

The Event Loop coordinates when their callbacks are executed.

Understanding the Event Loop is essential for understanding:

* Asynchronous JavaScript.
* Promises.
* `async` and `await`.
* `setTimeout()`.
* Microtasks and macrotasks.
* Browser rendering.
* Node.js asynchronous execution.

# 🗁 JavaScript Runtime Environment

JavaScript code executes inside a runtime environment.

A runtime environment provides the facilities needed to execute JavaScript and interact with the outside world.

Common JavaScript runtimes include:

* Web browsers.
* Node.js.
* Deno.
* Bun.

A simplified JavaScript runtime consists of:

* Call Stack.
* Heap.
* Web APIs or runtime APIs.
* Task Queue.
* Microtask Queue.
* Event Loop.

<escape>```text
              JavaScript Runtime
       ┌─────────────────────────────┐
       │                             │
       │        JavaScript Engine    │
       │                             │
       │    ┌───────────────────┐    │
       │    │     Call Stack    │    │
       │    └───────────────────┘    │
       │                             │
       │    ┌───────────────────┐    │
       │    │       Heap        │    │
       │    └───────────────────┘    │
       │                             │
       ├─────────────────────────────┤
       │                             │
       │       Runtime APIs          │
       │                             │
       │    Timers                   │
       │    Network Operations       │
       │    DOM Events               │
       │                             │
       ├─────────────────────────────┤
       │                             │
       │        Event Loop           │
       │              │              │
       │       ┌──────┴──────┐       │
       │       │             │       │
       │  Microtask       Task       │
       │   Queue          Queue      │
       │                             │
       └─────────────────────────────┘

```</escape>

This is a conceptual model. The exact runtime architecture differs between browsers and server-side JavaScript environments.

# 🗁 Call Stack

The Call Stack tracks currently executing JavaScript functions.

It follows the Last In, First Out (LIFO) principle.

The most recently added function executes first and is removed when it finishes.

**Example**

```javascript
function first() {
  console.log("First");

  second();
}

function second() {
  console.log("Second");

  third();
}

function third() {
  console.log("Third");
}

first();
```

**Output**

```text
First
Second
Third
```

**Execution flow**

```text
first() is called
    ↓
first() enters Call Stack
    ↓
second() is called
    ↓
second() enters Call Stack
    ↓
third() is called
    ↓
third() enters Call Stack
    ↓
third() finishes
    ↓
second() finishes
    ↓
first() finishes
```

Each function is removed from the stack after its execution completes.

# 🗁 Heap

The Heap is a memory region used for dynamically allocated objects and other runtime-managed data.

For example:

```javascript
const user = {
  id: 1,
  name: "John"
};

const numbers = [10, 20, 30];
```

Objects and arrays are stored in managed memory, while variables hold references to those objects.

The Heap and Call Stack serve different purposes:

| Call Stack | Heap |
| --- | --- |
| Tracks function execution | Stores dynamically allocated data |
| Uses stack frames | Uses managed memory allocation |
| Follows LIFO execution | Does not follow function-call order |
| Manages execution context | Supports object and data storage |

# 🗁 Runtime APIs

JavaScript engines do not independently perform every asynchronous operation.

The surrounding runtime environment provides APIs for operations such as timers, networking, and event handling.

**Browser APIs**

Examples include:

* `setTimeout()`
* `setInterval()`
* `fetch()`
* DOM event listeners.
* `XMLHttpRequest`.
* WebSocket APIs.

**Node.js APIs**

Examples include:

* File system operations.
* Network sockets.
* Timers.
* Streams.
* Operating system interactions.

Consider:

```javascript
console.log("Start");

setTimeout(() => {
  console.log("Timer completed");
}, 1000);

console.log("End");
```

The runtime manages the timer while JavaScript continues executing.

The timer callback becomes eligible for execution after its delay has elapsed and is scheduled through the runtime's task-processing mechanism.

# 🗁 Task Queue

The Task Queue, commonly called the Macrotask Queue in educational explanations, stores callbacks waiting to be executed.

Examples of task sources in browsers include:

* Timer callbacks.
* User interaction events.
* Message events.
* Certain networking-related events.

**Example**

```javascript
console.log("One");

setTimeout(() => {
  console.log("Two");
}, 0);

console.log("Three");
```

**Output**

```text
One
Three
Two
```

Even with a zero-millisecond delay, the timer callback does not execute immediately.

It must wait until the current JavaScript execution finishes and the runtime schedules its callback.

# 🗁 Microtask Queue

The Microtask Queue stores callbacks that must be processed at microtask checkpoints.

Common sources include:

* Promise `.then()` callbacks.
* Promise `.catch()` callbacks.
* Promise `.finally()` callbacks.
* `queueMicrotask()`.
* Mutation Observer callbacks in browsers.

Microtasks are generally processed before the next task is selected.

**Example**

```javascript
console.log("Start");

setTimeout(() => {
  console.log("Timeout");
}, 0);

Promise.resolve().then(() => {
  console.log("Promise");
});

console.log("End");
```

**Output**

```text
Start
End
Promise
Timeout
```

The Promise callback runs before the timer callback because microtasks are processed before the next task.

# 🗁 What Is the Event Loop?

The Event Loop coordinates the execution of tasks and microtasks.

In a simplified browser model, it repeatedly:

* Checks whether JavaScript can execute.
* Processes an eligible task.
* Performs a microtask checkpoint.
* Allows rendering opportunities when appropriate.
* Continues with subsequent tasks.

A simplified representation:

<escape>```text
              ┌──────────────────┐
              │   Call Stack     │
              │                 │
              │ Is it empty?    │
              └────────┬─────────┘
                       │
                       ▼
              ┌──────────────────┐
              │ Process Current  │
              │ JavaScript Task  │
              └────────┬─────────┘
                       │
                       ▼
              ┌──────────────────┐
              │ Process All      │
              │ Microtasks       │
              └────────┬─────────┘
                       │
                       ▼
              ┌──────────────────┐
              │ Rendering        │
              │ Opportunity      │
              └────────┬─────────┘
                       │
                       ▼
              ┌──────────────────┐
              │ Select Next Task │
              └────────┬─────────┘
                       │
                       └───────↺

```</escape>

This diagram is simplified. Browser rendering is not guaranteed after every task, and the Event Loop also follows runtime-specific scheduling rules.

# 🗁 Synchronous Execution

Synchronous code executes sequentially.

Each statement generally completes before the next statement begins.

```javascript
console.log("First");

console.log("Second");

console.log("Third");
```

**Output**

```text
First
Second
Third
```

The statements execute in their written order.

# 🗁 Asynchronous Execution

Asynchronous operations allow JavaScript to continue executing while an operation is pending.

```javascript
console.log("Start");

setTimeout(() => {
  console.log("Async operation");
}, 1000);

console.log("End");
```

**Output**

```text
Start
End
Async operation
```

The timer callback executes later through the runtime's scheduling mechanism.

# 🗁 Execution Priority

For common browser examples involving synchronous code, Promises, and timers, the simplified execution order is:

* Execute the current synchronous JavaScript.
* Process queued microtasks until the microtask queue is empty.
* Allow a rendering opportunity when appropriate.
* Select another task.
* Repeat the process.

**Example**

```javascript
console.log("A");

setTimeout(() => {
  console.log("B");
}, 0);

Promise.resolve().then(() => {
  console.log("C");
});

queueMicrotask(() => {
  console.log("D");
});

console.log("E");
```

**Output**

```text
A
E
C
D
B
```

Explanation:

* `A` executes synchronously.
* The timer callback is scheduled as a task.
* The Promise callback is scheduled as a microtask.
* `queueMicrotask()` schedules another microtask.
* `E` executes synchronously.
* The microtask queue is processed in order: `C`, then `D`.
* The timer callback executes afterward: `B`.

# 🗁 Nested Microtasks

Microtasks can schedule additional microtasks.

The runtime continues processing the microtask queue until it becomes empty at the microtask checkpoint.

```javascript
console.log("Start");

Promise.resolve().then(() => {
  console.log("First microtask");

  Promise.resolve().then(() => {
    console.log("Nested microtask");
  });
});

setTimeout(() => {
  console.log("Timer");
}, 0);

console.log("End");
```

**Output**

```text
Start
End
First microtask
Nested microtask
Timer
```

The nested microtask executes before the timer callback.

This behavior is important because continuously scheduling microtasks can delay other tasks and browser rendering.

# 🗁 Event Loop and async/await

Async/await also uses the Promise mechanism.

When an async function reaches an `await` expression, it suspends that function's execution and allows the surrounding JavaScript to continue.

After the awaited value settles, the function resumes through Promise-related microtask scheduling.

**Example**

```javascript
async function getData() {
  console.log("Inside function");

  await Promise.resolve();

  console.log("After await");
}

console.log("Start");

getData();

console.log("End");
```

**Output**

```text
Start
Inside function
End
After await
```

Explanation:

* `Start` executes synchronously.
* `getData()` starts executing.
* `Inside function` executes before the first `await`.
* The function suspends at `await`.
* `End` executes.
* The async function resumes in a later microtask.

# 🗁 Blocking the Event Loop

Long-running synchronous JavaScript prevents the main thread from processing other work.

This is called blocking the Event Loop.

**Example**

```javascript
console.log("Start");

const start = Date.now();

while (Date.now() - start < 5000) {
  // Blocking operation
}

console.log("Finished");
```

The loop blocks JavaScript execution for approximately five seconds.

During this time, the browser's main thread cannot process other JavaScript tasks.

This can delay:

* User interactions.
* Timer callbacks.
* Promise callbacks.
* Rendering.
* UI updates.

**Better approach**

Break large computations into smaller tasks or move suitable CPU-intensive work to a Web Worker.

```javascript
function processChunk(items, startIndex = 0) {
  const chunkSize = 1000;

  const endIndex = Math.min(
    startIndex + chunkSize,
    items.length
  );

  for (let index = startIndex; index < endIndex; index++) {
    // Process one item
  }

  if (endIndex < items.length) {
    setTimeout(() => {
      processChunk(items, endIndex);
    }, 0);
  }
}
```

This allows other tasks to receive execution opportunities between chunks.

# 🗁 Event Loop in Browsers vs Node.js

Browsers and Node.js both use event-loop-based asynchronous execution, but their runtime implementations differ.

| Feature | Browser | Node.js |
| --- | --- | --- |
| Runtime APIs | Web APIs | Node.js APIs |
| Timers | `setTimeout()`, `setInterval()` | `setTimeout()`, `setInterval()` |
| Promise callbacks | Microtask scheduling | Microtask scheduling |
| I/O operations | Browser networking and platform APIs | Node.js networking and file APIs |
| Event Loop implementation | Browser event loop model | Node.js event loop powered by libuv |
| Additional scheduling | Browser task sources and rendering | Event-loop phases and `process.nextTick()` |

Node.js has additional scheduling details, including:

* Timers phase.
* Pending callbacks phase.
* Poll phase.
* Check phase.
* Close callbacks phase.
* `process.nextTick()` queue.
* Promise microtask queue.

These details are covered more deeply in Node.js-specific documentation.

# 🗁 Practical Debugging Example

Consider the following code:

```javascript
console.log("A");

setTimeout(() => {
  console.log("B");

  Promise.resolve().then(() => {
    console.log("C");
  });
}, 0);

Promise.resolve().then(() => {
  console.log("D");
});

console.log("E");
```

**Output**

```text
A
E
D
B
C
```

Execution explanation:

* `A` executes synchronously.
* The timer callback is scheduled.
* The Promise callback is scheduled as a microtask.
* `E` executes synchronously.
* The microtask queue is processed, printing `D`.
* The timer callback executes, printing `B`.
* The timer callback schedules another microtask.
* The new microtask executes, printing `C`.

Understanding this sequence makes asynchronous execution much easier to predict.

# 🗁 Common Misconceptions

* JavaScript being single-threaded does not mean the entire runtime performs no work concurrently.
* `setTimeout(callback, 0)` does not execute immediately.
* Promises are not themselves the mechanism that performs network requests.
* `await` does not block the entire JavaScript thread.
* Microtasks generally run before the next task is selected.
* The Event Loop does not guarantee that every task executes at an exact time.
* Browser rendering does not necessarily occur after every task.
* The browser and Node.js have related but different Event Loop implementations.

# 🗁 Best Practices

* Avoid long-running synchronous operations on the main thread.
* Use asynchronous APIs for I/O operations.
* Use Web Workers for suitable CPU-intensive browser tasks.
* Use `Promise.all()` for independent asynchronous operations.
* Understand microtask scheduling when debugging execution order.
* Avoid continuously scheduling microtasks without yielding.
* Break expensive computations into manageable chunks when appropriate.
* Use browser developer tools to inspect performance and long tasks.
* Learn the differences between browser and Node.js scheduling.

# 🗁 Key Takeaways

* The Event Loop coordinates synchronous and asynchronous JavaScript execution.
* The Call Stack tracks active function execution.
* The Heap stores dynamically allocated data.
* Runtime APIs handle operations outside ordinary JavaScript execution.
* The Task Queue stores callbacks waiting to run.
* The Microtask Queue stores Promise-related and other microtask callbacks.
* Microtasks are processed at microtask checkpoints before the next task is selected.
* Async/await uses Promise-based scheduling.
* Long-running synchronous JavaScript can block the main thread.
* Browser and Node.js Event Loops have different runtime-specific scheduling details.
