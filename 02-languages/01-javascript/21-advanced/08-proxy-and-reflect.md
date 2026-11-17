# JavaScript Proxy and Reflect

**Proxy** and **Reflect** are JavaScript features that provide powerful mechanisms for intercepting and customizing object operations.

They are commonly used in:

* Object validation
* Property access control
* Data observation
* Reactive programming
* API wrappers
* Logging and debugging
* Framework internals
* Metaprogramming

A `Proxy` allows developers to intercept operations performed on an object.

`Reflect` provides methods for performing many of those operations using standardized JavaScript internal object behaviors.

# 🗁 What Is a Proxy?

A **Proxy** is an object that wraps another object and intercepts operations performed on it.

The wrapped object is called the **target**.

The object containing interception methods is called the **handler**.

Syntax:

```javascript
const proxy = new Proxy(target, handler);
```

Parameters:

* `target`: The original object being wrapped.
* `handler`: An object containing trap methods that intercept operations.

Example:

```javascript
const user = {
    name: "Alice",
    age: 25
};

const proxy = new Proxy(user, {});

console.log(proxy.name); // Alice
console.log(proxy.age);  // 25
```

Even with an empty handler, the proxy forwards ordinary operations to its target.

# 🗁 Understanding Proxy Architecture

A proxy consists of three main parts:

* Target: The original object.
* Handler: The object containing interception methods.
* Traps: Handler methods that intercept specific operations.

Conceptual structure:

```text
        Application Code
               |
               v
          Proxy Object
               |
               v
           Handler
               |
          Intercepted
           Operation
               |
               v
          Target Object
```

# 🗁 What Are Proxy Traps?

**Proxy Traps** are handler methods that intercept operations performed on a target object.

For example:

* `get`: Intercepts property access.
* `set`: Intercepts property assignment.
* `has`: Intercepts the `in` operator.
* `deleteProperty`: Intercepts property deletion.
* `apply`: Intercepts function calls.
* `construct`: Intercepts constructor calls.
* `ownKeys`: Intercepts property key enumeration.
* `getOwnPropertyDescriptor`: Intercepts property descriptor retrieval.
* `defineProperty`: Intercepts property definition.
* `getPrototypeOf`: Intercepts prototype retrieval.
* `setPrototypeOf`: Intercepts prototype modification.
* `isExtensible`: Intercepts extensibility checks.
* `preventExtensions`: Intercepts attempts to prevent extensions.

# 🗁 The get Trap

The `get` trap intercepts property access.

Syntax:

```javascript
get(target, property, receiver) {
    // Custom behavior
}
```

Parameters:

* `target`: The original object.
* `property`: The requested property key.
* `receiver`: The proxy or object through which the property access originated.

Example:

```javascript
const user = {
    name: "Alice",
    age: 25
};

const proxy = new Proxy(user, {
    get(target, property, receiver) {
        console.log(`Reading property: ${String(property)}`);

        return Reflect.get(target, property, receiver);
    }
});

console.log(proxy.name);
```

Output:

```text
Reading property: name
Alice
```

The trap logs the property access and delegates the actual retrieval to `Reflect.get()`.

# 🗁 The set Trap

The `set` trap intercepts property assignments.

Syntax:

```javascript
set(target, property, value, receiver) {
    // Custom behavior
}
```

Parameters:

* `target`: The original object.
* `property`: The property being modified.
* `value`: The new value.
* `receiver`: The proxy or object through which the assignment originated.

Example:

```javascript
const user = {
    name: "Alice",
    age: 25
};

const proxy = new Proxy(user, {
    set(target, property, value, receiver) {
        console.log(`Updating ${String(property)}:`, value);

        return Reflect.set(target, property, value, receiver);
    }
});

proxy.age = 30;

console.log(user.age); // 30
```

A `set` trap should return a Boolean indicating whether the assignment was successful.

# 🗁 Property Validation with Proxy

The `set` trap can validate values before modifying an object.

Example:

```javascript
const user = {
    name: "Alice",
    age: 25
};

const proxy = new Proxy(user, {
    set(target, property, value, receiver) {
        if (property === "age" && !Number.isInteger(value)) {
            throw new TypeError("Age must be an integer");
        }

        if (property === "age" && value < 0) {
            throw new RangeError("Age cannot be negative");
        }

        return Reflect.set(target, property, value, receiver);
    }
});

proxy.age = 30;

console.log(proxy.age); // 30

// proxy.age = -5; // RangeError
// proxy.age = 25.5; // TypeError
```

