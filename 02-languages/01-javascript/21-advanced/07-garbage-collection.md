# JavaScript Garbage Collection

**Garbage Collection (GC)** is the automatic memory reclamation process used by JavaScript engines to identify and release memory occupied by objects that are no longer reachable.

JavaScript uses automatic memory management, so developers generally do not need to manually allocate and free memory.

However, understanding garbage collection is important for:

* Preventing memory leaks
* Optimizing application performance
* Understanding object references
* Managing closures
* Working with large datasets
* Building long-running applications
* Understanding JavaScript engine behavior

# 🗁 What Is Garbage Collection?

Garbage collection identifies objects that are no longer accessible to an application and reclaims their memory.

Consider:

```javascript
let user = {
    name: "Alice",
    age: 25
};

user = null;
```

Initially, `user` references an object.

After assigning `null`, the variable no longer references that object.

If no other reachable references exist, the object becomes eligible for garbage collection.

The garbage collector may reclaim its memory during a later collection cycle.

# 🗁 Why Does JavaScript Need Garbage Collection?

Applications continuously create values and objects during execution.

Without memory reclamation, unused objects could accumulate and consume available memory.

Example:

```javascript
function createUser() {
    const user = {
        name: "Alice",
        age: 25
    };

    return user;
}

let currentUser = createUser();

currentUser = null;
```

After `currentUser` is assigned `null`, the previously returned object may become unreachable.

Garbage collection allows the runtime to reclaim memory when appropriate.

# 🗁 Reachability

**Reachability** is the primary concept behind modern JavaScript garbage collection.

An object is considered reachable if it can be accessed through references starting from garbage collection roots.

Common roots include:

* Global references
* Active execution contexts
* References maintained by runtime internals
* Other engine-managed references

Example:

```javascript
const user = {
    name: "Alice"
};
```

The object is reachable through the global binding `user`.

Now consider:

```javascript
let user = {
    name: "Alice"
};

user = null;
```

If no other reachable references exist, the object becomes unreachable.

# 🗁 Garbage Collection Roots

**Garbage Collection Roots** are starting points used by the garbage collector to determine which objects remain reachable.

Conceptual examples include:

* Global objects and their reachable properties
* Active function execution state
* References maintained by the runtime
* Other engine-specific root references

Example:

```javascript
const application = {
    user: {
        name: "Alice"
    }
};
```

Conceptual reference graph:

```text
Garbage Collection Root
          |
          v
    application
          |
          v
       User
          |
          v
    name: "Alice"
```

Both objects are reachable from the root.

If the application reference is removed and no other reachable references exist, the associated objects may become eligible for collection.

# 🗁 Mark-and-Sweep Algorithm

**Mark-and-Sweep** is a common tracing garbage collection approach.

It identifies reachable objects and reclaims memory associated with unreachable objects.

It consists of two main stages:

* Mark Phase
* Sweep Phase

### 🖆 Mark Phase

The garbage collector starts from its roots and traverses reachable objects.

Objects discovered during traversal are marked as reachable.

Example:

```javascript
const first = {
    name: "First"
};

const second = {
    name: "Second"
};

first.next = second;
```

Both objects are reachable through the global binding `first`.

### 🖆 Sweep Phase

The garbage collector identifies objects that were not marked as reachable and reclaims their memory.

Conceptual diagram:

```text
        Garbage Collection Roots
                   |
                   v
              First Object
                   |
                   v
             Second Object

          Both objects are marked.

                   |

                   v

           Unreachable Objects
                   |
                   v
            Memory Reclaimed
```

The actual implementation may use additional phases and optimizations.

# 🗁 Reference Counting

**Reference Counting** is a memory management technique that tracks how many references point to an object.

Conceptually:

* Creating a reference increases the reference count.
* Removing a reference decreases the reference count.
* An object with no remaining references may be reclaimed.

Example:

```javascript
let first = {
    name: "Alice"
};

let second = first;

first = null;
```

