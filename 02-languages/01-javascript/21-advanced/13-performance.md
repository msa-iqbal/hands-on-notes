# JavaScript Performance

**JavaScript Performance Optimization** is the process of improving how efficiently JavaScript code executes, uses memory, processes data, and interacts with browser rendering.

Performance optimization aims to make applications:

* Faster to execute.
* More responsive to user interactions.
* More efficient in memory usage.
* Faster to load.
* More scalable.
* More reliable under heavy workloads.

JavaScript performance matters in:

* Frontend web applications.
* Backend applications using Node.js.
* Real-time applications.
* Data-processing systems.
* Browser-based games.
* Large-scale web platforms.
* Interactive user interfaces.

# 🗁 Understanding JavaScript Performance

JavaScript performance depends on several factors:

| Factor | Description |
| --- | --- |
| Execution time | How long JavaScript takes to execute |
| Memory usage | How much memory the application consumes |
| CPU utilization | How much processing power JavaScript requires |
| Network requests | How much time is spent waiting for network resources |
| Rendering performance | How efficiently the browser updates the interface |
| Event handling | How quickly user interactions are processed |
| Startup time | How long the application takes to become usable |
| Garbage collection | How efficiently unused memory is reclaimed |

Optimizing one factor does not necessarily improve all the others.

For example, reducing JavaScript execution time may not improve page loading if the application is primarily waiting for network requests.

# 🗁 JavaScript Execution Performance

JavaScript executes instructions through the JavaScript engine.

Popular JavaScript engines include:

* V8 — Chrome, Chromium, and Node.js.
* SpiderMonkey — Firefox.
* JavaScriptCore — Safari.

Modern engines use techniques such as:

* Just-in-time (JIT) compilation.
* Bytecode interpretation.
* Runtime optimization.
* Inline caching.
* Hidden classes or shapes.
* Garbage collection.

These optimizations improve execution performance without requiring developers to manage machine-level compilation directly.

# 🗁 Avoid Unnecessary Computation

Repeated calculations can waste CPU resources.

Example of inefficient code:

```javascript
const numbers = [10, 20, 30, 40, 50];

for (let i = 0; i < numbers.length; i++) {
    const totalItems = numbers.length;

    console.log(
        numbers[i],
        totalItems
    );
}
```

A clearer approach:

```javascript
const numbers = [10, 20, 30, 40, 50];
const totalItems = numbers.length;

for (let i = 0; i < totalItems; i++) {
    console.log(
        numbers[i],
        totalItems
    );
}
```

Modern JavaScript engines may optimize simple repeated property accesses, so this example is mainly about avoiding unnecessary work and improving clarity rather than guaranteeing a measurable speed improvement.

# 🗁 Choose Efficient Algorithms

Algorithm selection has a significant impact on performance.

Consider searching for an item in an array.

Linear search:

```javascript
function findUser(users, id) {
    return users.find(user => user.id === id);
}
```

Its worst-case time complexity is O(n).

For repeated lookups by ID, a `Map` may be more appropriate:

```javascript
const users = [
    { id: 1, name: "Alice" },
    { id: 2, name: "Bob" },
    { id: 3, name: "Charlie" }
];

const usersById = new Map(
    users.map(user => [user.id, user])
);

console.log(usersById.get(2));
```

A `Map` generally provides average sublinear access time, commonly expected O(1) for typical hash-based implementations.

The trade-off is additional memory and the cost of constructing the map.

# 🗁 Understand Time Complexity

Time complexity describes how an algorithm's execution cost grows as its input size increases.

Common complexities:

| Complexity | Name | Example |
| --- | --- | --- |
| O(1) | Constant | Accessing an array element by index |
| O(log n) | Logarithmic | Binary search on sorted data |
| O(n) | Linear | Iterating through an array |
| O(n log n) | Linearithmic | Efficient comparison-based sorting |
| O(n²) | Quadratic | Comparing every pair of elements |
| O(2ⁿ) | Exponential | Some recursive combinatorial algorithms |

Example of quadratic complexity:

```javascript
function findDuplicates(numbers) {
    const duplicates = [];

    for (let i = 0; i < numbers.length; i++) {
        for (let j = i + 1; j < numbers.length; j++) {
            if (numbers[i] === numbers[j]) {
                duplicates.push(numbers[i]);
            }
        }
    }

    return duplicates;
}
```

For large arrays, the nested loops can become expensive.

An alternative using a `Set`:

