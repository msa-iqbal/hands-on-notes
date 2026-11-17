# Web Workers

Web Workers allow JavaScript applications to execute scripts in background threads, separate from the main browser thread.

Normally, JavaScript running in a browser executes on the main thread, which is also responsible for many user interface operations, including:

* Rendering page content.
* Processing user interactions.
* Updating the DOM.
* Handling animations.
* Executing JavaScript.

Long-running JavaScript operations can block the main thread and make an application unresponsive.

Web Workers solve this problem by moving computational tasks into separate worker threads.

Common use cases include:

* Large data processing.
* Complex calculations.
* Image processing.
* Parsing large files.
* Background computations.
* Data transformations.
* Computationally intensive algorithms.

# 🗁 Understanding the Main Thread

The main thread executes JavaScript and handles browser interface operations.

Consider this example:

```javascript
function calculate() {
  let result = 0;

  for (let i = 0; i < 1_000_000_000; i++) {
    result += i;
  }

  console.log(result);
}

calculate();
```

This computation blocks the main thread until it finishes.

During that time:

* The page may become unresponsive.
* Buttons may not respond immediately.
* Animations may freeze.
* Rendering may be delayed.

This is known as main-thread blocking.

# 🗁 How Web Workers Work

Web Workers execute JavaScript in a separate execution context.

The main thread and worker communicate through message passing.

```text
+---------------------------+
|        Main Thread        |
|                           |
|  Browser UI               |
|  DOM                      |
|  User Interactions        |
|                           |
+-------------+-------------+
              |
              | postMessage()
              |
              v
+---------------------------+
|       Worker Thread       |
|                           |
|  Background JavaScript    |
|  Calculations             |
|  Data Processing          |
|                           |
+-------------+-------------+
              |
              | postMessage()
              |
              v
+---------------------------+
|        Main Thread        |
|                           |
|  Receives Result          |
|  Updates DOM              |
|                           |
+---------------------------+
```

The worker performs background computations while the main thread remains available for interface operations.

# 🗁 Types of Web Workers

The browser provides different worker mechanisms.

| Type | Description |
| --- | --- |
| Dedicated Worker | Used by a single owning script |
| Shared Worker | Can be shared by multiple browsing contexts |
| Service Worker | Acts as an intermediary between an application, browser, and network |

This document primarily focuses on Dedicated Workers and Shared Workers. Service Workers are covered separately in the Service Worker API ecosystem.

# 🗁 Creating a Dedicated Worker

A Dedicated Worker can be created using the `Worker` constructor.

Syntax:

```javascript
const worker = new Worker(workerScript);
```

Example project structure:

```text
project/
├── index.html
├── main.js
└── worker.js
```

The worker script runs independently from the main JavaScript file.

# 🗁 Creating a Worker Script

Create a file named `worker.js`.

```javascript
console.log("Worker started");

self.postMessage("Hello from worker");
```

Inside a worker, `self` refers to the worker's global scope.

The worker uses `postMessage()` to send data back to its owner.

# 🗁 Communicating with a Worker

The main thread creates the worker and listens for messages.

`main.js`:

```javascript
const worker = new Worker("./worker.js");

worker.addEventListener("message", (event) => {
  console.log("Message from worker:", event.data);
});
```

Output:

```text
Message from worker: Hello from worker
```

The worker and main thread communicate using messages rather than directly sharing ordinary JavaScript variables.

# 🗁 postMessage()

The `postMessage()` method sends data from one execution context to another.

Main thread:

```javascript
worker.postMessage("Hello Worker");
```

Worker:

```javascript
self.addEventListener("message", (event) => {
  console.log(event.data);
});
```

The message event contains the received data in its `data` property.

# 🗁 Receiving Messages

Both the main thread and worker can listen for incoming messages.

Main thread:

```javascript
worker.addEventListener("message", (event) => {
  console.log(event.data);
});
```

Worker:

```javascript
self.addEventListener("message", (event) => {
  console.log(event.data);
});
```

