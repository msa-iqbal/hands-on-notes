# Function Scope

**Function scope** defines the accessibility of variables and functions declared inside a JavaScript function.

Variables declared with `var`, `let`, and `const` inside a function are accessible within that function and its nested scopes, but they cannot be accessed directly from outside the function.

Function scope is an important part of JavaScript's lexical scoping system.

# 🗁 Understanding Function Scope

Every function creates its own scope when executed.

```javascript
function greet() {
  const message = "Hello, JavaScript!";

  console.log(message);
}

greet(); // Hello, JavaScript!

console.log(message); // ReferenceError
```

The variable `message` belongs to the scope of `greet()`.

It can be accessed inside the function but not from the surrounding scope.

# 🗁 Variables Inside Function Scope

JavaScript provides three main variable declaration keywords:

* `var`
* `let`
* `const`

All three are function-scoped when declared inside a function.

```javascript
function example() {
  var first = "JavaScript";
  let second = "TypeScript";
  const third = "Python";

  console.log(first);
  console.log(second);
  console.log(third);
}

example();
```

All three variables are accessible throughout their containing function, subject to their initialization rules.

# 🗁 `var` and Function Scope

The `var` keyword declares function-scoped variables.

```javascript
function calculate() {
  if (true) {
    var result = 100;
  }

  console.log(result); // 100
}

calculate();

console.log(result); // ReferenceError
```

Although `result` is declared inside an `if` block, it is accessible throughout the containing function because `var` does not provide block scope.

# 🗁 `let` and `const` Inside Functions

The `let` and `const` keywords provide block scope, even when declared inside a function.

```javascript
function example() {
  if (true) {
    let first = 10;
    const second = 20;

    console.log(first);
    console.log(second);
  }

  console.log(first); // ReferenceError
  console.log(second); // ReferenceError
}

example();
```

Both variables are restricted to the `if` block.

This demonstrates the difference between function scope and block scope.

# 🗁 Nested Function Scope

A function declared inside another function is called a nested function.

The inner function can access variables from its outer function.

```javascript
function outerFunction() {
  const outerVariable = "Outer";

  function innerFunction() {
    const innerVariable = "Inner";

    console.log(outerVariable);
    console.log(innerVariable);
  }

  innerFunction();
}

outerFunction();
```

Output:

```text
Outer
Inner
```

The inner function can access its own variables and variables from its enclosing lexical environment.

However, the outer function cannot directly access variables declared inside the inner function.

```javascript
function outerFunction() {
  function innerFunction() {
    const message = "Hello!";
  }

  innerFunction();

  console.log(message); // ReferenceError
}

outerFunction();
```

# 🗁 Function Scope and Parameters

Function parameters are also local to the function.

```javascript
function greet(name, language) {
  console.log(`Hello, ${name}!`);
  console.log(`Language: ${language}`);
}

greet("Alice", "JavaScript");

console.log(name); // ReferenceError
console.log(language); // ReferenceError
```

The parameters `name` and `language` are available within the function body and its nested scopes.

# 🗁 Function Scope and Variable Shadowing

Variable shadowing occurs when a local variable has the same name as a variable in an outer scope.

```javascript
const message = "Global message";

function showMessage() {
  const message = "Local message";

  console.log(message);
}

showMessage(); // Local message

console.log(message); // Global message
```

The local variable shadows the outer variable within the function.

The outer variable remains unchanged.

# 🗁 Function Scope and Closures

A closure occurs when an inner function retains access to variables from its surrounding lexical environment.

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

Normally, local variables belong to a function execution.

However, the returned function retains access to `count` because it closes over the surrounding lexical environment.

This makes closures useful for maintaining private state.

# 🗁 Function Scope and `this`

In regular functions, the value of `this` depends on how the function is called.

```javascript
const user = {
  name: "Alice",

  greet: function () {
    console.log(this.name);
  },
};

user.greet(); // Alice
```

Here, `this` refers to the object used as the method receiver.

Arrow functions behave differently. They do not create their own `this`; instead, they capture it lexically from their surrounding environment.