This pattern can enforce basic object-level constraints.

# 🗁 The has Trap

The `has` trap intercepts the `in` operator.

Syntax:

```javascript
has(target, property) {
    // Custom behavior
}
```

Example:

```javascript
const user = {
    name: "Alice",
    age: 25
};

const proxy = new Proxy(user, {
    has(target, property) {
        console.log(`Checking property: ${String(property)}`);

        return Reflect.has(target, property);
    }
});

console.log("name" in proxy);
```

Output:

```text
Checking property: name
true
```

# 🗁 The deleteProperty Trap

The `deleteProperty` trap intercepts property deletion.

Syntax:

```javascript
deleteProperty(target, property) {
    // Custom behavior
}
```

Example:

```javascript
const user = {
    name: "Alice",
    age: 25
};

const proxy = new Proxy(user, {
    deleteProperty(target, property) {
        if (property === "name") {
            throw new Error("The name property cannot be deleted");
        }

        return Reflect.deleteProperty(target, property);
    }
});

delete proxy.age;

console.log(user); // { name: "Alice" }
```

# 🗁 The ownKeys Trap

The `ownKeys` trap intercepts operations that retrieve an object's own property keys.

Common operations include:

* `Object.keys()`
* `Object.getOwnPropertyNames()`
* `Reflect.ownKeys()`

Example:

```javascript
const user = {
    name: "Alice",
    age: 25,
    password: "secret"
};

const proxy = new Proxy(user, {
    ownKeys(target) {
        return Reflect.ownKeys(target).filter(
            key => key !== "password"
        );
    }
});

console.log(Object.keys(proxy));
// ["name", "age"]
```

Proxy invariants still apply. For example, non-configurable own properties cannot simply be omitted from the returned key list.

# 🗁 The apply Trap

The `apply` trap intercepts function calls.

It works with callable targets.

Syntax:

```javascript
apply(target, thisArg, argumentsList) {
    // Custom behavior
}
```

Parameters:

* `target`: The original function.
* `thisArg`: The value of `this` for the function call.
* `argumentsList`: An array containing the supplied arguments.

Example:

```javascript
function add(a, b) {
    return a + b;
}

const proxy = new Proxy(add, {
    apply(target, thisArg, argumentsList) {
        console.log("Function called");

        return Reflect.apply(
            target,
            thisArg,
            argumentsList
        );
    }
});

console.log(proxy(10, 20));
```

Output:

```text
Function called
30
```

# 🗁 The construct Trap

The `construct` trap intercepts object creation through the `new` operator.

It works with constructor targets.

Syntax:

```javascript
construct(target, argumentsList, newTarget) {
    // Custom behavior
}
```

Example:

```javascript
class User {
    constructor(name) {
        this.name = name;
    }
}

const ProxyUser = new Proxy(User, {
    construct(target, argumentsList, newTarget) {
        console.log("Creating user");

        return Reflect.construct(
            target,
            argumentsList,
            newTarget
        );
    }
});

const user = new ProxyUser("Alice");

console.log(user.name); // Alice
```

The `construct` trap must return an object.

# 🗁 Proxy with Functions

A proxy can wrap a function to intercept calls, construction, and property operations.

Example:

```javascript
function multiply(a, b) {
    return a * b;
}

const calculator = new Proxy(multiply, {
    apply(target, thisArg, argumentsList) {
        const result = Reflect.apply(
            target,
            thisArg,
            argumentsList
        );

        console.log("Result:", result);

        return result;
    }
});

calculator(5, 4);
```

Output:

```text
Result: 20
```

# 🗁 Proxy Invariants

**Proxy Invariants** are rules that prevent a proxy from violating fundamental object behavior.

JavaScript engines enforce these rules even when a handler defines custom traps.

Examples:

* A proxy cannot report an incompatible value for certain non-configurable, non-writable data properties.
* A proxy cannot hide non-configurable own properties through `ownKeys`.
* A proxy cannot report an object as extensible when its target is not extensible.
* A proxy cannot report an incompatible property descriptor for a non-configurable property.
* A proxy cannot report an incompatible prototype for a non-extensible target.

Example:

```javascript
const target = {};

Object.defineProperty(target, "name", {
    value: "Alice",
    writable: false,
    configurable: false
});

const proxy = new Proxy(target, {
    get() {
        return "Bob";
    }
});

console.log(proxy.name);
```

This operation throws a `TypeError` because the trap attempts to return a value different from the value of a non-configurable, non-writable data property.

# 🗁 What Is Reflect?

