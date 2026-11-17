# Microtasks and Macrotasks

JavaScript uses different scheduling mechanisms to manage asynchronous callbacks.

Two important scheduling categories are:

* **Microtasks:** High-priority callbacks processed at microtask checkpoints.
* **Macrotasks:** A commonly used informal term for callbacks scheduled through task queues.

Understanding their execution order is essential for mastering the JavaScript Event Loop.

This topic explains:

* How microtasks and tasks work.
* How their queues are processed.
* How Promises schedule microtasks.
* How timers schedule tasks.
* How nested microtasks affect execution.
* How to predict asynchronous execution order.

# 🗁 What Are Microtasks?

Microtasks are callbacks scheduled to execute at a microtask checkpoint.

They are processed after the current JavaScript execution completes and before the runtime proceeds to another task.

Common sources of microtasks include:

* Promise `.then()` callbacks.
* Promise `.catch()` callbacks.
* Promise `.finally()` callbacks.
* `queueMicrotask()`.
* Mutation Observer callbacks in browsers.

**Example**

```javascript
console.log("Start");

Promise.resolve().then(() => {
  console.log("Microtask");
});

console.log("End");
```

**Output**

```text
Start
End
Microtask
```

The Promise callback does not interrupt the current synchronous execution.

It runs after the current JavaScript execution finishes.

# 🗁 What Are Macrotasks?

Macrotask is a widely used informal term for a task scheduled by the JavaScript runtime.

In browsers, tasks come from different task sources, each with its own scheduling rules.

Common examples include:

* `setTimeout()` callbacks.
* `setInterval()` callbacks.
* User interaction event callbacks.
* Message event callbacks.
* Certain networking-related callbacks.

**Example**

```javascript
console.log("Start");

setTimeout(() => {
  console.log("Task");
}, 0);

console.log("End");
```

**Output**

```text
Start
End
Task
```

The timer callback runs as a later task after the current JavaScript execution completes.

# 🗁 Microtask vs Macrotask

| Feature | Microtask | Macrotask / Task |
| --- | --- | --- |
| Scheduling | Microtask scheduling | Runtime task scheduling |
| Execution | At microtask checkpoints | When selected by the Event Loop |
| Priority | Processed before the next task | Selected according to runtime scheduling |
| Examples | Promise callbacks, `queueMicrotask()` | Timers, events, message callbacks |
| Queue behavior | Drained until empty | Tasks selected individually |
| Typical use | Promise continuations | Timers and event handling |

The term *macrotask* is common in tutorials, but browser specifications generally use the term *task*.

# 🗁 Microtask Queue

The Microtask Queue stores callbacks waiting for a microtask checkpoint.

When the current JavaScript execution finishes, the runtime processes pending microtasks.

Importantly, newly scheduled microtasks are also processed during the same checkpoint.

**Example**

```javascript
console.log("Start");

queueMicrotask(() => {
  console.log("Microtask 1");
});

queueMicrotask(() => {
  console.log("Microtask 2");
});

console.log("End");
```

**Output**

```text
Start
End
Microtask 1
Microtask 2
```

Microtasks execute in their scheduling order.

# 🗁 Task Queue

Tasks are scheduled by the runtime and selected by the Event Loop.

Unlike the simplified idea of one universal macrotask queue, browsers have multiple task sources and scheduling rules.

**Example**

```javascript
console.log("Start");

setTimeout(() => {
  console.log("Timer 1");
}, 0);

setTimeout(() => {
  console.log("Timer 2");
}, 0);

console.log("End");
```

**Typical output**

```text
Start
End
Timer 1
Timer 2
```

For timers scheduled in the same context, their ordering follows the timer scheduling rules.

# 🗁 Execution Order

For common browser examples, the simplified execution sequence is:

* Execute the current synchronous JavaScript.
* Process pending microtasks.
* Continue processing newly scheduled microtasks until the queue is empty.
* Allow rendering when appropriate.
* Select another task.
* Repeat.

**Example**

