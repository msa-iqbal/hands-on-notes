# JavaScript Event Loop

The **Event Loop** is a JavaScript runtime mechanism that coordinates the execution of synchronous code, asynchronous callbacks, and other scheduled operations.

JavaScript executes synchronous code using a call stack. However, operations such as timers, network requests, user interactions, and file operations may complete asynchronously.

The event loop helps coordinate when their associated callbacks or continuations can execute.

Understanding the event loop is essential for understanding:

* Asynchronous JavaScript
* Callbacks
* Promises
* Async/Await
* Microtasks and Macrotasks
* Browser APIs
* Node.js asynchronous execution

# 🗁 Why Does JavaScript Need an Event Loop?

JavaScript typically executes application code on a single main thread in a browser.

If every operation had to finish before the next operation could begin, slow operations could block the entire application.

Consider:

```javascript
console.log("Start");

setTimeout(() => {
    console.log("Timer finished");
}, 2000);

console.log("End");
```

Output:

```text
Start
End
Timer finished
```

The timer does not block the synchronous execution of the remaining code.

The runtime manages the timer, and its callback becomes eligible for execution after the timer expires.

The event loop coordinates when that callback can run.

# 🗁 JavaScript Runtime Environment

The event loop operates as part of a JavaScript runtime environment.

A browser runtime provides several important components:

* JavaScript Engine
* Call Stack
* Web APIs
* Task Queues
* Microtask Queue
* Event Loop

Node.js provides its own runtime architecture, including the event loop and APIs implemented through components such as libuv.

A simplified browser runtime model:

```text
             JavaScript Runtime
    ┌────────────────────────────────┐
    │                                │
    │       JavaScript Engine        │
    │                                │
    │       ┌──────────────┐         │
    │       │  Call Stack  │         │
    │       └──────────────┘         │
    │                                │
    │       ┌──────────────┐         │
    │       │   Event Loop │         │
    │       └──────────────┘         │
    │                                │
    │   ┌──────────┐  ┌───────────┐  │
    │   │  Tasks   │  │ Microtasks│  │
    │   └──────────┘  └───────────┘  │
    │                                │
    └────────────────────────────────┘
             │
             ▼
       Browser Web APIs
```

This diagram is a conceptual model rather than a literal implementation layout.

# 🗁 Call Stack

The **Call Stack** tracks active execution contexts.

When JavaScript invokes a function, its execution context is added to the stack.

When the function returns, its execution context is removed.

Example:

```javascript
function first() {
    second();
}

function second() {
    console.log("Second function");
}

first();
```

The call stack manages the nested execution of these functions.

The event loop does not interrupt a currently executing synchronous function to run another callback.

# 🗁 Web APIs

Web APIs are browser-provided capabilities that allow JavaScript applications to perform operations outside the immediate execution of JavaScript code.

Examples include:

* `setTimeout()`
* `setInterval()`
* `fetch()`
* DOM events
* Browser storage APIs
* Geolocation APIs

Example:

```javascript
console.log("Start");

setTimeout(() => {
    console.log("Timer callback");
}, 1000);

console.log("End");
```

The browser manages the timer operation. When the timer expires, the callback becomes eligible to be scheduled as a task.

The JavaScript engine executes the callback when the runtime allows it to run.

# 🗁 Task Queue

The **Task Queue** stores tasks that are ready to execute.

Tasks are also commonly called macrotasks in JavaScript learning materials.

Examples of task sources include:

* Timer callbacks
* User interaction events
* Message events
* Some browser scheduling operations

Example:

```javascript
console.log("First");

setTimeout(() => {
    console.log("Timer");
}, 0);

console.log("Second");
```

Output:

```text
First
Second
Timer
```

Even with a delay of `0`, the timer callback does not execute immediately.

It must first become eligible for scheduling, and the current synchronous execution must finish.

# 🗁 Microtask Queue

The **Microtask Queue** stores microtasks scheduled for execution.

Common sources include:

* Promise fulfillment handlers
* Promise rejection handlers
* `queueMicrotask()`
* Mutation observer callbacks in browsers

Example:

```javascript
console.log("Start");

Promise.resolve().then(() => {
    console.log("Promise callback");
});

console.log("End");
```

Output:

```text
Start
End
Promise callback
```

The Promise callback runs after the current synchronous JavaScript execution completes.

# 🗁 Event Loop Execution Cycle

The event loop coordinates the execution of tasks and microtasks.

A simplified browser execution cycle looks like this:

* Execute the current JavaScript task.
* Complete the current synchronous execution.
* Perform a microtask checkpoint and process pending microtasks.
* Continue processing microtasks until the microtask queue is empty.
* The browser may perform rendering-related work at an appropriate point.
* Select and execute another task when one is available.
* Repeat the cycle.