The same messaging mechanism works in both directions.

# 🗁 Practical Example: Background Calculation

This example demonstrates performing a calculation inside a worker.

Project structure:

```text
project/
├── index.html
├── main.js
└── worker.js
```

`worker.js`:

```javascript
self.addEventListener("message", (event) => {
  const number = event.data;

  let result = 0;

  for (let i = 1; i <= number; i++) {
    result += i;
  }

  self.postMessage(result);
});
```

`main.js`:

```javascript
const worker = new Worker("./worker.js");

worker.addEventListener("message", (event) => {
  console.log("Calculation result:", event.data);
});

worker.postMessage(1000000);
```

The worker calculates the sum and sends the result to the main thread.

The main thread does not need to perform the calculation itself.

# 🗁 Passing Objects to Workers

Workers can receive structured data, including objects and arrays.

Example:

```javascript
worker.postMessage({
  operation: "multiply",
  numbers: [10, 20, 30]
});
```

Worker:

```javascript
self.addEventListener("message", (event) => {
  const { operation, numbers } = event.data;

  if (operation === "multiply") {
    const result = numbers.reduce(
      (total, number) => total * number,
      1
    );

    self.postMessage(result);
  }
});
```

The browser uses the structured clone algorithm to transfer supported values between execution contexts.

# 🗁 Structured Clone Algorithm

The structured clone algorithm is used to copy supported data between workers and other browser APIs.

Supported values include:

* Strings.
* Numbers.
* Booleans.
* Arrays.
* Plain objects.
* Maps and Sets.
* Dates.
* ArrayBuffers.
* Blobs.
* Many other structured-cloneable values.

Some values cannot be cloned, including:

* Functions.
* DOM nodes.
* Objects containing unsupported values.

Attempting to clone unsupported data can result in a `DataCloneError`.

Example:

```javascript
worker.postMessage({
  name: "Iqbal",
  calculate: () => 10
});
```

This fails because functions cannot be cloned.

# 🗁 Transferable Objects

Some objects can be transferred instead of copied.

Transferable objects change ownership from one execution context to another.

This avoids copying large binary data structures.

Common transferable objects include:

* `ArrayBuffer`.
* `MessagePort`.
* `ImageBitmap`.
* `OffscreenCanvas`.

Example:

```javascript
const buffer = new ArrayBuffer(1024);

worker.postMessage(buffer, [buffer]);
```

After transferring the buffer, the original `ArrayBuffer` in the sending context is detached.

```javascript
console.log(buffer.byteLength); // 0
```

The worker now owns the transferred buffer.

Transfer is useful when working with large binary data.

# 🗁 Worker Error Handling

Workers can generate errors during execution.

The main thread can listen for error events.

```javascript
worker.addEventListener("error", (event) => {
  console.error("Worker error:", event.message);

  console.error("File:", event.filename);

  console.error("Line:", event.lineno);
});
```

You can also listen for message deserialization errors:

```javascript
worker.addEventListener("messageerror", (event) => {
  console.error("Unable to deserialize worker message:", event);
});
```

A worker can also listen for its own errors:

```javascript
self.addEventListener("error", (event) => {
  console.error("Worker execution error:", event.message);
});
```

# 🗁 Terminating a Worker

Workers consume browser resources and should be stopped when they are no longer needed.

There are two common termination methods.

**Main thread:**

```javascript
worker.terminate();
```

This immediately terminates the worker.

The worker cannot continue executing or perform cleanup after termination.

**Inside the worker:**

```javascript
self.close();
```

This allows the worker to terminate itself.

Example:

```javascript
self.addEventListener("message", (event) => {
  if (event.data === "stop") {
    self.close();
  }
});
```

# 🗁 Worker Lifecycle

A typical worker lifecycle looks like this:

```text
Create Worker
     |
     v
Worker Starts
     |
     v
Receive Message
     |
     v
Perform Background Task
     |
     v
Send Result
     |
     v
Wait for Another Message
     |
     v
Terminate Worker
```

