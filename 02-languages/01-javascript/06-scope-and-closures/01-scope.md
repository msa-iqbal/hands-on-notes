# Scope

**Scope** determines where variables and functions can be accessed within a JavaScript program.

It defines the visibility and accessibility of identifiers such as:

* Variables declared with `var`, `let`, and `const`.
* Function declarations and expressions.
* Parameters.
* Classes.
* Other identifiers declared within a particular environment.

Understanding scope is essential for writing predictable, maintainable, and secure JavaScript applications.

# 🗁 What Is Scope?

Scope is the region of a program where a variable or function is accessible.

```javascript
const message = "Hello, JavaScript!";

function greet() {
  console.log(message);
}

greet(); // Hello, JavaScript!
```

The variable `message` is accessible inside `greet()` because it is declared in an outer scope.

However, variables declared inside a function are generally unavailable outside that function.

```javascript
function greet() {
  const message = "Hello!";
  console.log(message);
}

greet(); // Hello!

console.log(message); // ReferenceError
```

The variable `message` belongs to the function's scope.

# 🗁 Why Scope Matters

Scope provides several important benefits:

* **Encapsulation:** Keeps internal variables isolated.
* **Data protection:** Prevents unintended access to local variables.
* **Namespace management:** Reduces naming conflicts.
* **Maintainability:** Makes code easier to understand and modify.
* **Predictability:** Defines where identifiers can be accessed.
* **Memory management:** Allows inaccessible environments to become eligible for garbage collection when no longer referenced.

# 🗁 Types of Scope in JavaScript

JavaScript commonly uses the following scope categories:

| Scope | Description |
| --- | --- |
| Global scope | Accessible throughout the relevant script or execution environment |
| Function scope | Accessible within the function where it is declared |
| Block scope | Accessible within the block where it is declared |
| Module scope | Accessible within the module where it is declared |

JavaScript also uses **lexical scope**, which describes how nested functions resolve identifiers through their surrounding source-code structure.

# 🗁 Global Scope

A variable declared outside functions, blocks, and modules may belong to the global scope.

```javascript
const applicationName = "My Application";

function showApplicationName() {
  console.log(applicationName);
}

showApplicationName(); // My Application

console.log(applicationName); // My Application
```

Global variables can be accessed from different functions within the same relevant scope environment.

In browser classic scripts, top-level `var` declarations create properties on the global object, while top-level `let` and `const` declarations do not.

```javascript
var first = "JavaScript";
let second = "TypeScript";
const third = "Python";

console.log(globalThis.first);  // JavaScript
console.log(globalThis.second); // undefined
console.log(globalThis.third);  // undefined
```

This example applies to classic browser scripts. ES modules have different global declaration behavior.

# 🗁 Function Scope

Variables declared with `var`, `let`, and `const` inside a function are accessible within that function.

```javascript
function calculate() {
  const firstNumber = 10;
  const secondNumber = 20;

  var result = firstNumber + secondNumber;

  console.log(result); // 30
}

calculate();

console.log(result); // ReferenceError
```

The variables belong to the function's local scope.

# 🗁 Block Scope

A block is a section of code enclosed by curly braces `{}`.

Variables declared with `let` and `const` are block-scoped.

```javascript
{
  let message = "Inside block";
  const language = "JavaScript";

  console.log(message);
  console.log(language);
}

console.log(message); // ReferenceError
console.log(language); // ReferenceError
```

A block can appear in:

* `if` statements.
* `for` loops.
* `while` loops.
* `switch` statements.
* Standalone curly-brace blocks.

The `var` keyword does not provide block scope.

```javascript
{
  var first = "Accessible outside";
  let second = "Block scoped";
}

console.log(first);  // Accessible outside
console.log(second); // ReferenceError
```

# 🗁 Module Scope

JavaScript modules have their own scope.

Variables declared at the top level of an ES module are not automatically added to the global object or made available to other modules.

```javascript
// app.js

const applicationName = "My Application";

export function initialize() {
  console.log(`${applicationName} initialized`);
}
```

Another module can access the exported function:

```javascript
// main.js

import { initialize } from "./app.js";

initialize();
```

The variable `applicationName` remains private to `app.js` unless explicitly exported.

# 🗁 Lexical Scope

JavaScript uses lexical scoping, meaning that variable accessibility is determined by where functions and blocks are written in the source code.

```javascript
const language = "JavaScript";

function outerFunction() {
  const framework = "React";

  function innerFunction() {
    const library = "Redux";

    console.log(language);
    console.log(framework);
    console.log(library);
  }

  innerFunction();
}

outerFunction();
```

The inner function can access variables from its own scope and surrounding lexical scopes.

This is also the foundation of closures.

# 🗁 Scope Chain

When JavaScript encounters an identifier, it searches for that identifier through the current lexical environment and its outer environments.