**Reflect** is a built-in JavaScript object that provides methods for performing fundamental object operations.

It is not a constructor and cannot be instantiated.

Example:

```javascript
console.log(typeof Reflect); // object
```

Reflect methods correspond to many operations that can be intercepted by proxy traps.

Examples:

* `Reflect.get()`
* `Reflect.set()`
* `Reflect.has()`
* `Reflect.deleteProperty()`
* `Reflect.ownKeys()`
* `Reflect.apply()`
* `Reflect.construct()`
* `Reflect.defineProperty()`
* `Reflect.getOwnPropertyDescriptor()`
* `Reflect.getPrototypeOf()`
* `Reflect.setPrototypeOf()`
* `Reflect.isExtensible()`
* `Reflect.preventExtensions()`

# 🗁 Reflect.get()

`Reflect.get()` retrieves a property from an object.

Syntax:

```javascript
Reflect.get(target, propertyKey, receiver);
```

Example:

```javascript
const user = {
    name: "Alice",
    age: 25
};

console.log(Reflect.get(user, "name"));
// Alice

console.log(Reflect.get(user, "age"));
// 25
```

It also supports an optional receiver for accessor properties.

```javascript
const user = {
    firstName: "Alice",

    get name() {
        return this.firstName;
    }
};

const receiver = {
    firstName: "Bob"
};

console.log(
    Reflect.get(user, "name", receiver)
);
// Bob
```

# 🗁 Reflect.set()

`Reflect.set()` assigns a property value.

Syntax:

```javascript
Reflect.set(target, propertyKey, value, receiver);
```

Example:

```javascript
const user = {
    name: "Alice"
};

const success = Reflect.set(user, "name", "Bob");

console.log(success);   // true
console.log(user.name); // Bob
```

Unlike ordinary assignment, `Reflect.set()` returns a Boolean indicating whether the operation succeeded.

# 🗁 Reflect.has()

`Reflect.has()` checks whether a property exists on an object or anywhere in its prototype chain.

Example:

```javascript
const user = {
    name: "Alice"
};

console.log(Reflect.has(user, "name"));
// true

console.log(Reflect.has(user, "age"));
// false
```

It is equivalent in purpose to the `in` operator.

```javascript
console.log("name" in user);
console.log(Reflect.has(user, "name"));
```

# 🗁 Reflect.deleteProperty()

`Reflect.deleteProperty()` deletes an object's property.

Example:

```javascript
const user = {
    name: "Alice",
    age: 25
};

const success = Reflect.deleteProperty(user, "age");

console.log(success); // true
console.log(user);    // { name: "Alice" }
```

It returns a Boolean indicating whether the deletion operation succeeded.

# 🗁 Reflect.ownKeys()

`Reflect.ownKeys()` returns all own property keys of an object.

It includes:

* Enumerable string keys
* Non-enumerable string keys
* Symbol keys

Example:

```javascript
const id = Symbol("id");

const user = {
    name: "Alice",
    age: 25,
    [id]: 1001
};

Object.defineProperty(user, "secret", {
    value: "hidden",
    enumerable: false
});

console.log(Reflect.ownKeys(user));
```

Possible output:

```javascript
[
    "name",
    "age",
    "secret",
    Symbol(id)
]
```

# 🗁 Reflect.apply()

`Reflect.apply()` invokes a function with a specified `this` value and arguments.

Syntax:

```javascript
Reflect.apply(target, thisArgument, argumentsList);
```

Example:

```javascript
function greet(message) {
    return `${message}, ${this.name}`;
}

const user = {
    name: "Alice"
};

const result = Reflect.apply(
    greet,
    user,
    ["Hello"]
);

console.log(result);
// Hello, Alice
```

# 🗁 Reflect.construct()

`Reflect.construct()` creates an object using a constructor.

Example:

```javascript
class User {
    constructor(name, age) {
        this.name = name;
        this.age = age;
    }
}

const user = Reflect.construct(
    User,
    ["Alice", 25]
);

console.log(user);
```

It can also receive a third argument to specify the constructor used as `newTarget`.

# 🗁 Reflect.defineProperty()

`Reflect.defineProperty()` defines or modifies an object's property descriptor.

Example:

```javascript
const user = {};

const success = Reflect.defineProperty(user, "name", {
    value: "Alice",
    writable: true,
    enumerable: true,
    configurable: true
});

console.log(success); // true
console.log(user.name); // Alice
```

It returns a Boolean indicating whether the property definition succeeded.

# 🗁 Reflect.getOwnPropertyDescriptor()