The object remains referenced by `second`.

Therefore, removing the `first` reference does not make the object unreachable.

However, reference counting alone has a limitation: circular references.

# 🗁 Circular References

A **Circular Reference** occurs when objects reference one another in a cycle.

Example:

```javascript
let first = {};
let second = {};

first.next = second;
second.previous = first;
```

Conceptual structure:

```text
     ┌──────────────┐
     │ First Object │
     └──────────────┘
             |
             v
     ┌──────────────┐
     │ Second Object│
     └──────────────┘
             |
             └───────────────┐
                             |
                             v
                       First Object
```

Now remove the external references:

```javascript
first = null;
second = null;
```

The objects still reference each other, but they are no longer reachable from the roots.

A tracing garbage collector can identify this unreachable cycle and reclaim both objects.

This is one reason modern JavaScript garbage collectors rely primarily on reachability rather than simple reference counting.

# 🗁 Generational Garbage Collection

Many JavaScript engines use generational garbage collection strategies.

The basic idea is that newly created objects often have short lifetimes, while some objects remain in use for a long time.

Memory is conceptually divided into generations.

Common generations include:

* Young Generation
* Old Generation

### 🖆 Young Generation

The young generation contains newly allocated objects.

Many objects become unreachable shortly after creation, making this generation suitable for frequent collection.

Example:

```javascript
function processRequest() {
    const temporaryData = {
        requestId: 1001,
        status: "Processing"
    };

    return temporaryData.status;
}

processRequest();
```

The temporary object may become unreachable after the function completes if no reference escapes.

### 🖆 Old Generation

Objects that survive multiple collection cycles may be promoted to an older generation.

These objects are generally considered longer-lived.

Example:

```javascript
const applicationConfig = {
    theme: "dark",
    language: "en",
    version: "1.0"
};
```

A long-lived configuration object may remain reachable throughout the application's lifetime.

The exact generation structure and promotion rules depend on the JavaScript engine.

# 🗁 Minor and Major Garbage Collection

Generational collectors commonly perform different kinds of collection.

### 🖆 Minor Garbage Collection

A **Minor GC** primarily collects objects in the young generation.

Characteristics:

* Usually focuses on short-lived objects.
* Typically runs more frequently.
* Is designed to reclaim memory efficiently.
* May promote surviving objects to an older generation.

### 🖆 Major Garbage Collection

A **Major GC** generally involves collecting objects in the old generation or performing broader heap reclamation.

Characteristics:

* May process a larger portion of the managed heap.
* Can require more work than a minor collection.
* May involve significant application pauses, depending on the collector.
* Uses optimization techniques to reduce execution disruption.

These names and their exact meanings can vary between engines.

# 🗁 Incremental Garbage Collection

**Incremental Garbage Collection** divides garbage collection work into smaller steps instead of performing all work in one uninterrupted operation.

This approach can reduce long pauses.

Conceptual process:

```text
Garbage Collection
        |
        v
    Step One
        |
        v
    Step Two
        |
        v
    Step Three
        |
        v
    Step Four
        |
        v
   Collection Complete
```

The runtime can schedule collection work across multiple execution intervals.

# 🗁 Concurrent Garbage Collection

**Concurrent Garbage Collection** performs some garbage collection work simultaneously with application execution.

This can reduce the amount of time the application thread must spend performing garbage collection.

Modern JavaScript engines use different combinations of concurrent and parallel techniques depending on the collection phase.

# 🗁 Parallel Garbage Collection

**Parallel Garbage Collection** uses multiple worker threads to perform garbage collection tasks.

Instead of relying on one thread for all collection work, the engine can distribute suitable tasks across multiple threads.

This can improve collection performance on systems with multiple CPU cores.

Incremental, concurrent, and parallel collection are different techniques and may be combined in a garbage collector.

# 🗁 Memory Leaks and Garbage Collection

