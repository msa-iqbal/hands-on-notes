# JavaScript Lexical Environment

A **Lexical Environment** is an internal JavaScript mechanism that stores identifier bindings and maintains a reference to an outer lexical environment.

It is responsible for managing how JavaScript identifies and accesses variables and functions according to their position in the source code.

Lexical environments are fundamental to understanding:

* Scope
* Variable lookup
* Lexical scope
* Closures
* Hoisting
* Block scope
* Function scope
* The scope chain

JavaScript uses lexical environments to determine which variable a particular identifier refers to.

# 🗁 What Is a Lexical Environment?

A lexical environment represents the relationship between the code being executed and the variables and functions accessible from that code.

Consider:

```javascript
const name = "Alice";

function greet() {
    const message = "Hello";

    console.log(message);
    console.log(name);
}

greet();
```

Inside `greet()`:

* `message` is found in the function's own lexical environment.
* `name` is found in an outer lexical environment.
* Both identifiers are accessible because of lexical scope.

JavaScript searches the current environment first and then follows references to outer environments.

# 🗁 Components of a Lexical Environment

A lexical environment consists of two main components:

* Environment Record
* Outer Environment Reference

Conceptual structure:

```text
Lexical Environment
    |
    ├── Environment Record
    |       |
    |       ├── Variable Bindings
    |       ├── Function Bindings
    |       └── Other Identifier Bindings
    |
    └── Outer Environment Reference
            |
            └── Parent Lexical Environment
```

# 🗁 Environment Record

An **Environment Record** is an internal specification mechanism that stores identifier bindings associated with a particular lexical environment.

It manages the relationship between identifiers and their corresponding values or declarations.

For example:

```javascript
const username = "Alice";
let age = 25;

function greet() {
    const message = "Hello";
}
```

Conceptually, the global environment record contains bindings for `username`, `age`, and `greet`.

When `greet()` executes, its function environment contains the local binding for `message`.

Environment records are specification-level structures. Developers do not directly create or manipulate them through ordinary JavaScript syntax.

# 🗁 Types of Environment Records

ECMAScript defines several types of environment records.

### 🖆 Declarative Environment Record

A **Declarative Environment Record** stores bindings created by declarations such as:

* `let`
* `const`
* Class declarations
* Function declarations in relevant lexical scopes

Example:

```javascript
function calculate() {
    let first = 10;
    const second = 20;

    return first + second;
}

calculate();
```

The function's environment manages the local bindings `first` and `second`.

Block-scoped declarations also use declarative environment records.

```javascript
{
    let message = "Hello";
    const language = "JavaScript";

    console.log(message);
}
```

The block creates a lexical scope containing these bindings.

### 🖆 Object Environment Record

An **Object Environment Record** associates identifier bindings with properties of an object.

It is used in certain environments, including global environments and `with` statements.

Example:

```javascript
var username = "Alice";

console.log(globalThis.username);
```

In a classic browser script, a top-level `var` declaration can create a property on the global object.

Object environment records are also relevant to understanding how `with` works, although `with` is prohibited in strict mode.

### 🖆 Function Environment Record

A **Function Environment Record** is a specialized declarative environment record used for function execution.

It manages function-related information such as:

* Function parameters
* Local variable bindings
* The `this` binding
* The `super` binding where applicable
* The function's `new.target` value

Example:

```javascript
function greet(name) {
    const message = `Hello, ${name}`;

    return message;
}

greet("Alice");
```

The function environment manages the parameter `name` and local binding `message`.

### 🖆 Global Environment Record

A **Global Environment Record** represents the global scope of a script or realm.

It combines declarative and object-related environment record behavior.

It manages global declarations and provides access to the global object.

Example:

```javascript
var username = "Alice";
let age = 25;

function greet() {
    console.log("Hello");
}
```

In a classic browser script:

* `username` can become a property of `window`.
* `age` does not become a property of `window`.
* The global function declaration can become a global object property.