A worker can process multiple messages before being terminated.

# 🗁 Dedicated Worker Scope

A Dedicated Worker has its own global execution scope.

Inside the worker:

```javascript
console.log(self);
```

The global object is commonly referred to through `self`.

In a worker, `self` refers to the `WorkerGlobalScope`.

Unlike the main browser window, the worker does not have access to:

* `window`.
* `document`.
* DOM elements.
* Direct DOM manipulation.

Workers can access several browser APIs, including:

* `fetch()`.
* `setTimeout()` and `setInterval()`.
* `URL`.
* `WebSocket`.
* `IndexedDB`.
* `crypto`.

Availability depends on the API and browser environment.

# 🗁 Loading Worker Scripts

A worker is created by referencing a JavaScript file.

```javascript
const worker = new Worker("./worker.js");
```

The script URL is resolved relative to the document's URL or the specified base URL.

You can also provide worker options.

```javascript
const worker = new Worker("./worker.js", {
  type: "classic",
  name: "background-worker"
});
```

Common options:

| Option | Description |
| --- | --- |
| `type` | Worker script type: `classic` or `module` |
| `name` | Optional worker name |
| `credentials` | Credentials mode for fetching module worker scripts |

# 🗁 Module Workers

Module workers allow the use of JavaScript ES modules.

Create a worker using the `module` type:

```javascript
const worker = new Worker("./worker.js", {
  type: "module"
});
```

Now the worker can use `import` and `export`.

Example:

`math.js`:

```javascript
export function add(a, b) {
  return a + b;
}
```

`worker.js`:

```javascript
import { add } from "./math.js";

self.addEventListener("message", (event) => {
  const { a, b } = event.data;

  self.postMessage(add(a, b));
});
```

This allows worker code to use modular application architecture.

# 🗁 Shared Workers

A Shared Worker can be accessed by multiple browsing contexts, such as tabs or windows, when they share the appropriate origin and worker URL.

Create a shared worker:

```javascript
const worker = new SharedWorker("./shared-worker.js");
```

Unlike Dedicated Workers, Shared Workers communicate through a `MessagePort`.

Example:

`shared-worker.js`:

```javascript
self.addEventListener("connect", (event) => {
  const port = event.ports[0];

  port.addEventListener("message", (messageEvent) => {
    console.log(messageEvent.data);

    port.postMessage("Message received");
  });

  port.start();
});
```

Main thread:

```javascript
const worker = new SharedWorker("./shared-worker.js");

worker.port.addEventListener("message", (event) => {
  console.log(event.data);
});

worker.port.start();

worker.port.postMessage("Hello shared worker");
```

The shared worker can communicate with multiple connected clients through their individual message ports.

# 🗁 Dedicated Worker vs Shared Worker

| Feature | Dedicated Worker | Shared Worker |
| --- | --- | --- |
| Ownership | One owning script | Multiple connected contexts |
| Creation | `new Worker()` | `new SharedWorker()` |
| Communication | Worker messaging | MessagePort |
| Main use case | Background task for one page | Shared background coordination |
| Lifetime | Associated with its owner and active worker references | Associated with connected clients and active references |
| Complexity | Simpler | More complex |

Shared Worker support and behavior can vary across browser environments.

# 🗁 Worker and DOM Interaction

A Web Worker cannot directly manipulate the DOM.

This is not allowed:

```javascript
document.querySelector("h1").textContent = "Hello";
```

The `document` object is unavailable inside a normal worker.

Instead, send data back to the main thread.

Worker:

```javascript
self.postMessage("Hello from background");
```

Main thread:

```javascript
worker.addEventListener("message", (event) => {
  document.querySelector("h1").textContent = event.data;
});
```

This separation keeps interface operations on the main thread.

# 🗁 Creating a Worker with Blob

For small scripts, a worker can be created dynamically using a Blob.

Example:

```javascript
const workerCode = `
  self.addEventListener("message", (event) => {
    self.postMessage(event.data * 2);
  });