This process is called the **scope chain**.

```javascript
const globalVariable = "Global";

function outer() {
  const outerVariable = "Outer";

  function inner() {
    const innerVariable = "Inner";

    console.log(innerVariable);
    console.log(outerVariable);
    console.log(globalVariable);
  }

  inner();
}

outer();
```

The lookup order is:

* Current scope.
* Immediately enclosing scope.
* Next outer scope.
* Global scope, where applicable.

If the identifier cannot be found, JavaScript generally throws a `ReferenceError`.

# 🗁 Variable Shadowing

Variable shadowing occurs when a variable declared in an inner scope has the same name as a variable in an outer scope.

```javascript
const message = "Global message";

function showMessage() {
  const message = "Local message";

  console.log(message);
}

showMessage(); // Local message

console.log(message); // Global message
```

The inner variable shadows the outer variable within its scope.

The outer variable remains unchanged.

# 🗁 Scope and Variable Declarations

Different declaration keywords have different scope behaviors.

| Keyword | Scope | Redeclaration in Same Scope | Reassignment |
| --- | --- | --- | --- |
| `var` | Function or global | Allowed | Allowed |
| `let` | Block | Not allowed | Allowed |
| `const` | Block | Not allowed | Not allowed |

Example:

```javascript
function example() {
  var first = 10;
  let second = 20;
  const third = 30;

  first = 15;
  second = 25;

  // third = 35; // TypeError

  console.log(first, second, third);
}

example();
```

Note that `const` prevents reassignment of the binding, not mutation of an object or array referenced by that binding.

# 🗁 Scope and Closures

Closures allow functions to retain access to variables from their surrounding lexical environment, even after the outer function has finished executing.

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

The returned function retains access to `count`.

This behavior is called a closure and is widely used for private state, callbacks, and function factories.

# 🗁 Scope and Hoisting

Hoisting describes how JavaScript processes declarations before executing code.

Different declarations behave differently.

```javascript
console.log(first); // undefined

var first = 10;
```

A `var` declaration is initialized with `undefined` during its declaration processing.

With `let` and `const`, accessing the variable before its declaration is initialized results in a `ReferenceError`.

```javascript
console.log(second); // ReferenceError

let second = 20;
```

The period between entering the scope and initializing a lexical declaration is known as the **Temporal Dead Zone (TDZ)**.

# 🗁 Common Scope Errors

**Accessing a Local Variable Outside Its Scope**

```javascript
function example() {
  let value = 100;
}

console.log(value); // ReferenceError
```

**Using `var` When Block Scope Is Required**

```javascript
if (true) {
  var status = "Active";
}

console.log(status); // Active
```

Using `let` or `const` would keep the variable inside the block.

**Accidental Global Variables**

In non-strict JavaScript, assigning to an undeclared identifier can create a global property in certain environments.

```javascript
function example() {
  accidentalGlobal = "Unexpected global";
}

example();

console.log(globalThis.accidentalGlobal);
```

Strict mode and ES modules prevent this implicit global assignment by throwing a `ReferenceError`.

**Unexpected Variable Shadowing**

```javascript
const total = 100;

function calculate() {
  const total = 50;

  console.log(total); // 50
}

calculate();
```

The inner declaration hides the outer variable within the function.

# 🗁 Best Practices

* Prefer `const` by default.
* Use `let` when reassignment is necessary.
* Avoid `var` in modern JavaScript unless there is a specific reason to use its function-scoping behavior.
* Keep variables in the narrowest scope necessary.
* Avoid unnecessary global variables.
* Use descriptive names to reduce accidental shadowing.
* Use modules to organize application-level scope.
* Understand lexical scope when working with callbacks and closures.
* Avoid relying on implicit global variables.
* Keep functions small and their dependencies explicit.

# 🗁 Summary

* Scope defines where variables and functions can be accessed.
* JavaScript supports global, function, block, and module scope.
* JavaScript uses lexical scoping to resolve identifiers.
* The scope chain determines how JavaScript searches for variables.
* Variable shadowing occurs when an inner declaration hides an outer declaration.
* `var` is function-scoped, while `let` and `const` are block-scoped.
* Closures preserve access to surrounding lexical variables.
* Understanding scope is fundamental to understanding closures, hoisting, and execution contexts.

# 🗁 Practice Exercises

* Create a global variable and access it from multiple functions.
* Demonstrate function scope using `var`, `let`, and `const`.
* Create a block and compare the accessibility of variables declared with different keywords.
* Demonstrate variable shadowing using nested functions.
* Create three nested functions and access variables from each outer scope.
* Demonstrate the difference between global scope and module scope.
* Create a closure that maintains a private counter.
* Demonstrate a `ReferenceError` caused by accessing a variable outside its scope.
* Compare `var`, `let`, and `const` inside a loop.
* Explain how the scope chain resolves variables in a nested function.