```javascript
console.log(window.username); // Alice
console.log(window.age);      // undefined
```

These global object property behaviors differ for JavaScript modules.

### 🖆 Module Environment Record

A **Module Environment Record** manages bindings declared within an ECMAScript module.

Modules have their own top-level lexical scope.

Example:

```javascript
// user.js

export const username = "Alice";

export function greet() {
    console.log(`Hello, ${username}`);
}
```

The module environment manages the module's local bindings and exported bindings.

Imported bindings are live bindings, meaning they reflect updates to the exported binding rather than acting as independent copied values.

# 🗁 Outer Environment Reference

The **Outer Environment Reference** connects a lexical environment to its enclosing lexical environment.

It allows JavaScript to resolve identifiers outside the current scope.

Example:

```javascript
const globalValue = "Global";

function outer() {
    const outerValue = "Outer";

    function inner() {
        const innerValue = "Inner";

        console.log(innerValue);
        console.log(outerValue);
        console.log(globalValue);
    }

    inner();
}

outer();
```

The `inner()` function has access to:

* Its own environment.
* The environment of `outer()`.
* The global environment.

Conceptual structure:

```text
Global Environment
       |
       ▼
Outer Function Environment
       |
       ▼
Inner Function Environment
```

The outer reference establishes the environment chain used during identifier resolution.

# 🗁 Lexical Environment and Scope

A lexical environment provides the underlying mechanism for JavaScript's lexical scoping behavior.

**Lexical Scope** means that the accessibility of variables is determined by where functions and blocks are written in the source code.

Example:

```javascript
const country = "Bangladesh";

function showCountry() {
    const city = "Dhaka";

    function display() {
        console.log(country);
        console.log(city);
    }

    display();
}

showCountry();
```

The `display()` function can access `country` and `city` because its lexical environment is connected to the environments in which it was defined.

Moving a function call to a different location does not change its lexical scope.

# 🗁 Lexical Environment and Scope Chain

The **Scope Chain** describes the process of searching through the current lexical environment and its outer environments to resolve an identifier.

Example:

```javascript
const first = "Global";

function outer() {
    const second = "Outer";

    function inner() {
        const third = "Inner";

        console.log(third);
        console.log(second);
        console.log(first);
    }

    inner();
}

outer();
```

Output:

```text
Inner
Outer
Global
```

Identifier lookup follows this conceptual sequence:

* Search the current environment.
* If the identifier is not found, search the outer environment.
* Continue through the outer environment references.
* If no matching binding exists, produce a `ReferenceError`.

Example of an unresolved identifier:

```javascript
function showMessage() {
    console.log(message);
}

showMessage();
```

Output:

```text
ReferenceError: message is not defined
```

# 🗁 Lexical Environment and Block Scope

A new lexical environment is created for blocks that introduce lexical declarations.

The declarations `let` and `const` are block-scoped.

Example:

```javascript
{
    let username = "Alice";
    const age = 25;

    console.log(username);
    console.log(age);
}

console.log(username);
```

Output:

```text
Alice
25
ReferenceError
```

The bindings `username` and `age` are accessible only inside the block.

By contrast, `var` is not block-scoped.

```javascript
{
    var username = "Alice";
}

console.log(username); // Alice
```

The `var` declaration belongs to the surrounding function or global scope rather than the block's lexical scope.

# 🗁 Lexical Environment and Closures

A **Closure** occurs when a function retains access to lexical bindings from its surrounding environment, even after the outer function has completed execution.

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
console.log(counter()); // 3
```

The `increment()` function retains access to the `count` binding from the lexical environment created during `createCounter()`.

Although `createCounter()` has returned, its lexical environment remains reachable through the closure.

This demonstrates an important distinction:

* An execution context can leave the call stack.
* A lexical environment can remain accessible while references to its bindings are retained.

# 🗁 Lexical Environment and Hoisting

Lexical environments are involved in declaration instantiation and initialization.

Different declaration types have different initialization behavior.

### 🖆 var

A `var` binding is initialized with `undefined` during declaration instantiation.

```javascript
console.log(username);