`;

const blob = new Blob(
  [workerCode],
  { type: "text/javascript" }
);

const workerURL = URL.createObjectURL(blob);

const worker = new Worker(workerURL);

worker.postMessage(10);

worker.addEventListener("message", (event) => {
  console.log(event.data);
});
```

Output:

```text
20
```

When the worker is no longer needed:

```javascript
worker.terminate();

URL.revokeObjectURL(workerURL);
```

Blob-based workers can be useful for dynamically generated worker scripts, but separate worker files are generally easier to maintain.

# 🗁 Worker and Cross-Origin Restrictions

Worker scripts are subject to browser security restrictions.

Important considerations:

* Worker scripts generally must be served from the same origin as the owning document.
* Module workers follow module loading and CORS rules for imported resources.
* Cross-origin resources may require appropriate server response headers.
* Content Security Policy can restrict worker creation through the `worker-src` directive.
* Workers should be served over appropriate secure contexts when required by browser APIs.

# 🗁 Web Workers and Performance

Web Workers can improve responsiveness by moving CPU-intensive work away from the main thread.

However, workers are not automatically faster for every operation.

Consider:

* Worker startup overhead.
* Message serialization costs.
* Data copying costs.
* Memory consumption.
* Communication overhead.
* Number of worker threads.
* Complexity of coordinating tasks.

Workers are particularly useful for expensive computations that do not require direct DOM access.

For small operations, executing code on the main thread may be simpler and more efficient.

# 🗁 Web Workers vs Main Thread

| Feature | Main Thread | Web Worker |
| --- | --- | --- |
| DOM access | Yes | No |
| UI updates | Yes | No direct access |
| Background computation | Can perform it, but may block UI | Designed for parallel execution |
| Message communication | Not required for normal execution | Uses messaging |
| Global object | `window` in a browser document | `self` |
| Shared ordinary JavaScript variables | Within its execution context | Not directly shared with the main thread |

# 🗁 Practical Example: Search Processing

Imagine an application that performs expensive filtering over a large dataset.

The main thread can send search criteria to a worker.

`worker.js`:

```javascript
const products = Array.from(
  { length: 100000 },
  (_, index) => ({
    id: index + 1,
    name: `Product ${index + 1}`
  })
);

self.addEventListener("message", (event) => {
  const searchTerm = event.data.toLowerCase();

  const results = products.filter((product) =>
    product.name.toLowerCase().includes(searchTerm)
  );

  self.postMessage(results);
});
```

`main.js`:

```javascript
const worker = new Worker("./worker.js");

const searchInput = document.querySelector("#search");

searchInput.addEventListener("input", () => {
  worker.postMessage(searchInput.value);
});

worker.addEventListener("message", (event) => {
  console.log("Search results:", event.data);
});
```

The filtering operation runs inside the worker instead of blocking the main thread.

For production search interfaces, consider debouncing input and avoiding unnecessary repeated work.

# 🗁 Best Practices

* Use workers for CPU-intensive operations.
* Keep DOM manipulation on the main thread.
* Communicate through structured messages.
* Transfer large binary buffers when copying would be expensive.
* Handle worker errors explicitly.
* Terminate workers when they are no longer needed.
* Avoid creating excessive workers.
* Use module workers for modular worker code.
* Keep worker responsibilities focused.
* Consider startup and communication overhead.
* Use Shared Workers only when multiple browsing contexts genuinely need shared coordination.
* Profile application performance before introducing workers.

# 🗁 Summary

Web Workers provide background JavaScript execution through separate execution contexts.

Important concepts include:

* Main thread and worker thread separation.
* Dedicated Workers.
* Shared Workers.
* `Worker` and `SharedWorker`.
* `postMessage()` and message events.
* Structured cloning.
* Transferable objects.
* Worker error handling.
* `terminate()` and `close()`.
* Module workers.
* Blob-based workers.
* Worker performance and security considerations.

Web Workers are particularly valuable when an application needs to perform expensive computations without blocking the user interface.
