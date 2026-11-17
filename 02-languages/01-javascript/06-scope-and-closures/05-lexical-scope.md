# Lexical Scope

Lexical scope is JavaScript's rule for determining where variables and functions can be accessed based on **where they are declared in the source code**.

The word *lexical* refers to the program's written structure. JavaScript determines the scope relationship by examining how code is nested, not by where or how a function is called.

A function can access variables declared in:

* Its own local scope.
* Any enclosing function scope.
* The global scope.

It cannot directly access variables declared inside its child functions.

# 🗁 Basic Example

```javascript
const message = "Hello from global scope";

function greet() {
  const greeting = "Hello from greet";

  function sayHello() {
    const name = "Muhammad";

    console.log(message);
    console.log(greeting);
    console.log(name);
  }

  sayHello();
}

greet();
```

Output:

```text
Hello from global scope
Hello from greet
Muhammad
```

The `sayHello()` function can access variables from its own scope, the enclosing `greet()` scope, and the global scope.

This happens because of lexical scope.

# 🗁 Scope Chain

JavaScript uses a scope chain to resolve variable references.

When JavaScript encounters a variable reference, it searches in this order:

* Current local scope.
* Immediately enclosing scope.
* Next enclosing scope.
* Global scope.

If the variable is not found, JavaScript throws a `ReferenceError`.

Example:

```javascript
const globalValue = "Global";

function outer() {
  const outerValue = "Outer";

  function middle() {
    const middleValue = "Middle";

    function inner() {
      const innerValue = "Inner";

      console.log(innerValue);
      console.log(middleValue);
      console.log(outerValue);
      console.log(globalValue);
    }

    inner();
  }

  middle();
}

outer();
```

Output:

```text
Inner
Middle
Outer
Global
```

The nested function follows the scope chain to find variables declared in its enclosing lexical environments.

# 🗁 Lexical Scope and Nested Functions

Nested functions inherit access to the lexical environments surrounding their declaration.

```javascript
function createMessage() {
  const message = "JavaScript is powerful";

  function displayMessage() {
    console.log(message);
  }

  displayMessage();
}

createMessage();
```

Output:

```text
JavaScript is powerful
```

The `displayMessage()` function accesses `message` because it was declared inside the lexical environment of `createMessage()`.

# 🗁 Lexical Scope vs Dynamic Scope

Lexical scope and dynamic scope determine variable access differently.

| Lexical Scope | Dynamic Scope |
| --- | --- |
| Based on where code is written | Based on the call sequence |
| Determined by source-code structure | Determined at runtime through callers |
| Used by JavaScript | Not used by JavaScript |
| Enclosing declarations determine access | Calling functions may determine access |

JavaScript uses lexical scope.

Consider:

```javascript
const value = "Global";

function showValue() {
  console.log(value);
}

function execute() {
  const value = "Local";

  showValue();
}

execute();
```

Output:

```text
Global
```

Although `showValue()` is called from inside `execute()`, it accesses the global `value`.

Why?

Because `showValue()` was declared in the global lexical scope. Its variable lookup follows its declaration environment, not its caller's environment.

# 🗁 Variable Shadowing

Variable shadowing occurs when a variable declared in an inner scope has the same name as a variable in an outer scope.

```javascript
const message = "Global message";

function showMessage() {
  const message = "Local message";

  console.log(message);
}

showMessage();

console.log(message);
```

Output:

```text
Local message
Global message
```

The inner `message` shadows the outer variable within the function.

The outer variable remains unchanged.

# 🗁 Lexical Scope with Block Scope

Lexical scope also works with block-scoped variables declared using `let` and `const`.

```javascript
const language = "JavaScript";

if (true) {
  const framework = "React";

  console.log(language);
  console.log(framework);
}

console.log(language);

// ReferenceError: framework is not defined
console.log(framework);
```

The block can access `language` from its enclosing scope.

However, the outer scope cannot access `framework`, which belongs to the inner block.

# 🗁 Lexical Scope and Closures

Lexical scope is the foundation of closures.

A closure occurs when a function retains access to variables from its lexical environment, even after the outer function has finished executing.

```javascript
function createCounter() {
  let count = 0;

  return function increment() {
    count++;

    return count;
  };
}

const counter = createCounter();

console.log(counter());
console.log(counter());
console.log(counter());
```

Output:

```text
1
2
3
```

The `increment()` function retains access to `count` through its lexical environment.

This behavior is called a closure and is covered in the next topic.

# 🗁 Common Mistakes

**Assuming the caller determines variable access**

```javascript
const name = "Global";

function showName() {
  console.log(name);
}

function run() {
  const name = "Local";

  showName();
}

run();
```

Output:

```text
Global
```

JavaScript uses lexical scope, not the caller's local variables.

**Assuming inner functions expose their variables to outer scopes**

```javascript
function outer() {
  function inner() {
    const secret = "Hidden";
  }

  inner();

  console.log(secret);
}

outer();
```

This throws a `ReferenceError` because `secret` belongs to the `inner()` function's local scope.

**Confusing lexical scope with closure**

Lexical scope defines how variable access is determined. A closure is a function retaining access to its surrounding lexical environment.

They are closely related but are not the same concept.

# 🗁 Best Practices

* Understand the scope chain before using deeply nested functions.
* Use `const` by default and `let` when reassignment is required.
* Avoid unnecessary variable shadowing.
* Keep functions focused and limit unnecessary nesting.
* Use closures intentionally when implementing private state and function factories.
* Avoid relying on global variables when local scope is sufficient.
* Remember that a function's lexical environment is determined by where it is declared.

# 🗁 Key Takeaways

* Lexical scope determines variable accessibility based on source-code structure.
* JavaScript uses lexical scope rather than dynamic scope.
* Nested functions can access variables from their enclosing scopes.
* Variable lookup follows the scope chain.
* Inner variables can shadow variables from outer scopes.
* Outer scopes cannot directly access variables declared inside nested functions.
* Lexical scope is the foundation of closures.
