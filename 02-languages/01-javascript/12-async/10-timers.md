# JavaScript Timers

JavaScript timers allow code to execute after a specified delay or repeatedly at a specified interval. They are provided by the host environment, such as web browsers and Node.js, rather than being part of the core ECMAScript language.

The primary timer functions are:

* `setTimeout()` — schedules a callback to run once after a delay.
* `setInterval()` — schedules a callback to run repeatedly at an interval.
* `clearTimeout()` — cancels a scheduled timeout.
* `clearInterval()` — cancels a scheduled interval.

Timers are commonly used for delayed actions, periodic updates, animations, polling, debouncing, and scheduling background tasks.

# 🗁 setTimeout()

`setTimeout()` schedules a function to execute once after a specified delay.

**Syntax**

```javascript
const timeoutId = setTimeout(callback, delay, ...args);
```

* `callback` — function to execute.
* `delay` — minimum delay in milliseconds.
* `...args` — optional arguments passed to the callback.
* `timeoutId` — identifier used to cancel the scheduled timeout.

**Basic example**

```javascript
setTimeout(() => {
    console.log("Executed after 2 seconds");
}, 2000);
```

The callback becomes eligible to execute after approximately two seconds.

**Passing arguments**

```javascript
function greet(name) {
    console.log(`Hello, ${name}!`);
}

setTimeout(greet, 1000, "Muhammad");
```

The arguments after the delay are passed to the callback.

**Important behavior**

The delay is not an exact execution time. It specifies the minimum waiting period before the callback can run.

If the main thread is busy, the callback must wait until the current JavaScript execution finishes.

# 🗁 setInterval()

`setInterval()` repeatedly executes a callback after a specified interval.

**Syntax**

```javascript
const intervalId = setInterval(callback, delay, ...args);
```

* `callback` — function to execute repeatedly.
* `delay` — interval duration in milliseconds.
* `...args` — optional callback arguments.
* `intervalId` — identifier used to stop the interval.

**Basic example**

```javascript
const intervalId = setInterval(() => {
    console.log("Executed every second");
}, 1000);
```

The callback is scheduled repeatedly until the interval is canceled.

**Passing arguments**

```javascript
function displayMessage(message) {
    console.log(message);
}

setInterval(displayMessage, 2000, "Hello!");
```

This displays `Hello!` approximately every two seconds.

# 🗁 clearTimeout()

`clearTimeout()` cancels a timeout that has not yet executed.

**Syntax**

```javascript
clearTimeout(timeoutId);
```

**Example**

```javascript
const timeoutId = setTimeout(() => {
    console.log("This will not execute");
}, 3000);

clearTimeout(timeoutId);
```

The scheduled callback is canceled before execution.

Calling `clearTimeout()` with an invalid or already completed timeout identifier has no effect.

# 🗁 clearInterval()

`clearInterval()` stops a repeating interval.

**Syntax**

```javascript
clearInterval(intervalId);
```

**Example**

```javascript
let count = 0;

const intervalId = setInterval(() => {
    count++;

    console.log(`Count: ${count}`);

    if (count === 5) {
        clearInterval(intervalId);
    }
}, 1000);
```

The interval stops after five executions.

# 🗁 setTimeout() vs setInterval()

| Feature | setTimeout() | setInterval() |
| --- | --- | --- |
| Execution | Once | Repeatedly |
| Scheduling | After a delay | At repeated intervals |
| Cancellation | `clearTimeout()` | `clearInterval()` |
| Common use | Delayed actions | Periodic tasks |
| Repetition | Requires rescheduling | Automatic |

**Example comparison**

```javascript
// Executes once after 2 seconds.
setTimeout(() => {
    console.log("Timeout");
}, 2000);

// Executes repeatedly every 2 seconds.
setInterval(() => {
    console.log("Interval");
}, 2000);
```

# 🗁 Timer Execution and the Event Loop

JavaScript timers do not interrupt currently executing synchronous code.

When a timer expires, its callback becomes eligible to execute. The event loop can process it when the call stack is available.

```javascript
console.log("Start");

setTimeout(() => {
    console.log("Timer");
}, 0);

console.log("End");
```

**Output**

```text
Start
End
Timer
```

Even with a delay of `0`, the callback does not execute immediately.

A zero-delay timeout schedules asynchronous work for a later opportunity.

# 🗁 Zero-Delay Timers

A zero-delay timeout schedules a callback as soon as the current execution and higher-priority queued work allow it.

```javascript
console.log("First");

setTimeout(() => {
    console.log("Third");
}, 0);

console.log("Second");
```

**Output**

```text
First
Second
Third
```

Zero-delay timers are useful when execution needs to be deferred until after the current synchronous code.

They do not guarantee immediate execution.

# 🗁 Timer Delays and Accuracy

Timer delays represent minimum waiting periods, not guaranteed execution times.

Several factors can delay a callback:

* Long-running synchronous JavaScript.
* Other tasks waiting in the event loop.
* Browser scheduling and background-tab throttling.
* Runtime-specific timer limitations.
* System resource constraints.