```javascript
function findDuplicates(numbers) {
    const seen = new Set();
    const duplicates = new Set();

    for (const number of numbers) {
        if (seen.has(number)) {
            duplicates.add(number);
        } else {
            seen.add(number);
        }
    }

    return [...duplicates];
}
```

This approach has expected O(n) time complexity and uses additional memory proportional to the number of unique values.

# 🗁 Optimize Array Operations

Array methods make code expressive, but unnecessary intermediate arrays can increase memory allocation and processing.

Example:

```javascript
const numbers = [1, 2, 3, 4, 5, 6];

const result = numbers
    .filter(number => number % 2 === 0)
    .map(number => number * 2);

console.log(result);
// [4, 8, 12]
```

This creates an intermediate array after `filter()`.

A single loop avoids that intermediate array:

```javascript
const numbers = [1, 2, 3, 4, 5, 6];
const result = [];

for (const number of numbers) {
    if (number % 2 === 0) {
        result.push(number * 2);
    }
}

console.log(result);
// [4, 8, 12]
```

The single-loop version can reduce allocations, especially when processing large datasets.

However, readability should remain a priority. Built-in methods are often sufficiently optimized by modern JavaScript engines.

# 🗁 Avoid Unnecessary DOM Operations

DOM operations can be expensive when they trigger repeated layout calculations or rendering updates.

Inefficient approach:

```javascript
const list = document.querySelector("#list");

for (let i = 0; i < 100; i++) {
    const item = document.createElement("li");

    item.textContent = `Item ${i}`;

    list.appendChild(item);
}
```

A more efficient approach can build the elements in a `DocumentFragment`:

```javascript
const list = document.querySelector("#list");
const fragment = document.createDocumentFragment();

for (let i = 0; i < 100; i++) {
    const item = document.createElement("li");

    item.textContent = `Item ${i}`;

    fragment.appendChild(item);
}

list.appendChild(fragment);
```

The fragment allows the elements to be assembled before they are inserted into the document.

Modern browsers also optimize many DOM operations automatically, so measure before assuming a fragment will improve every workload.

# 🗁 Minimize Layout Thrashing

Layout thrashing happens when JavaScript repeatedly alternates between changing the DOM and reading layout-dependent properties.

Example of potentially inefficient code:

```javascript
const elements = document.querySelectorAll(".box");

for (const element of elements) {
    element.style.width = "200px";

    console.log(element.offsetWidth);
}
```

Reading `offsetWidth` after a style change may force the browser to calculate layout repeatedly.

A better approach separates writes and reads:

```javascript
const elements = document.querySelectorAll(".box");

for (const element of elements) {
    element.style.width = "200px";
}

const widths = [];

for (const element of elements) {
    widths.push(element.offsetWidth);
}
```

This reduces unnecessary alternation between layout-affecting writes and layout reads.

# 🗁 Debouncing

**Debouncing** delays function execution until a specified period has passed without another invocation.

It is useful for:

* Search input.
* Form validation.
* Window resizing.
* Autosave operations.
* Expensive filtering.

Example:

```javascript
function debounce(callback, delay = 300) {
    let timeoutId;

    return function (...args) {
        clearTimeout(timeoutId);

        timeoutId = setTimeout(() => {
            callback.apply(this, args);
        }, delay);
    };
}

const search = debounce((query) => {
    console.log("Searching:", query);
}, 500);

search("Java");
search("JavaScript");
```

Only the final invocation runs if no new call occurs during the delay period.

# 🗁 Throttling

**Throttling** limits how frequently a function can execute.

It is useful for:

* Scroll events.
* Mouse movement.
* Window resizing.
* Continuous user interactions.

Example:

```javascript
function throttle(callback, delay = 300) {
    let lastExecution = 0;

    return function (...args) {
        const now = Date.now();

        if (now - lastExecution >= delay) {
            lastExecution = now;

            return callback.apply(this, args);
        }
    };
}

const handleScroll = throttle(() => {
    console.log("Scroll event processed");
}, 500);

window.addEventListener("scroll", handleScroll);
```

The callback executes at most once per delay interval, based on the last successful execution.

# 🗁 Use RequestAnimationFrame

`requestAnimationFrame()` schedules a callback before the browser's next repaint.

It is suitable for visual animations and frame-based updates.

Example:

```javascript
const box = document.querySelector(".box");

let position = 0;

function animate() {
    position += 2;

    box.style.transform = `translateX(${position}px)`;

    if (position < 300) {
        requestAnimationFrame(animate);
    }
}

requestAnimationFrame(animate);
```

