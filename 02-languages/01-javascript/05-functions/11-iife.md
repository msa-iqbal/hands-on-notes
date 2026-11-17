# Immediately Invoked Function Expression (IIFE)

An **Immediately Invoked Function Expression (IIFE)** is a JavaScript function expression that executes immediately after it is defined.

IIFEs are commonly used to:

* Create isolated scopes.
* Prevent global namespace pollution.
* Encapsulate private variables and functions.
* Execute initialization logic.
* Avoid exposing internal implementation details.

An IIFE is defined and invoked in a single expression.

# 🗁 Basic IIFE Syntax

A basic IIFE uses a function expression wrapped in parentheses, followed by an invocation operator `()`.

```javascript
(function () {
  console.log("Hello from IIFE!");
})();
```

Output:

```text
Hello from IIFE!
```

The structure consists of:

* `(function () { ... })` — defines a function expression.
* `()` — immediately invokes the function.

# 🗁 Why Parentheses Are Required

JavaScript distinguishes between function declarations and function expressions.

A function declaration cannot be invoked immediately using the same syntax without first being assigned or referenced.

```javascript
function greet() {
  console.log("Hello!");
}

// This is a function declaration.
greet();
```

Wrapping a function in parentheses makes it an expression.

```javascript
(function greet() {
  console.log("Hello!");
})();
```

The parentheses tell the JavaScript parser to interpret the function as an expression rather than a declaration.

# 🗁 Common IIFE Syntax Variations

There are several valid ways to write an IIFE.

**Function Expression**

```javascript
(function () {
  console.log("Function expression IIFE");
})();
```

**Named Function Expression**

```javascript
(function initialize() {
  console.log("Named IIFE");
})();
```

The function name is available inside the function itself but is not added to the surrounding scope.

**Arrow Function IIFE**

```javascript
(() => {
  console.log("Arrow function IIFE");
})();
```

**Async IIFE**

An async IIFE allows `await` to be used inside the function.

```javascript
(async () => {
  const response = await fetch(
    "https://jsonplaceholder.typicode.com/users"
  );

  const users = await response.json();

  console.log(users);
})();
```

**Unary Operator IIFE**

Other operators can also force a function declaration-like expression into an expression context.

```javascript
!function () {
  console.log("IIFE using unary operator");
}();
```

Although valid, the parenthesized form is generally easier to recognize and maintain.

# 🗁 Passing Arguments to an IIFE

An IIFE can accept arguments just like any other function.

```javascript
(function (name) {
  console.log(`Hello, ${name}!`);
})("Muhammad");
```

Output:

```text
Hello, Muhammad!
```

Arguments are passed to the function immediately after its definition.

# 🗁 Returning Values from an IIFE

An IIFE can return a value that can be assigned to a variable.

```javascript
const result = (function () {
  const firstNumber = 10;
  const secondNumber = 20;

  return firstNumber + secondNumber;
})();

console.log(result); // 30
```

The returned value is assigned to `result`, while the internal variables remain local to the function.

# 🗁 IIFE and Scope Isolation

One of the traditional purposes of IIFEs is creating a private function scope.

```javascript
(function () {
  const message = "This variable is private";

  console.log(message);
})();

console.log(typeof message); // "undefined"
```

The `message` variable exists only inside the IIFE.

This prevents internal variables from becoming global variables.

# 🗁 IIFE for Data Encapsulation

An IIFE can create private state and expose only selected operations.

```javascript
const counter = (function () {
  let count = 0;

  return {
    increment() {
      count++;
      return count;
    },

    decrement() {
      count--;
      return count;
    },

    getValue() {
      return count;
    },
  };
})();

console.log(counter.increment()); // 1
console.log(counter.increment()); // 2
console.log(counter.decrement()); // 1
console.log(counter.getValue());  // 1
```

The `count` variable cannot be accessed directly from outside the IIFE.

```javascript
console.log(counter.count); // undefined
```

The returned methods retain access to `count` through closures.

This pattern is often called the **Module Pattern**.

# 🗁 IIFE and the Global Namespace

Before modern JavaScript modules, applications often used global variables to share functionality.

Too many global variables can cause naming conflicts.

An IIFE helps prevent this problem.

```javascript
(function () {
  const appName = "My Application";

  function initialize() {
    console.log(`${appName} initialized`);
  }

  initialize();
})();
```

The internal variables and functions do not become global properties.

