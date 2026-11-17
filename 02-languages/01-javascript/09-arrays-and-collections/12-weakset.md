# JavaScript `WeakSet`

A JavaScript `WeakSet` is a built-in collection that stores unique object references using weak references.

Unlike a regular `Set`, a WeakSet does not prevent its stored objects from being garbage-collected when they become otherwise unreachable.

WeakSet is useful when you need to track objects without extending their lifetime unnecessarily.

Common use cases include:

* Tracking objects that have already been processed.
* Preventing duplicate processing of object references.
* Tracking DOM elements.
* Managing object-related state.
* Recording whether an object has been initialized.
* Avoiding memory retention caused by strong references.

# 🗁 Creating a WeakSet

### 🖆 WeakSet Constructor

Create an empty WeakSet using the `WeakSet` constructor.

```javascript
const processedObjects = new WeakSet();

console.log(processedObjects);
// WeakSet { <items unknown> }
```

The exact console representation depends on the JavaScript environment.

### 🖆 Creating a WeakSet with Initial Values

A WeakSet can be initialized with an iterable containing objects or supported symbols.

```javascript
const userA = { id: 101 };
const userB = { id: 102 };

const users = new WeakSet([userA, userB]);

console.log(users.has(userA));
// true
```

Primitive values cannot be added to a WeakSet.

# 🗁 WeakSet Value Requirements

A WeakSet stores:

* Objects.
* Functions, because functions are objects.
* Non-registered symbols.

It does not accept primitive values such as:

* Strings.
* Numbers.
* Booleans.
* `null`.
* `undefined`.
* BigInts.

### 🖆 Storing Objects

```javascript
const users = new WeakSet();

const user = {
    id: 101,
    name: "Alice"
};

users.add(user);

console.log(users.has(user));
// true
```

### 🖆 Storing Functions

Functions can also be stored in a WeakSet.

```javascript
function calculate() {
    return 100;
}

const functions = new WeakSet();

functions.add(calculate);

console.log(functions.has(calculate));
// true
```

### 🖆 Registered and Non-Registered Symbols

Modern JavaScript permits non-registered symbols as WeakSet values.

```javascript
const localSymbol = Symbol("user");

const items = new WeakSet();

items.add(localSymbol);

console.log(items.has(localSymbol));
// true
```

Symbols registered using `Symbol.for()` cannot be stored in a WeakSet.

```javascript
const globalSymbol = Symbol.for("user");

const items = new WeakSet();

items.add(globalSymbol);
// TypeError
```

# 🗁 Adding Values with `add()`

The `add()` method inserts an object or supported symbol into a WeakSet.

```javascript
const users = new WeakSet();

const userA = { id: 101 };
const userB = { id: 102 };

users.add(userA);
users.add(userB);

console.log(users.has(userA));
// true
```

### 🖆 Adding Duplicate References

WeakSet stores unique references.

Adding the same object multiple times does not create additional entries.

```javascript
const users = new WeakSet();

const user = {
    id: 101
};

users.add(user);
users.add(user);
users.add(user);

console.log(users.has(user));
// true
```

The object is still represented by one membership entry.

### 🖆 Chaining `add()`

The `add()` method returns the WeakSet itself.

```javascript
const userA = { id: 101 };
const userB = { id: 102 };

const users = new WeakSet();

users
    .add(userA)
    .add(userB);

console.log(users.has(userA));
// true
```

# 🗁 Checking Values with `has()`

The `has()` method checks whether a particular object or supported symbol exists in the WeakSet.

It returns a boolean.

```javascript
const processed = new WeakSet();

const task = {
    id: 101
};

processed.add(task);

console.log(processed.has(task));
// true

console.log(processed.has({ id: 101 }));
// false
```

WeakSet uses object identity rather than comparing object contents.

### 🖆 Checking Missing Values

```javascript
const users = new WeakSet();

const user = {
    id: 101
};

console.log(users.has(user));
// false
```

# 🗁 Removing Values with `delete()`

The `delete()` method removes an object or supported symbol from a WeakSet.

It returns:

* `true` if the value was removed.
* `false` if the value did not exist.

```javascript
const users = new WeakSet();

const user = {
    id: 101
};

users.add(user);

console.log(users.delete(user));
// true

console.log(users.has(user));
// false
```

WeakSet does not provide a `clear()` method.

