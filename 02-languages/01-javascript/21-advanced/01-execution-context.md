# JavaScript Execution Context

An **Execution Context** is the environment in which JavaScript code is evaluated and executed.

Whenever JavaScript runs code, the JavaScript engine creates an execution context to manage:

* Variables and function declarations
* Scope and identifier resolution
* The value of `this`
* The execution state of the current code
* References to the outer lexical environment

Execution contexts are fundamental to understanding JavaScript's runtime behavior, including hoisting, closures, scope, and the call stack.

# 🗁 What Happens When JavaScript Executes?

JavaScript does not execute an entire program without preparation. The engine creates execution contexts and manages them as code runs.

Consider:

```javascript
const name = "Alice";

function greet() {
    const message = `Hello, ${name}`;
    console.log(message);
}

greet();
```

Conceptually, the engine performs these operations:

* Creates the global execution context.
* Initializes the global environment.
* Registers declarations and initializes bindings according to their declaration types.
* Executes the global code.
* Creates a function execution context when `greet()` is called.
* Executes the function body.
* Returns to the previous execution context when the function finishes.

# 🗁 Types of Execution Contexts

JavaScript has three commonly discussed execution-context categories.

### 🖆 Global Execution Context

The **Global Execution Context (GEC)** is created when JavaScript begins executing a script.

It provides the environment for code that is not currently running inside a function.

Example:

```javascript
const language = "JavaScript";

function showLanguage() {
    console.log(language);
}

showLanguage();
```

The global execution context manages the global-level declarations and execution state.

In a browser:

* The global object is `window`.
* Top-level `var` declarations in classic scripts can become properties of `window`.
* Top-level `let` and `const` declarations do not become properties of `window`.

Example:

```javascript
var username = "Alice";
let age = 25;

console.log(window.username); // Alice
console.log(window.age);      // undefined
```

This distinction applies to classic browser scripts. JavaScript modules have their own top-level scope and do not expose top-level declarations as global object properties.

In Node.js, the global environment differs from the browser environment.

### 🖆 Function Execution Context

A **Function Execution Context (FEC)** is created whenever a function is invoked.

Each function invocation receives its own execution context, including its own local bindings and execution state.

Example:

```javascript
function calculate() {
    const first = 10;
    const second = 20;

    return first + second;
}

calculate();
```

When `calculate()` is invoked, JavaScript creates a function execution context that manages its local variables and execution.

Each invocation creates a separate execution context.

```javascript
function counter() {
    let count = 0;

    count++;

    console.log(count);
}

counter(); // 1
counter(); // 1
```

Each invocation initializes a new local `count` binding.

### 🖆 Eval Execution Context

An **Eval Execution Context** is associated with code evaluated through the `eval()` function.

Example:

```javascript
const message = "Hello";

eval("console.log(message)");
```

Direct `eval()` executes code within the caller's environment, subject to strict-mode and declaration rules.

Indirect `eval()` executes code in the global environment.

Example:

```javascript
const value = 100;

(0, eval)("console.log(typeof value)");
```

The indirect evaluation does not use the local lexical environment of the calling function.

`eval()` is generally discouraged because it complicates scope analysis, debugging, optimization, and security.

# 🗁 Phases of Execution Context

An execution context is commonly explained through two conceptual phases:

* Creation Phase
* Execution Phase

These phases describe how the engine prepares an execution environment and then evaluates its code.

### 🖆 Creation Phase

During context preparation, the engine establishes the environment needed to execute the code.

This includes:

* Creating the appropriate lexical environment.
* Creating the variable environment.
* Establishing bindings for declarations.
* Setting up the `this` binding where applicable.

The exact initialization behavior depends on the declaration type and execution context.

Example:

```javascript
console.log(username);
console.log(age);

var username = "Alice";
let age = 25;
```

Conceptually:

* The `username` binding is created and initialized with `undefined`.
* The `age` binding is created but remains uninitialized until its declaration is evaluated.

Output:

```text
undefined
ReferenceError
```

The second statement throws a `ReferenceError` because `age` is in its temporal dead zone.

### 🖆 Execution Phase

During the execution phase, JavaScript evaluates statements and performs operations.

Example:

```javascript
var username = "Alice";
let age = 25;

function greet() {
    console.log(`Hello, ${username}`);
}

greet();
```

During execution:

* `username` receives `"Alice"`.
* `age` receives `25`.
* `greet()` is invoked.
* A function execution context is created.
* The function body executes.

# 🗁 Components of an Execution Context

An execution context contains several important internal components.

### 🖆 Lexical Environment

A **Lexical Environment** stores identifier bindings and maintains a reference to its outer lexical environment.

It is responsible for resolving variables and functions according to lexical scope.

A lexical environment consists conceptually of:

* Environment Record
* Outer Environment Reference

Example:

```javascript
const globalValue = "Global";

function outer() {
    const outerValue = "Outer";

    function inner() {
        const innerValue = "Inner";

        console.log(globalValue);
        console.log(outerValue);
        console.log(innerValue);
    }

    inner();
}

outer();
```

The `inner()` function can access bindings from its own environment and its outer environments.

### 🖆 Variable Environment

The **Variable Environment** is used to manage bindings introduced by declarations such as `var` within an execution context.

It is closely related to the lexical environment.

A simplified teaching model often describes the variable environment and lexical environment separately because they help explain differences between `var`, `let`, and `const`.

Modern ECMAScript execution contexts have more detailed internal structures than this simplified model.

### 🖆 This Binding

The `this` binding determines the value of `this` inside a function execution context.