A **Memory Leak** occurs when an application retains references to objects that are no longer needed.

Garbage collection cannot reclaim objects that remain reachable.

Example:

```javascript
const users = [];

function addUser(name) {
    users.push({
        name: name
    });
}

addUser("Alice");
addUser("Bob");
addUser("Charlie");
```

The `users` array continues retaining all inserted objects.

If the application keeps adding users without removing unnecessary entries, memory usage can grow continuously.

A better approach is to manage the collection's lifecycle.

```javascript
const users = [];

function addUser(name) {
    users.push({
        name: name
    });

    if (users.length > 100) {
        users.shift();
    }
}
```

# 🗁 Event Listener Memory Leaks

Event listeners can retain callback functions and objects referenced by their closures.

Example:

```javascript
function initialize() {
    const largeData = new Array(1000000).fill("Data");

    function handleClick() {
        console.log(largeData.length);
    }

    document.addEventListener("click", handleClick);
}

initialize();
```

The document retains the listener, and the listener retains access to `largeData`.

A cleanup function can remove the listener when it is no longer required.

```javascript
function initialize() {
    const largeData = new Array(1000000).fill("Data");

    function handleClick() {
        console.log(largeData.length);
    }

    document.addEventListener("click", handleClick);

    return function cleanup() {
        document.removeEventListener("click", handleClick);
    };
}

const cleanup = initialize();

cleanup();
```

# 🗁 Closure and Garbage Collection

Closures preserve access to variables from their surrounding lexical environments.

An object referenced by a closure remains reachable as long as the closure retains that reference and the closure itself remains reachable.

Example:

```javascript
function createCounter() {
    let count = 0;

    return function increment() {
        count++;

        return count;
    };
}

const counter = createCounter();

console.log(counter()); // 1
console.log(counter()); // 2
```

The `count` binding remains accessible through the returned function.

When the closure becomes unreachable, its retained environment may become eligible for collection if no other references exist.

Closures do not automatically cause memory leaks.

# 🗁 WeakMap and WeakSet

JavaScript provides weak collections that do not keep objects alive solely through their membership or use as keys.

### 🖆 WeakMap

A `WeakMap` stores key-value pairs where keys must be objects or non-registered symbols.

Example:

```javascript
const metadata = new WeakMap();

let user = {
    name: "Alice"
};

metadata.set(user, {
    role: "Developer"
});

console.log(metadata.get(user));
```

If `user` becomes otherwise unreachable, the WeakMap entry can be removed during garbage collection.

### 🖆 WeakSet

A `WeakSet` stores weakly held object references.

Example:

```javascript
const processed = new WeakSet();

let user = {
    name: "Alice"
};

processed.add(user);

console.log(processed.has(user)); // true
```

If the object becomes otherwise unreachable, it can be collected even though it was previously added to the WeakSet.

Weak collections are not enumerable because their contents can change as a consequence of garbage collection.

# 🗁 FinalizationRegistry

`FinalizationRegistry` allows developers to register cleanup callbacks that may run after an object becomes eligible for garbage collection and is collected.

Example:

```javascript
const registry = new FinalizationRegistry((value) => {
    console.log(`Object finalized: ${value}`);
});

let user = {
    name: "Alice"
};

registry.register(user, "Alice");

user = null;
```

The callback may run after the object is collected.

Important characteristics:

* Execution timing is nondeterministic.
* The callback may never execute before the application terminates.
* The callback should not be used for essential resource cleanup.
* It should not be treated as a deterministic memory management mechanism.

For resources such as file handles, sockets, and event listeners, explicit cleanup is preferable.

# 🗁 Manually Triggering Garbage Collection

Ordinary JavaScript code does not provide a standard function for forcing garbage collection.

Some development environments expose garbage collection controls for debugging.

For example, Node.js can expose an explicit garbage collection function when started with:

```bash
node --expose-gc app.js
```

Then:

```javascript
global.gc();
```