If you need to replace the entire collection, assign a new WeakSet.

```javascript
let processed = new WeakSet();

const task = {
    id: 101
};

processed.add(task);

processed = new WeakSet();
```

# 🗁 WeakSet and Garbage Collection

The main characteristic of WeakSet is that its stored objects are held weakly.

A WeakSet does not independently prevent its objects from being garbage-collected.

### 🖆 Regular Set Reference

A regular Set holds strong references to its values.

```javascript
let user = {
    id: 101,
    name: "Alice"
};

const users = new Set();

users.add(user);

user = null;
```

Even after the `user` variable is set to `null`, the Set still holds a reference to the original object.

The object remains reachable through the Set.

### 🖆 WeakSet Reference

```javascript
let user = {
    id: 101,
    name: "Alice"
};

const users = new WeakSet();

users.add(user);

user = null;
```

After the external reference is removed, the object may become eligible for garbage collection.

The WeakSet does not prevent its collection.

**Important:** Garbage collection is nondeterministic. Setting a variable to `null` does not guarantee immediate collection.

# 🗁 WeakSet vs Set

Both collections store unique values, but their capabilities differ.

| Feature | Set | WeakSet |
| --- | --- | --- |
| Stores unique values | Yes | Yes |
| Supports objects | Yes | Yes |
| Supports primitive values | Yes | No |
| Supports non-registered symbols | Yes | Yes |
| Uses strong references | Yes | No |
| Supports `add()` | Yes | Yes |
| Supports `has()` | Yes | Yes |
| Supports `delete()` | Yes | Yes |
| Supports `clear()` | Yes | No |
| Supports `size` | Yes | No |
| Supports iteration | Yes | No |
| Supports `forEach()` | Yes | No |
| Suitable for object tracking | Yes | Yes |
| Prevents stored objects from being kept alive solely by the collection | No | Yes |

### 🖆 Why WeakSet Is Not Iterable

WeakSet does not provide:

* `keys()`
* `values()`
* `entries()`
* `forEach()`
* `size`
* A default iterator

This restriction is intentional.

If a WeakSet exposed its contents or size, applications could observe changes caused by garbage collection.

Since garbage collection is nondeterministic, exposing that information could produce unpredictable behavior.

WeakSet therefore provides only direct membership operations.

# 🗁 Practical WeakSet Examples

### 🖆 Tracking Processed Objects

WeakSet is useful for tracking objects that have already been processed.

```javascript
const processed = new WeakSet();

function processUser(user) {
    if (processed.has(user)) {
        console.log("User already processed");
        return;
    }

    processed.add(user);

    console.log("Processing user:", user.name);
}

const user = {
    id: 101,
    name: "Alice"
};

processUser(user);
// Processing user: Alice

processUser(user);
// User already processed
```

The same object reference cannot be processed twice through this function.

### 🖆 Preventing Circular Object Processing

WeakSet can help prevent infinite recursion when traversing objects that contain circular references.

```javascript
const visited = new WeakSet();

function traverse(object) {
    if (object === null || typeof object !== "object") {
        return;
    }

    if (visited.has(object)) {
        return;
    }

    visited.add(object);

    for (const key of Object.keys(object)) {
        console.log(key);

        traverse(object[key]);
    }
}

const user = {
    name: "Alice"
};

user.self = user;

traverse(user);
```

Without tracking visited objects, recursively traversing `user.self` would repeatedly revisit the same object.

WeakSet prevents that repeated traversal.

This example assumes that the traversal should process each object only once during the lifetime of the `visited` collection.

### 🖆 Tracking Initialized Objects

```javascript
const initialized = new WeakSet();

function initialize(component) {
    if (initialized.has(component)) {
        return;
    }

    initialized.add(component);

    component.initialized = true;

    console.log("Component initialized");
}

const component = {};

initialize(component);
// Component initialized

initialize(component);
// No additional output

console.log(component.initialized);
// true
```

This pattern is useful when initialization should happen only once per object reference.

### 🖆 Tracking DOM Elements

WeakSet can track DOM elements that have already received event listeners.

```javascript
const initializedElements = new WeakSet();

function initializeButton(button) {
    if (initializedElements.has(button)) {
        return;
    }

    initializedElements.add(button);

    button.addEventListener("click", () => {
        console.log("Button clicked");
    });
}

const button = document.querySelector("button");

initializeButton(button);
initializeButton(button);
```