```javascript
console.log("A");

setTimeout(() => {
  console.log("B");
}, 0);

Promise.resolve().then(() => {
  console.log("C");
});

console.log("D");
```

**Output**

```text
A
D
C
B
```

Explanation:

* `A` executes synchronously.
* The timer callback is scheduled as a task.
* The Promise callback is scheduled as a microtask.
* `D` executes synchronously.
* The microtask runs before the next task.
* The timer callback runs afterward.

# 🗁 queueMicrotask()

`queueMicrotask()` schedules a callback to execute at the next available microtask checkpoint.

**Syntax**

```javascript
queueMicrotask(callback);
```

**Example**

```javascript
console.log("Start");

queueMicrotask(() => {
  console.log("Microtask executed");
});

console.log("End");
```

**Output**

```text
Start
End
Microtask executed
```

It provides a direct way to schedule microtask execution without creating a Promise.

**Using queueMicrotask() vs Promise.resolve().then()**

```javascript
queueMicrotask(() => {
  console.log("Using queueMicrotask");
});

Promise.resolve().then(() => {
  console.log("Using Promise");
});
```

Both callbacks are scheduled as microtasks.

The first callback executes first because it was scheduled first.

Use `queueMicrotask()` when you need to schedule a microtask without needing a Promise chain or a returned Promise.

# 🗁 Promise Callbacks as Microtasks

Promise handlers are scheduled as microtasks when the Promise settles.

**Example**

```javascript
console.log("Start");

const promise = Promise.resolve("Completed");

promise.then(result => {
  console.log(result);
});

console.log("End");
```

**Output**

```text
Start
End
Completed
```

Even an already fulfilled Promise schedules its `.then()` handler asynchronously through the microtask mechanism.

# 🗁 Nested Microtasks

A microtask can schedule another microtask.

The runtime continues processing microtasks until the microtask queue is empty.

**Example**

```javascript
console.log("Start");

Promise.resolve().then(() => {
  console.log("Microtask 1");

  Promise.resolve().then(() => {
    console.log("Microtask 2");
  });
});

queueMicrotask(() => {
  console.log("Microtask 3");
});

console.log("End");
```

**Output**

```text
Start
End
Microtask 1
Microtask 3
Microtask 2
```

Execution explanation:

* The first Promise callback is scheduled.
* The `queueMicrotask()` callback is scheduled.
* Synchronous execution completes.
* The first Promise callback executes.
* The second Promise callback is scheduled at the end of the microtask queue.
* The already queued third callback executes.
* The newly scheduled second callback executes.

This demonstrates that microtasks are processed in queue order, including microtasks added while the queue is being processed.

# 🗁 Microtasks Inside Macrotasks

A task can schedule microtasks.

Those microtasks are processed at the next microtask checkpoint before another task is selected.

**Example**

```javascript
console.log("Start");

setTimeout(() => {
  console.log("Timer");

  Promise.resolve().then(() => {
    console.log("Promise inside timer");
  });
}, 0);

setTimeout(() => {
  console.log("Second timer");
}, 0);

console.log("End");
```

**Output**

```text
Start
End
Timer
Promise inside timer
Second timer
```

After the first timer callback finishes, its Promise callback is processed before the next timer task.

# 🗁 Multiple Promise Callbacks

Multiple Promise handlers are processed according to their scheduling order.

```javascript
console.log("Start");

Promise.resolve().then(() => {
  console.log("Promise 1");
});

Promise.resolve().then(() => {
  console.log("Promise 2");
});

Promise.resolve().then(() => {
  console.log("Promise 3");
});

console.log("End");
```

**Output**

```text
Start
End
Promise 1
Promise 2
Promise 3
```

Each Promise handler is scheduled as a microtask.

# 🗁 async/await and Microtasks

The continuation of an async function after `await` is scheduled through Promise-related microtask processing.

**Example**

```javascript
async function example() {
  console.log("Async start");

  await Promise.resolve();

  console.log("Async continuation");
}

console.log("Start");

example();

console.log("End");
```

