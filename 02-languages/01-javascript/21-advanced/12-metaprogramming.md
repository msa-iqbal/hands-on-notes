# JavaScript Metaprogramming

**Metaprogramming** is a programming technique in which code can inspect, modify, generate, or control the behavior of other code.

In JavaScript, metaprogramming allows developers to customize how objects, functions, classes, and language operations behave.

JavaScript provides several features for metaprogramming:

* Proxy
* Reflect
* Symbols
* Property descriptors
* Getters and setters
* Dynamic property access
* Dynamic imports
* Function constructors
* Object prototypes
* Generators and iterators

Metaprogramming is widely used in:

* Framework development
* Reactive programming
* Dependency injection
* Object-relational mapping (ORM)
* Validation systems
* API design
* Serialization
* Testing and mocking
* Libraries and developer tools

# 🗁 What Is Metaprogramming?

Traditional programming focuses on writing instructions that perform application tasks.

Metaprogramming focuses on writing code that controls or modifies the behavior of other code.

Example of traditional programming:

```javascript
function add(a, b) {
    return a + b;
}

console.log(add(10, 20));
```

Example of metaprogramming:

```javascript
const user = {
    name: "Alice",
    age: 25
};

const proxy = new Proxy(user, {
    get(target, property, receiver) {
        console.log(`Accessing: ${String(property)}`);

        return Reflect.get(target, property, receiver);
    }
});

console.log(proxy.name);
```

The second example modifies how property access behaves without changing the original object.

# 🗁 Types of Metaprogramming

Metaprogramming can be understood through several approaches.

**Introspection**

Examining the structure and properties of objects or code at runtime.

**Intercession**

Intercepting and customizing operations performed on objects or functions.

**Code Generation**

Creating executable code dynamically.

**Reflection**

Inspecting and manipulating program structures through language-provided APIs.

JavaScript supports all these approaches through different language features.

# 🗁 Introspection

**Introspection** means examining an object's structure, properties, methods, and characteristics at runtime.

JavaScript provides several built-in methods for this purpose.

Example:

```javascript
const user = {
    name: "Alice",
    age: 25,

    greet() {
        return "Hello";
    }
};

console.log(Object.keys(user));
// ["name", "age", "greet"]

console.log(Object.values(user));
// ["Alice", 25, function]

console.log(Object.getOwnPropertyNames(user));
// ["name", "age", "greet"]

console.log(typeof user.greet);
// function
```

Introspection is useful when building generic utilities that work with different object structures.

# 🗁 Inspecting Property Descriptors

Property descriptors provide information about how object properties behave.

Example:

```javascript
const user = {
    name: "Alice"
};

const descriptor = Object.getOwnPropertyDescriptor(
    user,
    "name"
);

console.log(descriptor);
```

Output:

```javascript
{
    value: "Alice",
    writable: true,
    enumerable: true,
    configurable: true
}
```

Developers can inspect and modify property behavior using:

* `Object.getOwnPropertyDescriptor()`
* `Object.getOwnPropertyDescriptors()`
* `Object.defineProperty()`
* `Object.defineProperties()`

Example:

```javascript
const user = {};

Object.defineProperty(user, "id", {
    value: 1001,
    writable: false,
    enumerable: true,
    configurable: false
});

console.log(user.id);
```

# 🗁 Dynamic Property Access

JavaScript allows property names to be determined at runtime.

Example:

```javascript
const user = {
    name: "Alice",
    age: 25
};

const property = "name";

console.log(user[property]);
// Alice
```

This is useful for building dynamic utilities and data-processing systems.

Example:

```javascript
function getProperty(object, property) {
    return object[property];
}

const user = {
    name: "Alice",
    age: 25
};

console.log(getProperty(user, "age"));
// 25
```

# 🗁 Computed Property Names

Computed property names allow property keys to be evaluated dynamically during object creation.

Example:

```javascript
const property = "username";

const user = {
    [property]: "alice"
};

console.log(user.username);
// alice
```

They also work with symbols.

```javascript
const id = Symbol("id");

const user = {
    name: "Alice",
    [id]: 1001
};

console.log(user[id]);
// 1001
```

# 🗁 Intercession with Proxy