The browser controls the animation timing, helping visual updates synchronize with rendering.

For animations, CSS transitions and animations may be preferable when JavaScript does not need to control each frame.

# 🗁 Optimize Event Handling

Attaching many individual event listeners can increase memory usage and management complexity.

Event delegation allows a parent element to handle events from its descendants.

Example:

```javascript
const list = document.querySelector("#list");

list.addEventListener("click", (event) => {
    const item = event.target.closest("li");

    if (!item || !list.contains(item)) {
        return;
    }

    console.log(item.textContent);
});
```

This is particularly useful for dynamically generated elements.

# 🗁 Avoid Memory Leaks

A memory leak occurs when objects that are no longer needed remain reachable and cannot be garbage-collected.

Common causes include:

* Unremoved event listeners.
* Uncontrolled global variables.
* Long-lived closures retaining unnecessary data.
* Unreleased timers.
* Detached DOM nodes still referenced by JavaScript.
* Unbounded caches.
* Subscriptions that are never cleaned up.

Example of a potential memory leak:

```javascript
const button = document.querySelector("#button");

function handleClick() {
    console.log("Clicked");
}

button.addEventListener("click", handleClick);
```

If the component or interface is removed, the listener should be cleaned up when appropriate.

Example:

```javascript
const button = document.querySelector("#button");

function handleClick() {
    console.log("Clicked");
}

button.addEventListener("click", handleClick);

// Later, when the listener is no longer needed:
button.removeEventListener("click", handleClick);
```

# 🗁 Use WeakMap and WeakSet

`WeakMap` and `WeakSet` hold weak references to objects.

They are useful when associating metadata with objects without making those objects stay reachable solely because of the collection.

Example:

```javascript
const metadata = new WeakMap();

function attachMetadata(object, data) {
    metadata.set(object, data);
}

const user = {
    name: "Alice"
};

attachMetadata(user, {
    role: "admin"
});

console.log(metadata.get(user));
```

When the object becomes otherwise unreachable, its entry can be garbage-collected.

Weak collections are not enumerable and cannot be used like ordinary `Map` or `Set` collections.

# 🗁 Garbage Collection

JavaScript engines automatically manage memory through garbage collection.

The most common conceptual model is reachability-based garbage collection.

An object can be collected when it is no longer reachable from active program roots.

Example:

```javascript
let user = {
    name: "Alice"
};

user = null;
```

If no other references point to the original object, it becomes eligible for garbage collection.

Developers cannot reliably force immediate garbage collection in ordinary JavaScript application code.

# 🗁 Optimize Object Creation

Creating many temporary objects can increase memory allocation and garbage-collection work.

Example:

```javascript
function createUsers(names) {
    return names.map(name => ({
        name,
        active: true
    }));
}
```

For ordinary application code, this is a clear and appropriate implementation.

In performance-critical systems, consider:

* Reusing objects when appropriate.
* Avoiding unnecessary intermediate allocations.
* Processing large datasets in chunks.
* Using typed arrays for numeric workloads.
* Avoiding unnecessarily large object graphs.

Do not sacrifice maintainability for speculative allocation optimizations.

# 🗁 Optimize String Operations

Repeated string concatenation can create intermediate strings depending on the engine and workload.

Example:

```javascript
let result = "";

for (let i = 0; i < 1000; i++) {
    result += `Item ${i}\n`;
}
```

An alternative is collecting strings in an array:

```javascript
const parts = [];

for (let i = 0; i < 1000; i++) {
    parts.push(`Item ${i}`);
}

const result = parts.join("\n");
```

Benchmark both approaches for the actual workload. Modern engines can optimize string concatenation, so neither approach is universally faster.

# 🗁 Lazy Loading

Lazy loading delays loading resources until they are needed.

Dynamic imports support JavaScript code splitting.

Example:

```javascript
async function loadDashboard() {
    const module = await import("./dashboard.js");

    module.renderDashboard();
}
```

Lazy loading can reduce the amount of JavaScript required during the initial application load.

It is particularly useful for:

* Large applications.
* Admin dashboards.
* Route-based applications.
* Optional features.
* Heavy libraries.

# 🗁 Code Splitting

Code splitting divides application code into smaller bundles.

Instead of loading an entire application immediately, the browser can load only the code required for the current page or feature.

Example:

```javascript
const button = document.querySelector("#load-button");

button.addEventListener("click", async () => {
    const module = await import("./feature.js");

    module.runFeature();
});
```