`Reflect.getOwnPropertyDescriptor()` retrieves the property descriptor of an object's own property.

Example:

```javascript
const user = {
    name: "Alice"
};

const descriptor = Reflect.getOwnPropertyDescriptor(
    user,
    "name"
);

console.log(descriptor);
```

Possible output:

```javascript
{
    value: "Alice",
    writable: true,
    enumerable: true,
    configurable: true
}
```

It returns `undefined` when the requested own property does not exist.

# 🗁 Reflect.getPrototypeOf()

`Reflect.getPrototypeOf()` retrieves an object's prototype.

Example:

```javascript
const user = {
    name: "Alice"
};

const prototype = Reflect.getPrototypeOf(user);

console.log(prototype === Object.prototype);
// true
```

# 🗁 Reflect.setPrototypeOf()

`Reflect.setPrototypeOf()` changes an object's prototype.

Example:

```javascript
const parent = {
    greet() {
        return "Hello";
    }
};

const user = {
    name: "Alice"
};

const success = Reflect.setPrototypeOf(user, parent);

console.log(success); // true
console.log(user.greet()); // Hello
```

Changing prototypes dynamically can affect performance and should generally be avoided in performance-sensitive code.

# 🗁 Reflect.isExtensible()

`Reflect.isExtensible()` checks whether new properties can be added to an object.

Example:

```javascript
const user = {
    name: "Alice"
};

console.log(Reflect.isExtensible(user));
// true

Object.preventExtensions(user);

console.log(Reflect.isExtensible(user));
// false
```

# 🗁 Reflect.preventExtensions()

`Reflect.preventExtensions()` prevents new properties from being added to an object.

Example:

```javascript
const user = {
    name: "Alice"
};

const success = Reflect.preventExtensions(user);

console.log(success); // true

console.log(
    Reflect.isExtensible(user)
); // false
```

Existing properties can still be modified or deleted when their descriptors permit those operations.

# 🗁 Proxy and Reflect Together

Proxy and Reflect are commonly used together.

The proxy intercepts an operation, while Reflect delegates the operation to the target using standardized object semantics.

Example:

```javascript
const user = {
    name: "Alice",
    age: 25
};

const proxy = new Proxy(user, {
    get(target, property, receiver) {
        console.log(`Getting ${String(property)}`);

        return Reflect.get(target, property, receiver);
    },

    set(target, property, value, receiver) {
        console.log(`Setting ${String(property)}`);

        return Reflect.set(target, property, value, receiver);
    }
});

console.log(proxy.name);

proxy.age = 30;
```

This approach avoids manually reimplementing ordinary property behavior.

# 🗁 Practical Example: Read-Only Object

A proxy can prevent modification of an object.

Example:

```javascript
function createReadOnlyObject(target) {
    return new Proxy(target, {
        set() {
            throw new TypeError("Object is read-only");
        },

        deleteProperty() {
            throw new TypeError("Object is read-only");
        },

        defineProperty() {
            throw new TypeError("Object is read-only");
        }
    });
}

const user = createReadOnlyObject({
    name: "Alice",
    age: 25
});

console.log(user.name);

// user.age = 30;
// TypeError: Object is read-only
```

This is a basic read-only wrapper. It does not make nested objects immutable.

# 🗁 Practical Example: Default Property Values

A proxy can provide default values for missing properties.

Example:

```javascript
const user = {
    name: "Alice"
};

const proxy = new Proxy(user, {
    get(target, property, receiver) {
        const value = Reflect.get(target, property, receiver);

        return value === undefined
            ? "Not available"
            : value;
    }
});

console.log(proxy.name);
// Alice

console.log(proxy.age);
// Not available
```

This behavior also applies when an existing property has the value `undefined`.

# 🗁 Practical Example: Property Access Logging

A proxy can log property reads and writes.

Example:

```javascript
function createLogger(target) {
    return new Proxy(target, {
        get(target, property, receiver) {
            console.log(`READ: ${String(property)}`);

            return Reflect.get(target, property, receiver);
        },

        set(target, property, value, receiver) {
            console.log(
                `WRITE: ${String(property)} = ${value}`
            );

            return Reflect.set(
                target,
                property,
                value,
                receiver
            );
        }
    });
}

const settings = createLogger({
    theme: "dark",
    language: "en"
});

console.log(settings.theme);

settings.theme = "light";
```

Output:

```text
READ: theme
dark
WRITE: theme = light
```

# 🗁 Practical Example: Negative Array Indexing