The exact scheduling and rendering behavior is defined by the browser runtime specifications and task sources.

Conceptual flow:

```text
       Start JavaScript Execution
                  |
                  ▼
           Execute Task
                  |
                  ▼
       Run Synchronous Code
                  |
                  ▼
          Call Stack Empty?
                  |
                  ▼
          Process Microtasks
                  |
                  ▼
         Microtask Queue Empty?
                  |
                  ▼
       Rendering Opportunity
                  |
                  ▼
          Select Next Task
                  |
                  ▼
           Repeat Event Loop
```

A microtask checkpoint may process multiple microtasks because executing one microtask can schedule additional microtasks.

# 🗁 Synchronous vs Asynchronous Execution

Synchronous operations execute in sequence.

Asynchronous operations allow the current execution to continue while the runtime handles other work.

Example:

```javascript
console.log("One");

console.log("Two");

console.log("Three");
```

Output:

```text
One
Two
Three
```

Asynchronous example:

```javascript
console.log("One");

setTimeout(() => {
    console.log("Two");
}, 0);

console.log("Three");
```

Output:

```text
One
Three
Two
```

The timer callback executes after the synchronous code finishes.

# 🗁 Microtasks vs Tasks

Microtasks and tasks are scheduled through different mechanisms.

| Feature | Microtasks | Tasks |
| --- | --- | --- |
| Common name | Microtasks | Macrotasks |
| Scheduling | Microtask queue | Task queues |
| Common examples | Promise handlers, `queueMicrotask()` | Timers, DOM events |
| Execution timing | At microtask checkpoints | When selected by the event loop |
| Priority relationship | Processed before the next task at a microtask checkpoint | Run after the current task and its microtask checkpoint |

Example:

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

Output:

```text
Start
End
Promise
Timeout
```

The Promise reaction runs during the microtask checkpoint before the next timer task in this example.

# 🗁 Promise Execution and the Event Loop

Promises use microtasks to schedule their fulfillment and rejection handlers.

Example:

```javascript
console.log("Start");

const promise = Promise.resolve("Completed");

promise.then((result) => {
    console.log(result);
});

console.log("End");
```

Output:

```text
Start
End
Completed
```

The Promise is already fulfilled, but its `.then()` handler is still scheduled asynchronously as a microtask.

This distinction is important: Promise settlement and Promise handler execution are not the same event.

# 🗁 queueMicrotask()

The `queueMicrotask()` method schedules a callback to run during a microtask checkpoint.

Example:

```javascript
console.log("Start");

queueMicrotask(() => {
    console.log("Microtask");
});

console.log("End");
```

Output:

```text
Start
End
Microtask
```

It is useful when code needs to schedule work after the current synchronous execution but before the next task is processed.

# 🗁 setTimeout() and the Event Loop

`setTimeout()` schedules a timer callback.

The specified delay represents a minimum waiting period before the callback becomes eligible for execution, not an exact execution time.

Example:

```javascript
console.log("Start");

setTimeout(() => {
    console.log("Timer callback");
}, 1000);

console.log("End");
```

Output:

```text
Start
End
Timer callback
```

The callback may execute later than the specified delay because of:

* Long-running synchronous JavaScript
* Pending tasks
* Runtime scheduling
* Browser timer throttling
* Other execution constraints

# 🗁 Nested Timers

Timers can schedule additional timers.

Example:

```javascript
console.log("Start");

setTimeout(() => {
    console.log("First timer");

    setTimeout(() => {
        console.log("Second timer");
    }, 0);
}, 0);

console.log("End");
```

Output:

```text
Start
End
First timer
Second timer
```

The second timer is scheduled only after the first timer callback executes.

It therefore cannot run before the first callback finishes.

# 🗁 Event Loop and async/await

The `async` and `await` syntax provides a convenient way to work with Promises.

An `async` function always returns a Promise.

When execution reaches an `await` expression, the function pauses its remaining execution and schedules its continuation through Promise-related microtask processing.

Example:

```javascript
console.log("Start");

async function process() {
    console.log("Inside function");

    await Promise.resolve();

    console.log("After await");
}

process();

console.log("End");
```

Output:

```text
Start
Inside function
End
After await
```

The code before `await` executes synchronously when the function is invoked.

The continuation after `await` executes asynchronously.

# 🗁 Execution Order Example

Consider a more detailed example:

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

Output:

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
* The Promise handler is scheduled as a microtask.
* `queueMicrotask()` schedules another microtask.
* `E` executes synchronously.
* The current task finishes.
* The microtask checkpoint executes `C`.
* The next microtask executes `D`.
* The event loop later selects the timer task and executes `B`.

