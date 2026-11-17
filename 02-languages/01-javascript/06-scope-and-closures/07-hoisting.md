# Hoisting

Hoisting is JavaScript's behavior of processing declarations before executing the code within their scope.

This means certain variables and function declarations can be referenced before their declaration appears in the source code.

However, hoisting does not mean JavaScript physically moves declarations to the top of the file.

Instead, JavaScript creates the necessary bindings during the creation phase of an execution context before executing statements.

Hoisting behavior depends on how a variable or function is declared.

# 🗁 Hoisting with var

Variables declared with `var` are hoisted and initialized with `undefined`.

Example:

```javascript
console.log(message);

var message = "Hello JavaScript";

console.log(message);
```

Output:

```text
undefined
Hello JavaScript
```

JavaScript processes the declaration before executing the assignment.

Conceptually, the code behaves like:

```javascript
var message;

console.log(message);

message = "Hello JavaScript";

console.log(message);
```

The declaration is available from the beginning of its function or global scope, but the assignment remains at its original position.

# 🗁 Hoisting with let

Variables declared with `let` are also hoisted, but they are not initialized immediately.

Accessing them before their declaration results in a `ReferenceError`.

```javascript
console.log(message);

let message = "Hello JavaScript";
```

Output:

```text
ReferenceError: Cannot access 'message' before initialization
```

Unlike `var`, a `let` variable remains uninitialized until execution reaches its declaration.

# 🗁 Hoisting with const

Variables declared with `const` behave similarly to `let` during hoisting.

They are hoisted but remain uninitialized until their declaration is evaluated.

```javascript
console.log(language);

const language = "JavaScript";
```

Output:

```text
ReferenceError: Cannot access 'language' before initialization
```

A `const` declaration must also include an initializer.

```javascript
const language = "JavaScript";
```

# 🗁 Temporal Dead Zone (TDZ)

The Temporal Dead Zone is the period between entering a scope and executing the declaration that initializes a `let` or `const` variable.

During this period, accessing the variable causes a `ReferenceError`.

Example:

```javascript
{
  // Temporal Dead Zone begins.

  console.log(name);

  let name = "Muhammad";

  // Temporal Dead Zone ends.
}
```

The variable exists in the scope but cannot be accessed before initialization.

Another example:

```javascript
{
  let value = 10;

  console.log(value);
}
```

Output:

```text
10
```

The variable becomes accessible after its declaration is evaluated.

# 🗁 Hoisting with Function Declarations

Function declarations are hoisted with their function definitions.

This means they can be called before their declaration appears in the source code.

```javascript
greet();

function greet() {
  console.log("Hello, Muhammad!");
}
```

Output:

```text
Hello, Muhammad!
```

The function declaration is available before normal statement execution begins.

# 🗁 Hoisting with Function Expressions

Function expressions behave differently from function declarations.

The behavior depends on the variable declaration used to store the function.

**Using var**

```javascript
greet();

var greet = function () {
  console.log("Hello!");
};
```

Output:

```text
TypeError: greet is not a function
```

The `var` variable is initialized with `undefined`, but the function assignment has not executed yet.

Calling `undefined` as a function causes a `TypeError`.

Conceptually:

```javascript
var greet;

greet();

greet = function () {
  console.log("Hello!");
};
```

**Using let**

```javascript
greet();

let greet = function () {
  console.log("Hello!");
};
```

Output:

```text
ReferenceError: Cannot access 'greet' before initialization
```

The variable is in the Temporal Dead Zone when the call occurs.

**Using const**

```javascript
greet();

const greet = function () {
  console.log("Hello!");
};
```

Output:

```text
ReferenceError: Cannot access 'greet' before initialization
```

The behavior is the same as `let` before initialization.

# 🗁 Hoisting with Arrow Functions

Arrow functions are also function expressions and follow the hoisting behavior of their variable declarations.

```javascript
greet();

const greet = () => {
  console.log("Hello!");
};
```

Output:

```text
ReferenceError: Cannot access 'greet' before initialization
```