This is primarily useful for controlled testing and diagnostics.

Production application logic should not depend on manually triggering garbage collection.

# 🗁 Monitoring Garbage Collection

Developers can investigate garbage collection behavior using runtime profiling tools.

### 🖆 Browser Developer Tools

In Chromium-based browsers:

* Open Developer Tools.
* Navigate to the Memory panel.
* Capture a heap snapshot.
* Inspect retained objects.
* Compare snapshots after repeated operations.
* Use allocation profiling to identify memory growth.

### 🖆 Node.js Memory Monitoring

Node.js provides the `process.memoryUsage()` method.

```javascript
console.log(process.memoryUsage());
```

Example output:

```javascript
{
    rss: 50000000,
    heapTotal: 10000000,
    heapUsed: 6000000,
    external: 2000000,
    arrayBuffers: 1000000
}
```

These values are illustrative.

Useful properties:

* `rss`: Total resident memory used by the process.
* `heapTotal`: Total heap memory allocated by V8.
* `heapUsed`: Heap memory currently used by V8.
* `external`: Memory associated with native objects outside the V8 heap.
* `arrayBuffers`: Memory associated with `ArrayBuffer` and related objects.

# 🗁 Garbage Collection Best Practices

* Avoid retaining unnecessary object references.
* Remove event listeners when they are no longer needed.
* Clear timers and intervals during cleanup.
* Limit the size of long-lived caches.
* Avoid continuously growing arrays and collections.
* Understand how closures retain their referenced data.
* Use WeakMap and WeakSet for appropriate weak-reference use cases.
* Use explicit resource cleanup for files, sockets, and other external resources.
* Monitor memory usage in long-running applications.
* Use heap snapshots to investigate suspected memory leaks.
* Do not depend on garbage collection occurring at a specific time.
* Avoid using `FinalizationRegistry` for essential cleanup operations.

# 🗁 Common Misconceptions

**JavaScript immediately frees an object when its variable is set to `null`.**

Incorrect. The object becomes eligible for collection only if no other reachable references remain. Collection timing is nondeterministic.

**Garbage collection removes every unused variable immediately.**

Incorrect. Garbage collection is primarily concerned with reclaiming unreachable managed objects. It is not a guaranteed immediate cleanup mechanism for every variable or resource.

**Circular references always cause memory leaks.**

Incorrect. Tracing garbage collectors can identify unreachable cycles and reclaim them.

**WeakMap immediately deletes entries when an object is no longer referenced.**

Incorrect. Entries can be removed during garbage collection, but the exact timing is nondeterministic.

**Developers can control exactly when garbage collection happens.**

Incorrect. JavaScript engines manage collection scheduling. Development tools may expose diagnostic controls, but normal application code cannot rely on deterministic collection timing.

**Garbage collection releases every type of resource.**

Incorrect. External resources such as file handles, sockets, and event listeners often require explicit cleanup.

# 🗁 Key Takeaways

* Garbage collection automatically reclaims memory occupied by unreachable objects.
* Reachability is the central concept behind modern JavaScript garbage collection.
* Garbage collection roots provide starting points for reachability analysis.
* Mark-and-Sweep is a common tracing garbage collection approach.
* Reference counting alone cannot correctly handle unreachable circular references.
* Generational collection separates objects based on their lifetimes.
* Incremental, concurrent, and parallel techniques help improve collection performance.
* Memory leaks occur when unnecessary objects remain reachable.
* Closures and event listeners can retain objects.
* WeakMap and WeakSet provide weak-reference collection semantics.
* FinalizationRegistry provides nondeterministic cleanup notifications.
* Explicit cleanup is necessary for many external resources.

# 🗁 Related Topics

* Memory Management
* Execution Context
* Call Stack
* Lexical Environment
* Closures
* Objects
* Arrays
* Map
* Set
* WeakMap
* WeakSet
* FinalizationRegistry
* Performance Optimization