**Example**

```javascript
setTimeout(() => {
    console.log("Timer executed");
}, 1000);

const start = Date.now();

while (Date.now() - start < 3000) {
    // Block the main thread for approximately 3 seconds.
}

console.log("Synchronous work completed");
```

Although the timer delay is one second, its callback cannot execute while the main thread is blocked.

**Output**

```text
Synchronous work completed
Timer executed
```

# 🗁 Nested setTimeout()

A timeout can schedule another timeout from inside its callback.

```javascript
function repeatTask() {
    console.log("Task executed");

    setTimeout(repeatTask, 1000);
}

setTimeout(repeatTask, 1000);
```

This creates repeated execution using recursive scheduling.

Unlike `setInterval()`, the next timeout is scheduled after the current callback finishes.

This approach is useful when the next execution should wait until the previous operation has completed.

# 🗁 setInterval() vs Recursive setTimeout()

Consider a task that performs an asynchronous operation.

With `setInterval()`, another callback can begin even when a previous asynchronous operation is still running.

```javascript
setInterval(async () => {
    await fetch("/api/data");

    console.log("Request completed");
}, 1000);
```

If the request takes longer than one second, multiple requests may overlap.

Recursive `setTimeout()` provides better control:

```javascript
async function poll() {
    try {
        const response = await fetch("/api/data");
        const data = await response.json();

        console.log(data);
    } catch (error) {
        console.error("Request failed:", error);
    }

    setTimeout(poll, 1000);
}

poll();
```

The next polling operation is scheduled only after the current operation completes.

This prevents overlapping requests caused by the timer itself.

# 🗁 Debouncing with setTimeout()

Debouncing delays execution until a period of inactivity has passed.

It is commonly used for:

* Search input suggestions.
* Form validation.
* Window resize handlers.
* API requests triggered by user input.

**Example**

```javascript
let timeoutId;

function debounceSearch(callback, delay) {
    return (...args) => {
        clearTimeout(timeoutId);

        timeoutId = setTimeout(() => {
            callback(...args);
        }, delay);
    };
}

const search = debounceSearch((value) => {
    console.log("Searching for:", value);
}, 500);

search("J");
search("Ja");
search("Java");
search("JavaScript");
```

Only the final call executes if no new call occurs during the 500-millisecond delay.

For reusable implementations, the timeout identifier should generally be stored inside the returned function's closure rather than shared globally.

```javascript
function debounce(callback, delay) {
    let timeoutId;

    return function (...args) {
        clearTimeout(timeoutId);

        timeoutId = setTimeout(() => {
            callback.apply(this, args);
        }, delay);
    };
}
```

# 🗁 Throttling with setTimeout()

Throttling limits how frequently a function can execute.

It is useful for frequently triggered events such as scrolling and mouse movement.

```javascript
function throttle(callback, delay) {
    let lastExecution = 0;

    return function (...args) {
        const now = Date.now();

        if (now - lastExecution >= delay) {
            lastExecution = now;
            callback.apply(this, args);
        }
    };
}

const handleScroll = throttle(() => {
    console.log("Scroll event handled");
}, 1000);

window.addEventListener("scroll", handleScroll);
```

This implementation allows the callback to execute at most once per delay period while events continue to occur.

Debouncing waits for inactivity, whereas throttling limits execution frequency.

# 🗁 Timer Identifiers

Both `setTimeout()` and `setInterval()` return identifiers that can be used to cancel their scheduled callbacks.

```javascript
const timeoutId = setTimeout(() => {
    console.log("Timeout executed");
}, 2000);

const intervalId = setInterval(() => {
    console.log("Interval executed");
}, 1000);

clearTimeout(timeoutId);
clearInterval(intervalId);
```

In browsers, these identifiers are commonly numeric values. In Node.js, timer functions return `Timeout` objects.

Avoid depending on a particular identifier type when writing cross-environment JavaScript.

# 🗁 Browser and Node.js Timers

Timers are host-provided APIs, and their implementations differ between environments.

**Browser**

```javascript
setTimeout(() => {
    console.log("Browser timer");
}, 1000);
```

Browser timers are commonly used for UI-related scheduling and delayed interactions.

**Node.js**

```javascript
setTimeout(() => {
    console.log("Node.js timer");
}, 1000);
```

Node.js provides timers through its runtime and event loop.

Node.js also exposes additional timer-related capabilities, including:

* `setImmediate()` — schedules a callback for the check phase of the Node.js event loop.
* `process.nextTick()` — schedules a callback in Node.js's next-tick queue.
* `timers/promises` — provides promise-based timer utilities.

These APIs are specific to Node.js and are not standard browser timer functions.

# 🗁 Promise-Based Timers

Modern JavaScript applications can use promises with timers.

A promise-based timeout can be implemented manually:

```javascript
function delay(milliseconds) {
    return new Promise((resolve) => {
        setTimeout(resolve, milliseconds);
    });
}

async function runTask() {
    console.log("Starting");

    await delay(2000);

    console.log("Finished after 2 seconds");
}

runTask();
```

This allows asynchronous code to wait without blocking the main thread.

Node.js also provides promise-based timers:

```javascript
import { setTimeout } from "node:timers/promises";

async function runTask() {
    console.log("Starting");

    await setTimeout(2000);

    console.log("Finished");
}

runTask();
```

The Node.js implementation supports additional options, including cancellation with an `AbortSignal`.

# 🗁 Canceling Promise-Based Timers

A promise-based timer can support cancellation through `AbortController`.

**Example using a custom delay function**

```javascript
function delay(milliseconds, signal) {
    return new Promise((resolve, reject) => {
        if (signal?.aborted) {
            reject(new Error("Operation aborted"));
            return;
        }

        const timeoutId = setTimeout(() => {
            cleanup();
            resolve();
        }, milliseconds);

        function onAbort() {
            clearTimeout(timeoutId);
            cleanup();
            reject(new Error("Operation aborted"));
        }

        function cleanup() {
            signal?.removeEventListener("abort", onAbort);
        }

        signal?.addEventListener("abort", onAbort, {
            once: true
        });
    });
}

const controller = new AbortController();

delay(5000, controller.signal)
    .then(() => console.log("Completed"))
    .catch((error) => console.log(error.message));

setTimeout(() => {
    controller.abort();
}, 1000);
```

The operation is canceled after one second rather than waiting for the full five-second delay.

# 🗁 Common Timer Mistakes

**Calling a function instead of passing a callback**

Incorrect:

```javascript
setTimeout(console.log("Hello"), 1000);
```

The function executes immediately, and its return value is passed to `setTimeout()`.

Correct:

```javascript
setTimeout(() => {
    console.log("Hello");
}, 1000);
```

**Forgetting to clear intervals**

Incorrect:

```javascript
setInterval(() => {
    console.log("Running");
}, 1000);
```

If the interval is no longer needed, it should be canceled.

Correct:

```javascript
const intervalId = setInterval(() => {
    console.log("Running");
}, 1000);

clearInterval(intervalId);
```

**Assuming timers execute at exact times**

Incorrect assumption:

```javascript
setTimeout(task, 1000);
```

The callback is not guaranteed to execute exactly one second later.

**Creating overlapping asynchronous operations**

Using `setInterval()` for asynchronous operations can create concurrent requests when execution takes longer than the interval.

Use recursive `setTimeout()` when the next operation must wait for the current one to finish.

# 🗁 Best Practices

* Store timer identifiers when cancellation may be necessary.
* Clear unnecessary timers to avoid unwanted work.
* Use `setTimeout()` for one-time delayed operations.
* Use `setInterval()` for simple recurring synchronous tasks.
* Prefer recursive `setTimeout()` when recurring asynchronous work must not overlap.
* Use debouncing for actions that should run after user inactivity.
* Use throttling to limit frequently triggered operations.
* Avoid blocking the main thread with long-running synchronous code.
* Do not assume timer delays guarantee exact execution times.
* Use `requestAnimationFrame()` for browser UI animations instead of relying on timer intervals.
* Consider background-tab throttling when designing browser applications.

# 🗁 Quick Reference

| Function | Purpose |
| --- | --- |
| `setTimeout()` | Execute once after a delay |
| `clearTimeout()` | Cancel a timeout |
| `setInterval()` | Execute repeatedly |
| `clearInterval()` | Cancel an interval |
| `setImmediate()` | Schedule a Node.js callback for the check phase |
| `process.nextTick()` | Schedule a Node.js next-tick callback |
| `requestAnimationFrame()` | Schedule browser animation work |
| `AbortController` | Cancel supported asynchronous operations |

# 🗁 Practice Exercises

* Create a timeout that displays a message after three seconds.
* Create an interval that displays numbers from 1 to 10.
* Stop an interval automatically after five executions.
* Create a countdown timer that displays the remaining seconds.
* Implement a reusable `delay()` function using promises.
* Implement a debounce function for a search input.
* Implement a throttle function for a scroll event.
* Create a polling function using recursive `setTimeout()`.
* Add cancellation support to a promise-based delay function.
* Compare `setInterval()` and recursive `setTimeout()` using an asynchronous API request.

# 🗁 Key Takeaways

* JavaScript timers are provided by the host environment.
* `setTimeout()` schedules one callback after a minimum delay.
* `setInterval()` schedules recurring callbacks.
* `clearTimeout()` and `clearInterval()` cancel scheduled timers.
* Timer callbacks do not interrupt synchronous JavaScript execution.
* A zero-delay timeout is asynchronous, not immediate.
* Timer execution depends on the event loop and runtime scheduling.
* Recursive `setTimeout()` provides better control over asynchronous recurring tasks.
* Debouncing and throttling are common timer-based programming patterns.
* Promise-based timers integrate naturally with `async` and `await`.