**Intercession** means intercepting operations and controlling how they behave.

JavaScript provides `Proxy` for this purpose.

Example:

```javascript
const user = {
    name: "Alice",
    age: 25
};

const proxy = new Proxy(user, {
    get(target, property, receiver) {
        console.log(`GET: ${String(property)}`);

        return Reflect.get(target, property, receiver);
    },

    set(target, property, value, receiver) {
        console.log(`SET: ${String(property)}`);

        return Reflect.set(target, property, value, receiver);
    }
});

console.log(proxy.name);

proxy.age = 30;
```

A proxy can intercept property access, assignment, deletion, function calls, and other supported operations.

# 🗁 Proxy Traps

Proxy traps are handler methods that customize fundamental object operations.

Common traps include:

| Trap | Intercepted Operation |
| --- | --- |
| `get` | Property access |
| `set` | Property assignment |
| `has` | `in` operator |
| `deleteProperty` | Property deletion |
| `ownKeys` | Own property key retrieval |
| `apply` | Function invocation |
| `construct` | Constructor invocation |
| `defineProperty` | Property definition |
| `getPrototypeOf` | Prototype retrieval |
| `setPrototypeOf` | Prototype modification |
| `isExtensible` | Extensibility check |
| `preventExtensions` | Preventing extensions |

Example:

```javascript
const settings = {
    theme: "dark"
};

const proxy = new Proxy(settings, {
    get(target, property, receiver) {
        if (!(property in target)) {
            return "Unknown setting";
        }

        return Reflect.get(target, property, receiver);
    }
});

console.log(proxy.theme);
// dark

console.log(proxy.language);
// Unknown setting
```

# 🗁 Reflection with Reflect

`Reflect` provides methods for performing fundamental object operations.

It is especially useful when implementing proxy traps because it provides standardized ways to delegate operations.

Example:

```javascript
const user = {
    name: "Alice",
    age: 25
};

console.log(Reflect.get(user, "name"));
// Alice

Reflect.set(user, "age", 30);

console.log(Reflect.get(user, "age"));
// 30
```

Common methods include:

* `Reflect.get()`
* `Reflect.set()`
* `Reflect.has()`
* `Reflect.deleteProperty()`
* `Reflect.ownKeys()`
* `Reflect.apply()`
* `Reflect.construct()`
* `Reflect.defineProperty()`
* `Reflect.getPrototypeOf()`
* `Reflect.setPrototypeOf()`

# 🗁 Symbols in Metaprogramming

Symbols provide unique property keys and well-known identifiers that customize language behavior.

Example:

```javascript
const userId = Symbol("userId");

const user = {
    name: "Alice",
    [userId]: 1001
};

console.log(user[userId]);
// 1001
```

Well-known symbols provide deeper control over JavaScript operations.

Examples:

* `Symbol.iterator`: Defines default iteration behavior.
* `Symbol.asyncIterator`: Defines asynchronous iteration behavior.
* `Symbol.toPrimitive`: Controls primitive conversion.
* `Symbol.toStringTag`: Customizes object tags.
* `Symbol.hasInstance`: Customizes `instanceof`.
* `Symbol.species`: Customizes constructors used by certain built-in methods.

# 🗁 Custom Iteration with Symbol.iterator

An object can customize its iteration behavior by implementing `Symbol.iterator`.

Example:

```javascript
const collection = {
    items: ["JavaScript", "TypeScript", "Python"],

    *[Symbol.iterator]() {
        for (const item of this.items) {
            yield item;
        }
    }
};

for (const language of collection) {
    console.log(language);
}
```

Output:

```text
JavaScript
TypeScript
Python
```

This allows custom objects to work with `for...of`, spread syntax, and other iterable-consuming operations.

# 🗁 Dynamic Function Creation

JavaScript provides the `Function` constructor for creating functions dynamically at runtime.

Syntax:

```javascript
new Function(parameter1, parameter2, functionBody);
```

Example:

```javascript
const add = new Function(
    "a",
    "b",
    "return a + b"
);

console.log(add(10, 20));
// 30
```

The function body is provided as a string.

Unlike ordinary functions, functions created using the `Function` constructor do not capture the surrounding lexical scope.