The arrow function is not available until the assignment executes.

# 🗁 Hoisting Inside Functions

Hoisting also applies within function scopes.

```javascript
function example() {
  console.log(value);

  var value = 100;

  console.log(value);
}

example();
```

Output:

```text
undefined
100
```

The `var` declaration is hoisted to the beginning of the function scope.

However, the assignment remains in its original position.

# 🗁 Function Scope and Block Scope

The scope in which a declaration appears determines where its hoisted binding is available.

**var is function-scoped**

```javascript
function example() {
  if (true) {
    var message = "Hello";
  }

  console.log(message);
}

example();
```

Output:

```text
Hello
```

The `var` declaration belongs to the containing function scope, not the `if` block.

**let and const are block-scoped**

```javascript
function example() {
  if (true) {
    let message = "Hello";
  }

  console.log(message);
}

example();
```

Output:

```text
ReferenceError: message is not defined
```

The `let` declaration belongs only to the `if` block.

# 🗁 Hoisting with Classes

Class declarations are hoisted in the sense that their bindings are created when the scope is entered.

However, they remain uninitialized until execution reaches the class declaration.

```javascript
const user = new User();

class User {
  constructor() {
    this.name = "Muhammad";
  }
}
```

Output:

```text
ReferenceError: Cannot access 'User' before initialization
```

The class cannot be accessed before its declaration is evaluated.

Correct usage:

```javascript
class User {
  constructor() {
    this.name = "Muhammad";
  }
}

const user = new User();

console.log(user.name);
```

Output:

```text
Muhammad
```

# 🗁 Hoisting Comparison

| Declaration | Hoisted | Initial State | Before Declaration |
| --- | --- | --- | --- |
| `var` | Yes | `undefined` | Returns `undefined` |
| `let` | Yes | Uninitialized | `ReferenceError` |
| `const` | Yes | Uninitialized | `ReferenceError` |
| Function declaration | Yes | Function definition | Can be called |
| Function expression with `var` | Variable only | `undefined` | Calling causes `TypeError` |
| Function expression with `let` | Variable only | Uninitialized | `ReferenceError` |
| Arrow function with `const` | Variable only | Uninitialized | `ReferenceError` |
| Class declaration | Yes | Uninitialized | `ReferenceError` |

# 🗁 Common Hoisting Mistakes

**Assuming declarations are physically moved**

Hoisting is a description of JavaScript's execution behavior, not a literal source-code transformation.

**Confusing undefined with not defined**

```javascript
console.log(value);

var value = 10;
```

The variable exists and contains `undefined` at the time of access.

By contrast:

```javascript
console.log(value);
```

If no accessible declaration exists, JavaScript throws a `ReferenceError`.

**Assuming let and const are not hoisted**

They are hoisted, but remain uninitialized during the Temporal Dead Zone.

**Calling a function expression before initialization**

Function expressions are not available before their variable assignment executes.

**Confusing declaration hoisting with assignment**

Hoisting makes declarations available according to their rules. It does not move assignment statements to the beginning of the scope.

# 🗁 Best Practices

* Prefer `const` for bindings that do not need reassignment.
* Use `let` when reassignment is necessary.
* Avoid `var` in modern JavaScript unless there is a specific compatibility requirement.
* Declare variables before using them.
* Declare functions before their first use when that improves readability.
* Understand the Temporal Dead Zone when working with `let` and `const`.
* Avoid relying on hoisting as a code-organization technique.
* Keep declarations close to where they are used.

# 🗁 Key Takeaways

* Hoisting is JavaScript's declaration-processing behavior.
* `var` declarations are initialized with `undefined`.
* `let` and `const` declarations remain uninitialized until evaluated.
* Accessing `let` or `const` before initialization causes a `ReferenceError`.
* Function declarations can be called before their declaration.
* Function expressions and arrow functions follow their variable declaration's hoisting behavior.
* Class declarations cannot be accessed before initialization.
* The Temporal Dead Zone applies to `let`, `const`, and class declarations.