Bundlers such as Vite, webpack, and Rollup can use dynamic imports as code-splitting boundaries.

# 🗁 Minification and Compression

Minification removes unnecessary characters from source code.

Example:

Readable JavaScript:

```javascript
function calculateTotal(price, quantity) {
    return price * quantity;
}
```

Minified JavaScript:

```javascript
function calculateTotal(e,t){return e*t}
```

Minification can reduce transfer size.

Compression algorithms such as Brotli and gzip reduce network payload sizes further.

These techniques are commonly handled by build tools and web servers.

# 🗁 Use Web Workers

Web Workers execute JavaScript in a background thread separate from the main browser thread.

They are useful for CPU-intensive operations that would otherwise block user-interface interactions.

Main JavaScript file:

```javascript
const worker = new Worker("./worker.js");

worker.postMessage(1000000);

worker.onmessage = (event) => {
    console.log("Result:", event.data);
};
```

Worker file:

```javascript
self.onmessage = (event) => {
    const limit = event.data;

    let total = 0;

    for (let i = 0; i < limit; i++) {
        total += i;
    }

    self.postMessage(total);
};
```

Web Workers cannot directly manipulate the DOM.

They communicate with the main thread using messages.

# 🗁 Use Typed Arrays

Typed arrays provide efficient storage and manipulation of binary data and numeric values.

Common typed arrays include:

* `Int8Array`
* `Uint8Array`
* `Uint8ClampedArray`
* `Int16Array`
* `Uint16Array`
* `Int32Array`
* `Uint32Array`
* `Float32Array`
* `Float64Array`
* `BigInt64Array`
* `BigUint64Array`

Example:

```javascript
const numbers = new Float32Array(3);

numbers[0] = 10.5;
numbers[1] = 20.5;
numbers[2] = 30.5;

console.log(numbers);
```

Typed arrays are useful for:

* Graphics processing.
* Audio processing.
* Binary protocols.
* Image manipulation.
* Scientific calculations.
* Large numeric datasets.

# 🗁 Optimize Network Requests

Network requests often contribute significantly to application response time.

Useful techniques include:

* Reducing unnecessary requests.
* Using caching.
* Compressing responses.
* Implementing pagination.
* Using request cancellation.
* Avoiding duplicate requests.
* Loading resources only when needed.
* Using appropriate API response sizes.

Example using `AbortController`:

```javascript
const controller = new AbortController();

async function loadUsers() {
    try {
        const response = await fetch("/api/users", {
            signal: controller.signal
        });

        if (!response.ok) {
            throw new Error(
                `HTTP error: ${response.status}`
            );
        }

        return await response.json();
    } catch (error) {
        if (error.name === "AbortError") {
            console.log("Request cancelled");
            return;
        }

        throw error;
    }
}

// Cancel the request when it is no longer needed:
controller.abort();
```

Request cancellation is especially useful when users navigate away from a page or start a newer search before an earlier request finishes.

# 🗁 Caching Expensive Calculations

Caching stores previously calculated results so they can be reused.

Example:

```javascript
const cache = new Map();

function calculateSquare(number) {
    if (cache.has(number)) {
        return cache.get(number);
    }

    const result = number * number;

    cache.set(number, result);

    return result;
}

console.log(calculateSquare(10));
// 100

console.log(calculateSquare(10));
// 100
```

Caching is useful when:

* Calculations are expensive.
* The same inputs occur repeatedly.
* Cached results remain valid.
* Memory usage is controlled.

A production cache should consider expiration, invalidation, maximum size, and data consistency.

# 🗁 Memoization

Memoization is a caching technique that stores function results based on their inputs.

Example:

```javascript
function memoize(callback) {
    const cache = new Map();

    return function (...args) {
        const key = JSON.stringify(args);

        if (cache.has(key)) {
            return cache.get(key);
        }

        const result = callback.apply(this, args);

        cache.set(key, result);

        return result;
    };
}

function square(number) {
    console.log("Calculating...");

    return number * number;
}

const memoizedSquare = memoize(square);

console.log(memoizedSquare(5));
// Calculating...
// 25

console.log(memoizedSquare(5));
// 25
```

This simple implementation is suitable only for inputs that can be represented safely and consistently using JSON serialization.

More advanced memoization systems may need custom key generation, bounded caches, and support for asynchronous results.

# 🗁 Measure Performance with performance.now()

The Performance API provides high-resolution timing information.

Example:

```javascript
const start = performance.now();

let total = 0;

for (let i = 0; i < 1000000; i++) {
    total += i;
}

const end = performance.now();

console.log(`Execution time: ${end - start} ms`);
```

`performance.now()` is generally more suitable than `Date.now()` for measuring short execution intervals.

# 🗁 Measure Performance with performance.mark()

The Performance API also supports named performance marks and measurements.

Example:

```javascript
performance.mark("calculation-start");

let total = 0;

for (let i = 0; i < 1000000; i++) {
    total += i;
}

performance.mark("calculation-end");

performance.measure(
    "calculation",
    "calculation-start",
    "calculation-end"
);

const measurements = performance.getEntriesByName(
    "calculation"
);

console.log(measurements[0].duration);
```

This approach is useful for measuring specific application operations.

# 🗁 Browser Performance Tools

Modern browsers provide tools for analyzing JavaScript performance.

**Chrome DevTools**

Useful features include:

* Performance panel.
* Memory panel.
* CPU profiling.
* Heap snapshots.
* Allocation instrumentation.
* Network panel.
* Coverage panel.
* Lighthouse integration.

**Firefox Developer Tools**

Useful features include:

* Performance profiler.
* Memory analysis.
* Network monitoring.
* JavaScript debugging.

**Safari Web Inspector**

Useful features include:

* Timelines.
* JavaScript profiling.
* Memory inspection.
* Network analysis.

# 🗁 Benchmarking JavaScript

Benchmarking measures the performance of different implementations.

Example:

```javascript
function benchmark(name, callback, iterations = 100000) {
    const start = performance.now();

    for (let i = 0; i < iterations; i++) {
        callback();
    }

    const end = performance.now();

    console.log(
        `${name}: ${(end - start).toFixed(3)} ms`
    );
}

benchmark("Array length", () => {
    const numbers = [1, 2, 3, 4, 5];

    return numbers.length;
});
```

For reliable benchmarking:

* Use realistic input data.
* Run multiple iterations.
* Warm up the code when appropriate.
* Avoid measuring unrelated operations.
* Test in representative environments.
* Account for browser and system differences.
* Compare results statistically when precision matters.

A single timing result is not sufficient to establish that one implementation is consistently faster.

# 🗁 Common Performance Mistakes

* Optimizing code without measuring it.
* Using inefficient algorithms for large datasets.
* Creating unnecessary intermediate arrays.
* Performing repeated DOM updates.
* Causing layout thrashing.
* Attaching excessive event listeners.
* Ignoring memory leaks.
* Loading large dependencies unnecessarily.
* Blocking the main thread with expensive tasks.
* Making unnecessary network requests.
* Creating unbounded caches.
* Using complex abstractions for simple operations.

# 🗁 Performance Optimization Checklist

* [ ] Use appropriate algorithms and data structures.
* [ ] Avoid unnecessary computations.
* [ ] Reduce unnecessary memory allocations.
* [ ] Avoid repeated DOM updates.
* [ ] Prevent layout thrashing.
* [ ] Use debouncing and throttling where appropriate.
* [ ] Use `requestAnimationFrame()` for frame-based visual updates.
* [ ] Clean up event listeners and timers.
* [ ] Use caching for expensive repeated operations.
* [ ] Apply lazy loading and code splitting.
* [ ] Use Web Workers for suitable CPU-intensive tasks.
* [ ] Optimize network requests and caching.
* [ ] Measure execution using the Performance API.
* [ ] Profile real application workloads.
* [ ] Recheck performance after optimization.

# 🗁 Key Takeaways

* JavaScript performance depends on execution time, memory, rendering, and network activity.
* Algorithm selection often matters more than small syntax-level optimizations.
* Efficient data structures can significantly improve repeated operations.
* DOM updates should be organized to reduce unnecessary layout work.
* Debouncing and throttling help control frequently triggered operations.
* Garbage collection manages memory automatically, but developers must prevent unnecessary references.
* Lazy loading and code splitting reduce initial loading costs.
* Web Workers move suitable CPU-intensive work away from the main thread.
* The Performance API and browser developer tools help identify bottlenecks.
* Always measure before optimizing and verify improvements afterward.

# 🗁 Related Topics

* JavaScript Execution Context
* Call Stack
* Event Loop
* Memory Management
* Garbage Collection
* Arrays and Collections
* Map and Set
* WeakMap and WeakSet
* Asynchronous JavaScript
* Web Workers
* Browser Rendering
* DOM Optimization
* Performance API
* Algorithm Complexity
* Code Splitting
* Lazy Loading