**Output**

```text
Start
Async start
End
Async continuation
```

Explanation:

* The main script starts.
* The async function executes synchronously until `await`.
* The function suspends.
* The main script continues.
* The function resumes through a microtask.

# 🗁 Microtask Starvation

Microtask starvation happens when microtasks continuously schedule more microtasks.

Because the runtime drains the microtask queue before proceeding to another task, an endless chain can prevent other work from receiving execution time.

**Example**

```javascript
function repeat() {
  queueMicrotask(repeat);
}

repeat();

setTimeout(() => {
  console.log("Timer executed");
}, 0);
```

The recursive microtask scheduling prevents the microtask queue from becoming empty.

Consequently, the timer callback cannot execute.

This can also delay browser rendering and user interactions.

**Better approach**

Use task scheduling to yield execution periodically.

```javascript
function processItems(items) {
  let index = 0;

  function processChunk() {
    const chunkSize = 100;

    const end = Math.min(
      index + chunkSize,
      items.length
    );

    while (index < end) {
      // Process one item
      index++;
    }

    if (index < items.length) {
      setTimeout(processChunk, 0);
    }
  }

  processChunk();
}
```

This gives the runtime opportunities to process other tasks between chunks.

# 🗁 Browser Rendering and Microtasks

Browsers generally perform microtask checkpoints before rendering opportunities.

A long-running microtask chain can delay:

* DOM updates becoming visible.
* Animation frames.
* User interactions.
* Other scheduled tasks.

**Example**

```javascript
document.querySelector("#message").textContent = "Updated";

queueMicrotask(() => {
  console.log("Microtask executed");
});
```

The DOM property changes synchronously, but the browser generally gets an opportunity to render the updated content after the current JavaScript execution and microtask processing.

For animation-related work, `requestAnimationFrame()` can schedule a callback before a browser repaint.

# 🗁 Execution Order Practice

**Example: Mixed scheduling**

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

setTimeout(() => {
  console.log("E");
}, 0);

console.log("F");
```

**Output**

```text
A
F
C
D
B
E
```

**Explanation**

* `A` executes synchronously.
* The first timer is scheduled.
* The Promise callback is scheduled as a microtask.
* The `queueMicrotask()` callback is scheduled.
* The second timer is scheduled.
* `F` executes synchronously.
* Microtasks execute in order: `C`, then `D`.
* Timer callbacks execute afterward: `B`, then `E`.

# 🗁 Common Mistakes

* Assuming `setTimeout(callback, 0)` executes immediately.
* Assuming all asynchronous callbacks use the same queue.
* Thinking Promise callbacks execute synchronously when a Promise is already fulfilled.
* Assuming `await` blocks the entire JavaScript thread.
* Forgetting that microtasks can schedule additional microtasks.
* Assuming browsers always render after every task.
* Using endless microtask scheduling for long-running processing.
* Treating the term macrotask as a formal browser specification term.

# 🗁 Best Practices

* Use Promises and `queueMicrotask()` for microtask scheduling.
* Use timers when work should be scheduled as a later task.
* Avoid unnecessarily large microtask chains.
* Break expensive operations into smaller chunks.
* Understand microtask checkpoints when debugging asynchronous behavior.
* Use `requestAnimationFrame()` for animation-related updates.
* Use Web Workers for suitable CPU-intensive operations.
* Remember that browser task scheduling is more complex than a single universal FIFO macrotask queue.

# 🗁 Key Takeaways

* Microtasks and tasks are different scheduling mechanisms.
* Promise handlers execute as microtasks.
* `queueMicrotask()` schedules a microtask directly.
* Timers schedule callbacks as tasks.
* Microtasks are processed at microtask checkpoints before the next task is selected.
* Newly scheduled microtasks are processed during the same checkpoint.
* Endless microtask scheduling can cause starvation.
* Browser rendering can be delayed by long-running JavaScript and microtask chains.
* Understanding scheduling order is essential for debugging asynchronous JavaScript.
