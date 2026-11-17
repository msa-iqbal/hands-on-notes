# JavaScript `WeakMap`

A JavaScript `WeakMap` is a built-in collection that stores key-value pairs, where keys must be objects or non-registered symbols.

Unlike a regular `Map`, a `WeakMap` holds its object keys weakly. This means storing an object as a WeakMap key does not, by itself, prevent that object from being garbage-collected when it becomes otherwise unreachable.

WeakMaps are particularly useful for associating private metadata with objects without extending the lifetime of those objects unnecessarily.

Common use cases include:

* Storing private object metadata.
* Associating additional data with DOM elements.
* Caching computed information for objects.
* Managing object-related state.
* Tracking resources associated with object lifetimes.
* Implementing private data storage.

# 🗁 Creating a WeakMap

### 🖆 WeakMap Constructor

Create an empty WeakMap using the `WeakMap` constructor.

```javascript
const metadata = new WeakMap();

console.log(metadata);
// WeakMap { <items unknown> }
```

The exact console representation depends on the JavaScript environment.

### 🖆 Creating a WeakMap with Initial Entries

A WeakMap can be initialized using an iterable containing key-value pairs.

```javascript
const user = { id: 101 };

const metadata = new WeakMap([
    [user, {
        role: "admin",
        lastLogin: "2026-11-14"
    }]
]);

console.log(metadata.get(user));
```

Every key must satisfy the WeakMap key requirements.

# 🗁 WeakMap Key Requirements

A WeakMap accepts:

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

### 🖆 Object Keys

```javascript
const user = { id: 101 };

const metadata = new WeakMap();

metadata.set(user, "Administrator");

console.log(metadata.get(user));
// Administrator
```

### 🖆 Function Keys

Functions can also be used as keys.

```javascript
function calculate() {
    return 10 * 20;
}

const metadata = new WeakMap();

metadata.set(calculate, {
    description: "Calculation function"
});

console.log(metadata.get(calculate));
```

### 🖆 Registered and Non-Registered Symbols

Modern JavaScript permits non-registered symbols as WeakMap keys.

```javascript
const localSymbol = Symbol("user");

const metadata = new WeakMap();

metadata.set(localSymbol, "Private metadata");

console.log(metadata.get(localSymbol));
// Private metadata
```

A symbol registered using `Symbol.for()` cannot be used as a WeakMap key.

```javascript
const globalSymbol = Symbol.for("user");

const metadata = new WeakMap();

metadata.set(globalSymbol, "Metadata");
// TypeError
```

This distinction exists because registered symbols are maintained in a global symbol registry and are not eligible for weak-key behavior.

# 🗁 Adding Entries with `set()`

The `set()` method adds or updates a key-value pair.

```javascript
const user = { id: 101 };

const metadata = new WeakMap();

metadata.set(user, {
    name: "Alice",
    role: "admin"
});

console.log(metadata.get(user));
```

### 🖆 Updating an Existing Entry

```javascript
const user = { id: 101 };

const metadata = new WeakMap();

metadata.set(user, "User");
metadata.set(user, "Administrator");

console.log(metadata.get(user));
// Administrator
```

Setting an existing key replaces its associated value.

### 🖆 Chaining `set()`

The `set()` method returns the WeakMap itself.

```javascript
const userA = { id: 101 };
const userB = { id: 102 };

const metadata = new WeakMap();

metadata
    .set(userA, "Alice")
    .set(userB, "Bob");

console.log(metadata.get(userA));
// Alice
```

# 🗁 Retrieving Values with `get()`

The `get()` method retrieves the value associated with a key.

If the key does not exist, it returns `undefined`.

```javascript
const user = { id: 101 };

const metadata = new WeakMap();

metadata.set(user, {
    role: "admin"
});

console.log(metadata.get(user));
// { role: "admin" }

console.log(metadata.get({ id: 101 }));
// undefined
```

The second object has the same properties but a different reference.

### 🖆 Accessing Missing Keys