# 🗁 Microtasks Can Delay Tasks

Microtasks are processed until the microtask queue is empty.

If microtasks continuously schedule additional microtasks, the runtime may spend a long time processing them before moving to another task.

Example:

```javascript
function repeatMicrotask() {
    queueMicrotask(() => {
        console.log("Microtask");

        repeatMicrotask();
    });
}

repeatMicrotask();

setTimeout(() => {
    console.log("Timer");
}, 0);
```

This program continuously schedules microtasks.

The timer may never execute because the microtask queue never becomes empty.

This behavior can cause **microtask starvation** and prevent the browser from making progress on other tasks or rendering.

Avoid creating unbounded microtask chains.

# 🗁 Event Loop in Node.js

Node.js has its own event loop implementation and scheduling behavior.

It uses libuv to coordinate many asynchronous operations.

The Node.js event loop includes several phases:

* Timers
* Pending Callbacks
* Idle, Prepare
* Poll
* Check
* Close Callbacks

The phases determine when different categories of callbacks can execute.

Node.js also provides scheduling mechanisms such as:

* `process.nextTick()`
* Promise microtasks
* `setTimeout()`
* `setImmediate()`

Example:

```javascript
console.log("Start");

setTimeout(() => {
    console.log("Timeout");
}, 0);

setImmediate(() => {
    console.log("Immediate");
});

console.log("End");
```

The relative order of the timer and immediate callback at the top level can vary depending on runtime conditions and Node.js version.

Therefore, do not assume that one always executes before the other.

# 🗁 Browser Event Loop vs Node.js Event Loop

Both browser JavaScript and Node.js use event-loop-based asynchronous execution, but their runtime environments differ.

| Feature | Browser | Node.js |
| --- | --- | --- |
| Runtime | Web browser | Node.js |
| Event loop | Browser event loop model | libuv-based event loop |
| APIs | DOM, Fetch, timers, browser APIs | File system, networking, timers |
| Rendering | Browser rendering opportunities | No browser rendering pipeline |
| Task scheduling | Browser task sources | Node.js event-loop phases |
| Additional scheduling | Promise microtasks, `queueMicrotask()` | Promise microtasks, `queueMicrotask()`, `process.nextTick()` |

The underlying JavaScript language is shared, but asynchronous scheduling details depend on the host environment.

# 🗁 Common Misconceptions

**`setTimeout(callback, 0)` executes immediately.**

Incorrect. It schedules a callback that becomes eligible after the timer delay and runtime scheduling conditions are satisfied.

**Promises execute synchronously.**

Incorrect. The Promise executor runs synchronously when the Promise is constructed, but `.then()`, `.catch()`, and `.finally()` handlers execute asynchronously as microtasks.

**The event loop executes JavaScript in parallel.**

Incorrect. The event loop coordinates scheduling. It does not make synchronous JavaScript execution on one thread parallel.

**An asynchronous function runs entirely asynchronously.**

Incorrect. An `async` function begins executing synchronously and continues asynchronously after an `await` that suspends its execution.

**Microtasks always execute one at a time without interruption.**

Each microtask executes to completion, but it can schedule additional microtasks. The runtime continues processing microtasks until the queue is empty.

# 🗁 Best Practices

* Understand the difference between synchronous code, tasks, and microtasks.
* Use Promises and `async/await` for asynchronous workflows.
* Avoid long-running synchronous operations on the browser's main thread.
* Avoid continuously scheduling microtasks.
* Do not assume timers execute at exact specified times.
* Understand browser and Node.js scheduling differences.
* Use Web Workers or Node.js worker threads for suitable CPU-intensive operations.
* Use browser developer tools to inspect asynchronous execution and performance.

# 🗁 Key Takeaways

* The event loop coordinates synchronous and asynchronous JavaScript execution.
* The call stack tracks active execution contexts.
* Browser APIs and runtime facilities manage asynchronous operations.
* Tasks and microtasks use different scheduling mechanisms.
* Promise handlers execute as microtasks.
* Timer callbacks execute as tasks.
* Microtasks are processed at microtask checkpoints before the event loop proceeds to another task.
* `async/await` uses Promise-based asynchronous execution.
* Browser and Node.js event loops have different runtime-specific scheduling behavior.
* Unbounded microtask scheduling can cause starvation.

# 🗁 Related Topics

* Execution Context
* Call Stack
* Execution Model
* Synchronous vs Asynchronous JavaScript
* Callbacks
* Promises
* Promise Methods
* Async/Await
* Microtasks and Macrotasks
* Timers
* Web Workers
* Node.js Runtime