JavaScript arrays do not support Python-style negative indexing directly.

A proxy can implement that behavior.

Example:

```javascript
function createNegativeIndexArray(array) {
    return new Proxy(array, {
        get(target, property, receiver) {
            if (
                typeof property === "string" &&
                /^-\d+$/.test(property)
            ) {
                const index = target.length + Number(property);

                return Reflect.get(
                    target,
                    String(index),
                    receiver
                );
            }

            return Reflect.get(target, property, receiver);
        }
    });
}

const numbers = createNegativeIndexArray([
    10,
    20,
    30,
    40
]);

console.log(numbers[-1]); // 40
console.log(numbers[-2]); // 30
console.log(numbers[0]);  // 10
```

This example customizes reads only. It does not implement negative-index assignment or deletion.

# 🗁 Proxy Limitations

Although proxies are powerful, they have important limitations.

* A proxy does not automatically make nested objects proxied.
* Some built-in objects require special handling because their methods depend on internal slots.
* Proxy traps must respect JavaScript invariants.
* Proxy operations can introduce performance overhead.
* Proxies cannot intercept every possible language operation.
* A proxy cannot be transparently substituted for every target object in every situation.
* Some built-in methods may fail when called with a proxy as their receiver.

For example, a `Map` uses internal slots that an ordinary proxy does not automatically provide.

```javascript
const map = new Map();

const proxy = new Proxy(map, {});

// proxy.set("name", "Alice");
// TypeError in typical implementations
```

The `Map.prototype.set()` method expects a receiver with the required Map internal state.

One possible approach is to bind methods to the original target:

```javascript
const proxy = new Proxy(map, {
    get(target, property) {
        const value = Reflect.get(target, property, target);

        return typeof value === "function"
            ? value.bind(target)
            : value;
    }
});

proxy.set("name", "Alice");

console.log(proxy.get("name"));
// Alice
```

This is a basic workaround. Binding methods changes their `this` behavior and may not preserve all desired proxy semantics.

# 🗁 Proxy vs Reflect

| Feature | Proxy | Reflect |
| --- | --- | --- |
| Main purpose | Intercept object operations | Perform object operations |
| Primary mechanism | Handler traps | Static methods |
| Custom behavior | Yes | No |
| Property access | Intercepts through `get` | Performs through `Reflect.get()` |
| Property assignment | Intercepts through `set` | Performs through `Reflect.set()` |
| Function calls | Can intercept with `apply` | Can invoke with `Reflect.apply()` |
| Object construction | Can intercept with `construct` | Can construct with `Reflect.construct()` |
| Typical use | Validation, observation, reactive systems | Delegation and standardized operations |

# 🗁 Common Use Cases

**Reactive Systems**

Frameworks can use proxies to detect property access and modification.

**Validation**

Proxies can validate property values before accepting assignments.

**Logging**

Proxies can record property reads, writes, and function calls.

**Access Control**

Proxies can restrict access to selected properties or operations.

**API Wrappers**

Proxies can provide custom behavior around existing objects and functions.

**Virtual Objects**

Proxies can represent objects whose properties are computed dynamically.

**Dependency Tracking**

Reactive systems can track which properties are accessed and determine which computations need updating.

# 🗁 Best Practices

* Use proxies when interception is genuinely required.
* Use Reflect for forwarding intercepted operations.
* Preserve the receiver when delegating property operations.
* Return the correct Boolean values from `set`, `deleteProperty`, and other Boolean-returning traps.
* Respect proxy invariants.
* Avoid unnecessary proxy layers in performance-sensitive code.
* Remember that nested objects require separate proxying when deep interception is needed.
* Test proxies with non-configurable and non-writable properties.
* Handle built-in objects with internal slots carefully.
* Keep proxy behavior predictable and easy to debug.

# 🗁 Key Takeaways

* Proxy wraps an object and intercepts operations performed on it.
* A proxy consists of a target, handler, and traps.
* Traps customize property access, assignment, deletion, function calls, and other operations.
* Reflect provides standardized methods for performing fundamental object operations.
* Proxy and Reflect are commonly used together.
* Proxy invariants enforce JavaScript object consistency.
* Proxies are useful for validation, logging, reactive programming, and metaprogramming.
* Proxies do not automatically intercept operations on nested objects.
* Built-in objects with internal slots may require special handling.

# 🗁 Related Topics

* Objects
* Property Descriptors
* Getters and Setters
* Prototype Chain
* Symbols
* Closures
* Classes
* Metaprogramming
* Reactive Programming
* Memory Management