Its value depends on how a function is invoked and the kind of function being executed.

Example:

```javascript
const user = {
    name: "Alice",

    greet() {
        console.log(this.name);
    }
};

user.greet(); // Alice
```

Here, `this` refers to the `user` object because `greet()` is called as a method of that object.

Arrow functions do not create their own `this` binding. They inherit `this` from their surrounding lexical environment.

# 🗁 Execution Context and Hoisting

Hoisting describes observable behavior resulting from declaration instantiation and initialization rules.

Different declarations behave differently.

### 🖆 var

A `var` binding is initialized with `undefined` during environment setup.

```javascript
console.log(name);

var name = "Alice";
```

Output:

```text
undefined
```

### 🖆 let and const

Bindings declared with `let` and `const` are created before their declaration is evaluated, but they remain uninitialized during that period.

```javascript
console.log(age);

let age = 25;
```

Output:

```text
ReferenceError
```

This period is called the **Temporal Dead Zone (TDZ)**.

### 🖆 Function Declarations

Function declarations are instantiated so that the function can be called before its textual declaration in the same scope.

```javascript
greet();

function greet() {
    console.log("Hello!");
}
```

Output:

```text
Hello!
```

Function expressions assigned to variables follow the initialization rules of their variable declarations.

```javascript
greet();

var greet = function () {
    console.log("Hello!");
};
```

Output:

```text
TypeError
```

The variable exists and is initialized with `undefined`, so attempting to call it produces a `TypeError`.

# 🗁 Execution Context and Scope Chain

JavaScript resolves identifiers by searching the current lexical environment and then its outer environments.

This process is known as lexical scope resolution.

Example:

```javascript
const country = "Bangladesh";

function outer() {
    const city = "Dhaka";

    function inner() {
        const area = "Gazipur";

        console.log(area);
        console.log(city);
        console.log(country);
    }

    inner();
}

outer();
```

Output:

```text
Gazipur
Dhaka
Bangladesh
```

The identifier lookup follows this conceptual order:

* Current environment
* Outer function environment
* Global environment

If an identifier cannot be found in the accessible environment chain, JavaScript throws a `ReferenceError`.

# 🗁 Execution Context Lifecycle

An execution context follows a lifecycle managed by the JavaScript engine.

The general process is:

* Create an execution context.
* Prepare its environment and bindings.
* Execute its code.
* Return a result when execution completes.
* Remove the context from the active execution stack when it is no longer executing.

A function execution context is generally removed from the call stack after the function returns.

However, variables and functions created during that invocation may remain reachable.

Example:

```javascript
function createCounter() {
    let count = 0;

    return function () {
        count++;

        return count;
    };
}

const counter = createCounter();

console.log(counter()); // 1
console.log(counter()); // 2
console.log(counter()); // 3
```

The execution context of `createCounter()` finishes, but its `count` binding remains accessible through the returned function's closure.

This demonstrates that an execution context leaving the call stack does not necessarily mean all data associated with its execution becomes eligible for garbage collection.

# 🗁 Practical Example

Consider the following program:

```javascript
const globalMessage = "Global";

function first() {
    const firstMessage = "First";

    function second() {
        const secondMessage = "Second";

        console.log(globalMessage);
        console.log(firstMessage);
        console.log(secondMessage);
    }

    second();
}

first();
```

The conceptual execution sequence is:

* The global execution context is created.
* Global declarations are initialized according to their declaration types.
* The global code executes.
* `first()` is invoked.
* A function execution context for `first()` is created.
* `firstMessage` is initialized.
* `second()` is invoked.
* A function execution context for `second()` is created.
* `secondMessage` is initialized.
* The three values are resolved through the lexical environment chain.
* `second()` finishes and its execution context leaves the call stack.
* `first()` finishes and its execution context leaves the call stack.
* Global execution continues.

Output:

```text
Global
First
Second
```

# 🗁 Execution Context vs Call Stack

Execution context and call stack are related but different concepts.

| Execution Context | Call Stack |
| --- | --- |
| Represents the environment for executing code | Tracks active execution contexts |
| Contains execution-related state and environment references | Maintains execution order |
| Created for global code and function calls | Receives contexts as execution begins |
| Manages bindings and `this` | Uses Last In, First Out (LIFO) order |
| Describes where and how code executes | Describes which execution context runs next |

The execution context describes the execution environment, while the call stack manages the sequence of active execution contexts.

# 🗁 Best Practices

* Understand execution contexts before studying advanced JavaScript concepts.
* Prefer `const` and `let` over `var` in modern JavaScript.
* Understand temporal dead zones and declaration initialization.
* Learn lexical scope and closures to understand variable resolution.
* Avoid unnecessary use of `eval()`.
* Use browser developer tools or Node.js debugging tools to inspect execution flow.
* Remember that an execution context leaving the call stack does not automatically destroy every value created during that execution.

# 🗁 Key Takeaways

* An execution context is the environment in which JavaScript code executes.
* JavaScript uses global, function, and eval execution contexts.
* Execution-context preparation establishes environments and bindings before code execution.
* The execution phase evaluates statements and performs operations.
* Lexical environments manage identifier bindings and outer environment references.
* The `this` binding depends on the execution context and invocation rules.
* Hoisting is a consequence of declaration instantiation and initialization behavior.
* Closures can preserve access to variables after a function has finished executing.
* The call stack manages the order of active execution contexts.

# 🗁 Related Topics

* JavaScript Scope
* Lexical Environment
* Hoisting
* Temporal Dead Zone
* Closures
* Call Stack
* Event Loop
* Execution Model
* Memory Management