var username = "Alice";
```

Output:

```text
undefined
```

### 🖆 let

A `let` binding is created but remains uninitialized until its declaration is evaluated.

```javascript
console.log(age);

let age = 25;
```

Output:

```text
ReferenceError
```

### 🖆 const

A `const` binding also remains uninitialized until its declaration is evaluated.

```javascript
console.log(country);

const country = "Bangladesh";
```

Output:

```text
ReferenceError
```

The period before initialization is known as the **Temporal Dead Zone (TDZ)**.

# 🗁 Lexical Environment vs Execution Context

These concepts are closely related but represent different things.

| Lexical Environment | Execution Context |
| --- | --- |
| Manages identifier bindings | Represents the environment and state of code execution |
| Contains an environment record | Contains references to execution-related environments |
| Maintains an outer environment reference | Maintains execution state such as the code being evaluated |
| Supports scope resolution | Manages execution of global code and function calls |
| Can remain reachable after a function returns | Leaves the active call stack when execution completes |

An execution context uses lexical environments to manage identifier bindings and scope resolution.

# 🗁 Practical Example

Consider this example:

```javascript
const application = "Hands-on-Notes";

function createApplication() {
    const version = "1.0";

    function displayApplication() {
        const status = "Running";

        console.log(application);
        console.log(version);
        console.log(status);
    }

    return displayApplication;
}

const runApplication = createApplication();

runApplication();
```

Output:

```text
Hands-on-Notes
1.0
Running
```

Explanation:

* The global environment contains `application` and `createApplication`.
* Calling `createApplication()` creates a function environment.
* The function environment contains `version` and `displayApplication`.
* `displayApplication` is defined inside `createApplication`.
* The returned function retains access to the environment containing `version`.
* Calling `runApplication()` creates a new function execution context.
* Identifier lookup resolves `status`, `version`, and `application` through the lexical environment chain.

This example demonstrates lexical scope and closures working together.

# 🗁 Common Misconceptions

**A lexical environment and an execution context are the same thing.**

Incorrect. An execution context manages code execution, while a lexical environment manages identifier bindings and outer environment references.

**A function uses the scope where it is called.**

Incorrect. A function normally resolves identifiers using its lexical definition environment, not the location from which it is called.

**All variables are stored in the global environment.**

Incorrect. Variables are associated with the environment corresponding to their declaration scope.

**A function's lexical environment disappears immediately after it returns.**

Incorrect. An environment may remain reachable when a closure retains access to its bindings.

**The scope chain is a separate object that developers can access.**

Incorrect. The scope chain describes identifier resolution through linked lexical environments. It is not an ordinary JavaScript object exposed to application code.

# 🗁 Best Practices

* Understand lexical scope before studying closures.
* Prefer `let` and `const` for block-scoped declarations.
* Keep variable declarations close to where they are used.
* Avoid unnecessary global variables.
* Understand the difference between lexical scope and dynamic execution.
* Use closures intentionally when preserving private state.
* Be aware that closures can keep objects and other values reachable.
* Avoid relying on implementation-specific internal environment structures.

# 🗁 Key Takeaways

* A lexical environment stores identifier bindings and an outer environment reference.
* Environment records manage variables, functions, and other declarations.
* JavaScript defines several environment record types.
* Outer environment references establish the scope chain.
* Identifier resolution starts in the current lexical environment.
* Lexical scope depends on where code is defined.
* Blocks can create new lexical environments.
* Closures preserve access to bindings from surrounding environments.
* Lexical environments are essential to understanding hoisting and the temporal dead zone.

# 🗁 Related Topics

* Execution Context
* Call Stack
* Scope
* Global Scope
* Function Scope
* Block Scope
* Lexical Scope
* Closures
* Hoisting
* Temporal Dead Zone
* Execution Model