```javascript
const multiplier = 10;

const multiply = new Function(
    "number",
    "return number * 10"
);

console.log(multiply(5));
// 50
```

The function can access global bindings but not local lexical variables from its creation location.

# 🗁 Dynamic Code Generation

Dynamic code generation means creating executable behavior from data or strings at runtime.

Example:

```javascript
function createMultiplier(factor) {
    return new Function(
        "number",
        `return number * ${Number(factor)}`
    );
}

const double = createMultiplier(2);

console.log(double(10));
// 20
```

This example converts the supplied factor to a number before embedding it.

Dynamic code generation should generally be avoided when ordinary functions, closures, or lookup tables can solve the problem more clearly.

# 🗁 Template Literals and Code Generation

Template literals make it easier to construct dynamic strings.

Example:

```javascript
const tableName = "users";
const query = `SELECT * FROM ${tableName}`;

console.log(query);
```

Template literals generate strings, not automatically executable code.

They can be used to construct source code, SQL statements, or configuration text, but the resulting content must be handled according to the rules of the target language.

# 🗁 Dynamic Import

Dynamic import loads a JavaScript module at runtime.

It uses the `import()` expression and returns a Promise.

Example:

```javascript
async function loadModule() {
    const module = await import("./calculator.js");

    console.log(module.add(10, 20));
}

loadModule();
```

Dynamic imports are useful for:

* Lazy loading
* Code splitting
* Conditional module loading
* Reducing initial application loading costs
* Loading optional functionality

# 🗁 Function Metadata and Inspection

JavaScript provides some runtime information about functions.

Example:

```javascript
function greet(name) {
    return `Hello, ${name}`;
}

console.log(greet.name);
// greet

console.log(greet.length);
// 1

console.log(typeof greet);
// function
```

The `name` property generally provides the function's name.

The `length` property reports the number of parameters before the first default parameter, excluding rest parameters.

These properties provide limited metadata, not a complete representation of a function's source or behavior.

# 🗁 Custom Object Behavior

Metaprogramming allows objects to define custom behavior through language features.

Example using `Symbol.toPrimitive`:

```javascript
const product = {
    name: "Laptop",
    price: 1200,

    [Symbol.toPrimitive](hint) {
        if (hint === "number") {
            return this.price;
        }

        return this.name;
    }
};

console.log(Number(product));
// 1200

console.log(String(product));
// Laptop
```

The object now controls how it is converted into primitive values.

# 🗁 Practical Example: Object Validation

A proxy can validate assignments before modifying an object.

Example:

```javascript
function createValidatedUser(user) {
    return new Proxy(user, {
        set(target, property, value, receiver) {
            if (property === "age") {
                if (!Number.isInteger(value)) {
                    throw new TypeError(
                        "Age must be an integer"
                    );
                }

                if (value < 0) {
                    throw new RangeError(
                        "Age cannot be negative"
                    );
                }
            }

            return Reflect.set(
                target,
                property,
                value,
                receiver
            );
        }
    });
}

const user = createValidatedUser({
    name: "Alice",
    age: 25
});

user.age = 30;

console.log(user.age);
// 30
```

# 🗁 Practical Example: Automatic Property Logging

A proxy can automatically record object property access and modification.

Example:

```javascript
function createLogger(target) {
    return new Proxy(target, {
        get(target, property, receiver) {
            console.log(
                `Reading: ${String(property)}`
            );

            return Reflect.get(
                target,
                property,
                receiver
            );
        },

        set(target, property, value, receiver) {
            console.log(
                `Updating: ${String(property)}`
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

const user = createLogger({
    name: "Alice",
    age: 25
});

console.log(user.name);

user.age = 30;
```

# 🗁 Practical Example: Reactive State

Reactive systems can use proxies to detect changes in application state.

A simplified example:

```javascript
function reactive(target) {
    return new Proxy(target, {
        set(target, property, value, receiver) {
            const previousValue = Reflect.get(
                target,
                property,
                receiver
            );

            const success = Reflect.set(
                target,
                property,
                value,
                receiver
            );

            if (success && !Object.is(previousValue, value)) {
                console.log(
                    `${String(property)} changed`
                );
            }

            return success;
        }
    });
}

const state = reactive({
    count: 0
});

state.count = 1;
state.count = 2;
```