The event listener is registered only once for the same button object.

When the DOM element becomes otherwise unreachable, the WeakSet does not keep it alive solely because it was tracked.

### 🖆 Tracking Validated Objects

```javascript
const validatedObjects = new WeakSet();

function validateUser(user) {
    if (validatedObjects.has(user)) {
        return true;
    }

    const isValid =
        typeof user.name === "string" &&
        typeof user.email === "string";

    if (isValid) {
        validatedObjects.add(user);
    }

    return isValid;
}

const user = {
    name: "Alice",
    email: "alice@example.com"
};

console.log(validateUser(user));
// true

console.log(validateUser(user));
// true
```

This example assumes that the object is not modified after successful validation. If its properties can change, cached validation results may become stale.

# 🗁 WeakSet and WeakMap

WeakSet and WeakMap share weak-key behavior, but they solve different problems.

| Feature | WeakSet | WeakMap |
| --- | --- | --- |
| Stores | Objects and supported symbols | Key-value pairs |
| Key-value association | No | Yes |
| Stores additional metadata | No | Yes |
| `add()` | Yes | No |
| `set()` | No | Yes |
| `get()` | No | Yes |
| `has()` | Yes | Yes |
| `delete()` | Yes | Yes |
| Iteration | No | No |
| `size` | No | No |

### 🖆 Choosing Between WeakSet and WeakMap

Use WeakSet when you only need to know whether an object has been registered or processed.

```javascript
const processed = new WeakSet();

processed.add(user);

console.log(processed.has(user));
// true
```

Use WeakMap when you need to associate additional information with an object.

```javascript
const metadata = new WeakMap();

metadata.set(user, {
    role: "admin"
});

console.log(metadata.get(user));
// { role: "admin" }
```

# 🗁 Common Mistakes

### 🖆 Storing Primitive Values

Incorrect:

```javascript
const items = new WeakSet();

items.add("JavaScript");
// TypeError
```

Correct:

```javascript
const items = new WeakSet();

items.add({
    language: "JavaScript"
});
```

### 🖆 Expecting a `size` Property

Incorrect:

```javascript
console.log(items.size);
// undefined
```

WeakSet does not expose its number of stored values.

### 🖆 Trying to Iterate Over a WeakSet

Incorrect:

```javascript
for (const item of items) {
    console.log(item);
}
```

WeakSet is not iterable.

If you need to iterate over values, use a regular Set.

### 🖆 Expecting `clear()` to Exist

Incorrect:

```javascript
items.clear();
// TypeError
```

WeakSet does not provide a `clear()` method.

### 🖆 Assuming Identical Objects Are Equal

```javascript
const items = new WeakSet();

items.add({
    id: 101
});

console.log(items.has({
    id: 101
}));
// false
```

The two object literals create different references.

### 🖆 Assuming Garbage Collection Happens Immediately

```javascript
let object = {
    name: "Example"
};

const items = new WeakSet();

items.add(object);

object = null;
```

The object may become eligible for garbage collection, but the JavaScript engine determines when collection happens.

# 🗁 Best Practices

* Use WeakSet to track object membership without keeping objects alive solely through the collection.
* Use `Set` when primitive values or iteration are required.
* Use `WeakMap` when additional metadata must be associated with objects.
* Use `has()` before processing an object when duplicate processing must be avoided.
* Use `delete()` when an object should no longer be tracked.
* Avoid relying on garbage collection timing.
* Remember that membership is based on object identity.
* Use WeakSet for temporary object tracking and lifecycle-related metadata.
* Do not use WeakSet when you need to inspect, enumerate, or count stored objects.

# 🗁 Summary

A JavaScript `WeakSet` is a collection that stores unique object references and supported non-registered symbols using weak references.

Key points:

* WeakSet stores objects, functions, and non-registered symbols.
* Primitive values are not supported.
* `add()` inserts a value.
* `has()` checks whether a value exists.
* `delete()` removes a value.
* WeakSet does not prevent stored objects from being garbage-collected.
* WeakSet is not iterable.
* WeakSet does not expose `size` or `clear()`.
* WeakSet is useful for tracking processed objects, DOM elements, and initialization state.

**Remember:** Use `Set` when you need a general-purpose collection of unique values. Use `WeakSet` when you only need to track object membership without keeping those objects alive solely through the collection.