```javascript
const user = {
  name: "Alice",

  greet: function () {
    const inner = () => {
      console.log(this.name);
    };

    inner();
  },
};

user.greet(); // Alice
```

The arrow function uses the `this` value from `greet()`.

# 🗁 Function Scope vs Block Scope

Function scope and block scope determine variable accessibility at different levels.

| Feature | Function Scope | Block Scope |
| --- | --- | --- |
| Boundary | Function body | Curly-brace block |
| Common declarations | `var`, `let`, `const` inside functions | `let`, `const` |
| Accessible outside function | No | Depends on containing function |
| Accessible outside block | Yes for `var` | No for `let` and `const` |
| Nested scope access | Yes, through lexical scope | Yes, through lexical scope |

Example:

```javascript
function example() {
  if (true) {
    var functionScoped = "Function scoped";
    let blockScoped = "Block scoped";
  }

  console.log(functionScoped); // Function scoped
  console.log(blockScoped); // ReferenceError
}

example();
```

# 🗁 Function Scope in Loops

Function scope is particularly important when using `var` and `let` in loops.

Consider:

```javascript
function example() {
  for (var i = 0; i < 3; i++) {
    console.log(i);
  }

  console.log(i); // 3
}

example();
```

The variable `i` remains accessible after the loop because `var` is function-scoped.

With `let`:

```javascript
function example() {
  for (let i = 0; i < 3; i++) {
    console.log(i);
  }

  console.log(i); // ReferenceError
}

example();
```

The variable `i` is restricted to the loop's lexical scope.

# 🗁 Function Scope in JavaScript Modules

Functions declared inside a module can access module-level variables through lexical scope.

```javascript
// user.js

const defaultRole = "Guest";

export function getUserRole() {
  return defaultRole;
}
```

The function `getUserRole()` can access `defaultRole` because it is declared in the surrounding module scope.

The function's local scope and the module's scope remain separate lexical environments.

# 🗁 Common Function Scope Errors

**Accessing Local Variables Outside a Function**

```javascript
function calculate() {
  const total = 100;
}

console.log(total); // ReferenceError
```

**Assuming `var` Is Block-Scoped**

```javascript
function example() {
  if (true) {
    var status = "Active";
  }

  console.log(status); // Active
}
```

**Accessing an Inner Function's Variables from an Outer Function**

```javascript
function outer() {
  function inner() {
    const value = 50;
  }

  inner();

  console.log(value); // ReferenceError
}
```

**Unexpected Variable Shadowing**

```javascript
const total = 100;

function calculate() {
  const total = 50;

  console.log(total); // 50
}

calculate();

console.log(total); // 100
```

# 🗁 Best Practices

* Prefer `const` for variables that do not need reassignment.
* Use `let` when a variable needs reassignment.
* Avoid `var` in modern JavaScript unless its function-scoping behavior is specifically required.
* Keep function-local variables close to where they are used.
* Avoid unnecessarily large functions with too many local variables.
* Use meaningful names to reduce accidental shadowing.
* Pass dependencies through parameters when appropriate.
* Use closures when private state or persistent lexical access is needed.
* Understand the difference between function scope and block scope.

# 🗁 Summary

* Function scope defines where identifiers declared inside a function can be accessed.
* Every function creates its own lexical scope.
* `var`, `let`, and `const` are function-contained when declared inside a function.
* `var` is function-scoped, while `let` and `const` are block-scoped.
* Nested functions can access variables from their outer lexical environments.
* Function parameters belong to the function's scope.
* Closures allow inner functions to retain access to outer function variables.
* Understanding function scope helps prevent unexpected variable access and naming conflicts.

# 🗁 Practice Exercises

* Create a function with three local variables using `var`, `let`, and `const`.
* Try accessing local variables outside their function and observe the errors.
* Create a nested function that accesses a variable from its outer function.
* Demonstrate the difference between `var` and `let` inside an `if` block.
* Create a function that accepts parameters and uses them inside a nested function.
* Demonstrate variable shadowing between global and function scopes.
* Build a counter using a closure.
* Create a function that returns another function with access to a private variable.
* Compare `var` and `let` inside a `for` loop.
* Explain how lexical scope allows nested functions to access outer variables.