Output:

```text
count changed
count changed
```

Real reactive frameworks require additional mechanisms for dependency tracking, nested objects, scheduling, and cleanup.

# 🗁 Practical Example: Dynamic Method Execution

A generic dispatcher can execute an object method using a method name supplied at runtime.

Example:

```javascript
const calculator = {
    add(a, b) {
        return a + b;
    },

    subtract(a, b) {
        return a - b;
    },

    multiply(a, b) {
        return a * b;
    }
};

function executeMethod(object, method, ...args) {
    if (
        typeof object[method] !== "function" ||
        !Object.hasOwn(object, method)
    ) {
        throw new Error("Invalid method");
    }

    return Reflect.apply(
        object[method],
        object,
        args
    );
}

console.log(executeMethod(calculator, "add", 10, 20));
// 30
```

This pattern is useful for controlled dynamic dispatch.

# 🗁 Metaprogramming in JavaScript Frameworks

Metaprogramming is widely used inside JavaScript frameworks and libraries.

**Reactive Frameworks**

Frameworks can use proxies to observe state changes and track dependencies.

**ORM Libraries**

Object-relational mapping libraries can intercept property access and manage database-backed objects.

**Validation Libraries**

Validation systems can inspect object properties and enforce constraints.

**Dependency Injection**

Frameworks can inspect constructors, metadata, or registration information to resolve dependencies.

**Testing Libraries**

Mocking systems can intercept function calls and object operations to control test behavior.

# 🗁 Metaprogramming vs Regular Programming

| Feature | Regular Programming | Metaprogramming |
| --- | --- | --- |
| Main purpose | Implement application logic | Control or inspect program behavior |
| Focus | Data and operations | Code and runtime structures |
| Property behavior | Usually direct | Can be intercepted |
| Object inspection | Limited or task-specific | Often central |
| Dynamic behavior | Explicitly implemented | Can be customized through language features |
| Typical tools | Functions, loops, objects | Proxy, Reflect, Symbols, descriptors |

# 🗁 Advantages of Metaprogramming

* Enables flexible and reusable abstractions.
* Supports dynamic object behavior.
* Reduces repetitive implementation code.
* Makes advanced framework features possible.
* Provides powerful validation and interception mechanisms.
* Supports reactive programming.
* Allows generic utilities to work with different object structures.

# 🗁 Disadvantages of Metaprogramming

* Can make code difficult to understand.
* May introduce runtime performance overhead.
* Can complicate debugging.
* May hide behavior behind proxies or dynamic dispatch.
* Can make static analysis more difficult.
* Requires careful handling of language invariants.
* Excessive abstraction can make applications harder to maintain.

# 🗁 Best Practices

* Use metaprogramming only when ordinary programming techniques are insufficient.
* Prefer explicit and readable code when possible.
* Use Proxy for operations that genuinely need interception.
* Use Reflect to delegate fundamental object operations.
* Use Symbols for unique property keys and language customization.
* Avoid unnecessary dynamic function construction.
* Validate dynamic inputs before using them in generated code.
* Document non-obvious runtime behavior.
* Test proxy invariants and edge cases.
* Avoid excessive abstraction in application-level code.

# 🗁 Key Takeaways

* Metaprogramming means writing code that inspects or controls other code.
* Introspection examines objects and runtime structures.
* Intercession intercepts and customizes operations.
* Proxy provides operation interception through traps.
* Reflect provides standardized object-operation methods.
* Symbols enable unique property keys and custom language behavior.
* Property descriptors control property characteristics.
* Dynamic imports support runtime module loading.
* The Function constructor enables dynamic function creation.
* Metaprogramming is fundamental to many frameworks and libraries.
* Powerful metaprogramming should be used carefully to preserve readability and maintainability.

# 🗁 Related Topics

* Objects
* Property Descriptors
* Getters and Setters
* Classes
* Prototypes
* Proxy
* Reflect
* Symbols
* Iterators
* Generators
* Dynamic Import
* Functional Programming
* Reactive Programming
* Framework Architecture