An IIFE can also expose a controlled public interface:

```javascript
const App = (function () {
  const version = "1.0.0";

  function initialize() {
    console.log("Application initialized");
  }

  return {
    initialize,
    getVersion() {
      return version;
    },
  };
})();

App.initialize();
console.log(App.getVersion());
```

Only the returned interface is accessible through `App`.

# 🗁 IIFE vs Regular Function

| Feature | Regular Function | IIFE |
| --- | --- | --- |
| Execution | Called when needed | Invoked immediately after definition |
| Reusability | Can be called multiple times | Usually designed for one-time execution |
| Scope | Has its own function scope | Has its own function scope |
| Arguments | Supports arguments | Supports arguments |
| Return value | Can return values | Can return values |
| Common purpose | Reusable operations | Initialization and encapsulation |

# 🗁 IIFE vs Block Scope

Modern JavaScript provides block scope through `let` and `const`.

This reduces the need for IIFEs when the only goal is variable isolation.

Using an IIFE:

```javascript
(function () {
  const message = "Private scope";
  console.log(message);
})();
```

Using a block:

```javascript
{
  const message = "Block scope";
  console.log(message);
}
```

Both approaches isolate `message` from the surrounding scope.

However, an IIFE provides additional function capabilities, such as parameters, return values, and closures.

# 🗁 IIFE in Modern JavaScript

IIFEs remain valid JavaScript, but their usage has changed with modern language features.

**ES Modules**

ES modules provide their own module scope, reducing the need for IIFEs to avoid global namespace pollution.

```javascript
// app.js

const applicationName = "My Application";

export function initialize() {
  console.log(`${applicationName} initialized`);
}
```

**Block Scope**

`let` and `const` provide block-scoped variables without requiring an additional function.

**Async Initialization**

Async IIFEs remain useful when asynchronous initialization is needed in a context where top-level `await` is unavailable.

```javascript
(async function () {
  try {
    const response = await fetch("/api/config");
    const config = await response.json();

    console.log(config);
  } catch (error) {
    console.error("Initialization failed:", error);
  }
})();
```

In ES modules, top-level `await` can often eliminate the need for an async IIFE.

# 🗁 Common Mistakes

**Forgetting the Invocation Parentheses**

```javascript
(function () {
  console.log("Hello!");
});
```

This defines a function expression but does not execute it.

Correct:

```javascript
(function () {
  console.log("Hello!");
})();
```

**Incorrectly Treating an IIFE as Reusable**

An IIFE executes immediately. To reuse its behavior, assign the function itself to a variable instead.

```javascript
const greet = function () {
  console.log("Hello!");
};

greet();
greet();
```

**Assuming IIFE Variables Are Globally Accessible**

```javascript
(function () {
  const secret = "private";
})();

console.log(secret); // ReferenceError
```

Variables declared inside the IIFE are not available in the surrounding scope.

**Overusing IIFEs**

Not every block of code needs an IIFE. Modern block scope, modules, and ordinary functions often provide simpler solutions.

# 🗁 Best Practices

* Use IIFEs when immediate execution and isolated function scope are genuinely useful.
* Prefer clear, parenthesized syntax.
* Use `const` and `let` for internal variables.
* Expose only the functionality that needs to be public.
* Use async IIFEs when asynchronous execution is required in a context without top-level `await`.
* Prefer ES modules for modern application architecture.
* Avoid unnecessary IIFEs when a block or regular function is sufficient.

# 🗁 Summary

* An IIFE is a function expression that executes immediately after definition.
* Parentheses are commonly used to make the function an expression.
* IIFEs can receive arguments and return values.
* They create isolated function scopes.
* They can encapsulate private state through closures.
* The Module Pattern commonly uses IIFEs to expose controlled public interfaces.
* Modern JavaScript features such as ES modules and block scope reduce the need for many traditional IIFE use cases.

# 🗁 Practice Exercises

* Create an IIFE that prints a welcome message.
* Create an IIFE that accepts two numbers and returns their sum.
* Create an arrow function IIFE that calculates the square of a number.
* Create an async IIFE that fetches data from a public API.
* Build a counter using an IIFE with private state.
* Create a module-like object using an IIFE that exposes `login()`, `logout()`, and `getStatus()` methods.
* Demonstrate how an IIFE prevents global variable pollution.
* Implement a configuration object using an IIFE and expose only selected properties.