```javascript
const metadata = new WeakMap();

const user = { id: 101 };

console.log(metadata.get(user));
// undefined
```

### 🖆 Storing `undefined`

A WeakMap can explicitly store `undefined` as a value.

Use `has()` to distinguish an existing entry from a missing entry.

```javascript
const metadata = new WeakMap();

const user = { id: 101 };

metadata.set(user, undefined);

console.log(metadata.get(user));
// undefined

console.log(metadata.has(user));
// true
```

# 🗁 Checking Keys with `has()`

The `has()` method checks whether a key exists in a WeakMap.

```javascript
const metadata = new WeakMap();

const user = { id: 101 };

metadata.set(user, "Alice");

console.log(metadata.has(user));
// true

console.log(metadata.has({ id: 101 }));
// false
```

WeakMap checks object identity rather than comparing object contents.

# 🗁 Removing Entries with `delete()`

The `delete()` method removes an entry associated with a key.

It returns `true` if the entry was removed, or `false` if the key did not exist.

```javascript
const metadata = new WeakMap();

const user = { id: 101 };

metadata.set(user, "Alice");

console.log(metadata.delete(user));
// true

console.log(metadata.has(user));
// false
```

WeakMap does not provide a `clear()` method.

If all entries need to become unreachable through the WeakMap variable, one approach is to replace the WeakMap reference.

```javascript
let metadata = new WeakMap();

const user = { id: 101 };

metadata.set(user, "Alice");

metadata = new WeakMap();
```

The old WeakMap and its entries can become eligible for garbage collection when no other references to them remain.

# 🗁 WeakMap and Garbage Collection

The most important characteristic of WeakMap is its weak association with object keys.

In a regular Map, storing an object as a key creates a strong reference to that object.

In a WeakMap, the key association does not independently keep the object alive.

### 🖆 Regular Map Reference

```javascript
let user = {
    id: 101,
    name: "Alice"
};

const users = new Map();

users.set(user, "User metadata");

user = null;
```

Even after the `user` variable is set to `null`, the Map still holds a strong reference to the original object.

The object remains reachable through the Map.

### 🖆 WeakMap Reference

```javascript
let user = {
    id: 101,
    name: "Alice"
};

const metadata = new WeakMap();

metadata.set(user, "User metadata");

user = null;
```

After the external reference is removed, the object may become eligible for garbage collection.

The WeakMap does not prevent that collection.

Its corresponding entry becomes inaccessible and is eventually removed as part of garbage collection.

**Important:** Garbage collection is automatic and nondeterministic. Setting a variable to `null` does not guarantee that an object is collected immediately.

# 🗁 WeakMap vs Map

Both collections store key-value pairs, but they have different behavior.

| Feature | Map | WeakMap |
| --- | --- | --- |
| Key types | Any value | Objects and non-registered symbols |
| Strong key references | Yes | No |
| Supports `get()` | Yes | Yes |
| Supports `set()` | Yes | Yes |
| Supports `has()` | Yes | Yes |
| Supports `delete()` | Yes | Yes |
| Supports `clear()` | Yes | No |
| Supports `size` | Yes | No |
| Supports iteration | Yes | No |
| Supports `forEach()` | Yes | No |
| Exposes keys and values iterators | Yes | No |
| Suitable for object metadata | Yes | Yes |
| Prevents keys from being kept alive solely by the collection | Yes | No |

### 🖆 Why WeakMap Is Not Iterable

A WeakMap does not provide:

* `keys()`
* `values()`
* `entries()`
* `forEach()`
* `size`
* A default iterator

This restriction is intentional.

If WeakMap exposed its keys or size, applications could potentially observe garbage collection indirectly.

Because garbage collection is nondeterministic, such observations would produce unpredictable results.

WeakMap therefore exposes only direct key-based operations.

# 🗁 Practical WeakMap Examples

### 🖆 Storing Private Metadata

WeakMap can store information associated with objects without placing that information directly on the objects.

```javascript
const privateData = new WeakMap();

class User {
    constructor(name, password) {
        this.name = name;

        privateData.set(this, {
            password
        });
    }

    verifyPassword(password) {
        return privateData.get(this).password === password;
    }
}

const user = new User("Alice", "secret123");

console.log(user.name);
// Alice

console.log(user.password);
// undefined

console.log(user.verifyPassword("secret123"));
// true
```

The password is not exposed as a normal property of the instance.

This is a conceptual example of private metadata storage, not a complete authentication or password-storage design.

### 🖆 Storing DOM Element Metadata

WeakMap is useful for associating additional information with DOM elements.

```javascript
const elementMetadata = new WeakMap();

const button = document.querySelector("button");

elementMetadata.set(button, {
    clicks: 0,
    label: "Submit button"
});

button.addEventListener("click", () => {
    const metadata = elementMetadata.get(button);

    metadata.clicks++;

    console.log(metadata.clicks);
});
```

When the DOM element becomes otherwise unreachable, the WeakMap association does not keep it alive.

### 🖆 Caching Computed Results

WeakMap can cache information associated with object inputs.

```javascript
const cache = new WeakMap();

function calculateArea(shape) {
    if (cache.has(shape)) {
        return cache.get(shape);
    }

    const result = shape.width * shape.height;

    cache.set(shape, result);

    return result;
}

const rectangle = {
    width: 10,
    height: 5
};

console.log(calculateArea(rectangle));
// 50

console.log(calculateArea(rectangle));
// 50
```

The second call retrieves the cached result.

This cache is associated with the rectangle object rather than a primitive identifier.

If the rectangle becomes unreachable elsewhere, the WeakMap does not keep it alive solely because it is cached.

### 🖆 Associating State with Objects

```javascript
const objectState = new WeakMap();

function initializeState(element) {
    objectState.set(element, {
        initialized: true,
        active: false
    });
}

function activate(element) {
    const state = objectState.get(element);

    if (state) {
        state.active = true;
    }
}

const component = {};

initializeState(component);
activate(component);

console.log(objectState.get(component));
```

Result:

```javascript
{
    initialized: true,
    active: true
}
```

### 🖆 Tracking Object Processing

```javascript
const processedObjects = new WeakMap();

function processObject(object) {
    if (processedObjects.has(object)) {
        return processedObjects.get(object);
    }

    const result = {
        processed: true,
        timestamp: Date.now()
    };

    processedObjects.set(object, result);

    return result;
}

const task = {
    id: 101
};

console.log(processObject(task));
console.log(processObject(task));
```

The same object reference reuses its previously stored processing result.

# 🗁 WeakMap and Private Class Data

Before native private class fields became available, WeakMap was a common technique for implementing private instance data.

It is still useful when private metadata should remain outside the instance itself.

### 🖆 Using WeakMap for Class Privacy

```javascript
const privateData = new WeakMap();

class BankAccount {
    constructor(balance) {
        privateData.set(this, {
            balance
        });
    }

    deposit(amount) {
        const data = privateData.get(this);

        data.balance += amount;
    }

    getBalance() {
        return privateData.get(this).balance;
    }
}

const account = new BankAccount(1000);

account.deposit(500);

console.log(account.getBalance());
// 1500

console.log(account.balance);
// undefined
```

### 🖆 Comparing WeakMap with Private Fields

Modern JavaScript supports native private class fields using the `#` prefix.

```javascript
class BankAccount {
    #balance;

    constructor(balance) {
        this.#balance = balance;
    }

    deposit(amount) {
        this.#balance += amount;
    }

    getBalance() {
        return this.#balance;
    }
}

const account = new BankAccount(1000);

account.deposit(500);

console.log(account.getBalance());
// 1500
```

Both approaches can encapsulate instance data.

Native private fields are generally simpler for ordinary class encapsulation.

WeakMap remains useful when metadata must be maintained externally or when working with objects that cannot be modified directly.

# 🗁 WeakMap and Memory Management

WeakMap is particularly useful when data should follow the lifetime of an object.

Consider a system that associates metadata with many DOM elements.

Using a regular Map can keep those elements alive as long as the Map retains references to them.

Using a WeakMap avoids this strong-reference relationship for keys.

### 🖆 Memory-Friendly Metadata Storage

```javascript
const metadata = new WeakMap();

function attachMetadata(element, data) {
    metadata.set(element, data);
}

function getMetadata(element) {
    return metadata.get(element);
}
```

The WeakMap does not independently prevent an element from being garbage-collected.

However, weak keys do not guarantee that every associated value is immediately collected. Values can still be retained through other references, and garbage collection timing is controlled by the JavaScript engine.

# 🗁 WeakMap Limitations

* Keys must be objects or non-registered symbols.
* Primitive values cannot be used as keys.
* WeakMaps cannot be iterated.
* WeakMaps do not expose their size.
* WeakMaps do not provide `clear()`.
* WeakMaps cannot be converted directly into arrays using the spread operator.
* Garbage collection cannot be controlled or observed through WeakMap.
* A WeakMap does not make its values automatically private or secure.
* WeakMap is not a replacement for every Map use case.

# 🗁 Common Mistakes

### 🖆 Using Primitive Values as Keys

Incorrect:

```javascript
const metadata = new WeakMap();

metadata.set("user", "Alice");
// TypeError
```

Correct:

```javascript
const user = {
    id: 101
};

metadata.set(user, "Alice");
```

### 🖆 Expecting a `size` Property

Incorrect:

```javascript
console.log(metadata.size);
// undefined
```

WeakMap does not expose the number of entries.

### 🖆 Trying to Iterate Over a WeakMap

Incorrect:

```javascript
for (const entry of metadata) {
    console.log(entry);
}
```

WeakMap is not iterable.

If iteration is required, use a regular Map.

### 🖆 Expecting `clear()` to Exist

Incorrect:

```javascript
metadata.clear();
// TypeError
```

WeakMap does not provide a `clear()` method.

Create a new WeakMap when the entire collection needs to be replaced.

### 🖆 Assuming Identical Objects Are the Same Key

```javascript
const metadata = new WeakMap();

metadata.set({ id: 1 }, "Alice");

console.log(metadata.get({ id: 1 }));
// undefined
```

Each object literal creates a different object reference.

### 🖆 Assuming Garbage Collection Happens Immediately

```javascript
let user = {
    id: 101
};

const metadata = new WeakMap();

metadata.set(user, "User metadata");

user = null;
```

The original object may become eligible for garbage collection, but the JavaScript engine decides when collection occurs.

# 🗁 Best Practices

* Use WeakMap for metadata associated with object lifetimes.
* Use `Map` when iteration or collection size is required.
* Use `WeakMap` when keys should not be kept alive solely by the collection.
* Use `get()` to retrieve associated metadata.
* Use `has()` when distinguishing missing entries from stored `undefined` values.
* Use `delete()` to remove individual entries when needed.
* Prefer native private class fields for straightforward class encapsulation.
* Avoid using WeakMap as a general-purpose collection.
* Do not depend on garbage collection timing.
* Remember that WeakMap keys are compared by reference for objects.

# 🗁 Summary

A JavaScript `WeakMap` is a collection that associates values with weakly held object or non-registered symbol keys.

Key points:

* WeakMap stores key-value pairs.
* Object and function keys are supported.
* Non-registered symbols are supported as keys in modern JavaScript.
* Primitive keys are not supported.
* `set()` adds or updates entries.
* `get()` retrieves values.
* `has()` checks whether a key exists.
* `delete()` removes an entry.
* WeakMap keys do not independently prevent garbage collection.
* WeakMaps are not iterable.
* WeakMaps do not expose `size` or `clear()`.
* WeakMap is useful for private metadata, caching, and object-associated state.

**Remember:** Use `Map` when you need a fully iterable key-value collection. Use `WeakMap` when you need to associate data with object lifetimes without keeping those objects alive solely through the collection.
