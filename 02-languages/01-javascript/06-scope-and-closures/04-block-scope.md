# Block Scope

**Block scope** determines the accessibility of variables declared inside a block of code.

A block is a section of JavaScript code enclosed by curly braces `{}`.

JavaScript uses block scope to restrict variables declared with `let` and `const` to the block in which they are defined.

Block scope helps prevent naming conflicts, accidental variable modifications, and unintended access to local data.

# 🗁 What Is a Block?

A block is created using curly braces.

```javascript
{
  const message = "Hello, JavaScript!";

  console.log(message);
}
```

The variable `message` is accessible inside the block.

Outside the block, it cannot be accessed.

```javascript
{
  const message = "Hello!";
}

console.log(message); // ReferenceError
```

# 🗁 Block-Scoped Variables

JavaScript provides two block-scoped declaration keywords:

* `let`
* `const`

Both declarations restrict variable accessibility to the nearest enclosing block.

```javascript
{
  let language = "JavaScript";
  const version = "ES2025";

  console.log(language);
  console.log(version);
}
```

Attempting to access these variables outside the block produces a `ReferenceError`.

# 🗁 Block Scope with `let`

The `let` keyword declares a variable that is limited to its enclosing block.

```javascript
{
  let count = 10;

  console.log(count); // 10
}

console.log(count); // ReferenceError
```

A `let` variable can be reassigned within its scope.

```javascript
{
  let count = 10;

  count = 20;

  console.log(count); // 20
}
```

However, it cannot be redeclared in the same lexical scope.

```javascript
{
  let count = 10;

  // let count = 20; // SyntaxError
}
```

# 🗁 Block Scope with `const`

The `const` keyword also creates a block-scoped binding.

```javascript
{
  const applicationName = "My Application";

  console.log(applicationName);
}

console.log(applicationName); // ReferenceError
```

A `const` binding cannot be reassigned.

```javascript
{
  const version = "1.0.0";

  // version = "2.0.0"; // TypeError
}
```

However, objects and arrays declared with `const` can still be mutated.

```javascript
{
  const user = {
    name: "Alice",
  };

  user.name = "Bob";

  console.log(user.name); // Bob
}
```

The binding remains unchanged; only the object's contents are modified.

# 🗁 Block Scope vs `var`

Unlike `let` and `const`, `var` does not create block-scoped variables.

It is scoped to the nearest containing function or the global environment.

```javascript
{
  var first = "JavaScript";
  let second = "TypeScript";
  const third = "Python";
}

console.log(first);  // JavaScript
console.log(second); // ReferenceError
console.log(third);  // ReferenceError
```

The `var` declaration escapes the block, while `let` and `const` remain inside it.

# 🗁 Block Scope in Conditional Statements

Conditional statements create blocks when their bodies are enclosed in curly braces.

```javascript
const age = 25;

if (age >= 18) {
  const status = "Adult";

  console.log(status); // Adult
}

console.log(status); // ReferenceError
```

The variable `status` exists only inside the `if` block.

The same principle applies to `else` and `else if` blocks.

```javascript
const score = 75;

if (score >= 90) {
  const grade = "A";
  console.log(grade);
} else if (score >= 70) {
  const grade = "B";
  console.log(grade);
} else {
  const grade = "C";
  console.log(grade);
}
```

Each block has its own lexical environment.

# 🗁 Block Scope in Loops

Loop statements commonly use block-scoped variables.

```javascript
for (let i = 0; i < 3; i++) {
  console.log(i);
}

console.log(i); // ReferenceError
```

The variable `i` is restricted to the loop's scope.

Using `var` produces different behavior.

```javascript
for (var i = 0; i < 3; i++) {
  console.log(i);
}

console.log(i); // 3
```

The variable `i` remains accessible after the loop because `var` is not block-scoped.

# 🗁 Block Scope in Nested Blocks

A block can contain another block.

Each nested block can introduce its own lexical declarations.

```javascript
{
  const first = "Outer";

  {
    const second = "Inner";

    console.log(first);  // Outer
    console.log(second); // Inner
  }

  console.log(first);  // Outer
  console.log(second); // ReferenceError
}
```

The inner block can access variables from the outer block.

The outer block cannot directly access variables declared in the inner block.

# 🗁 Variable Shadowing in Blocks

Variable shadowing occurs when an inner scope declares a variable with the same name as an outer variable.

```javascript
const message = "Global message";

{
  const message = "Block message";

  console.log(message); // Block message
}

console.log(message); // Global message
```

The inner declaration shadows the outer declaration within the block.

The outer variable remains unchanged.

# 🗁 Block Scope and the Temporal Dead Zone

Variables declared with `let` and `const` are subject to the **Temporal Dead Zone (TDZ)**.

The TDZ is the period between entering a lexical scope and initializing a lexical declaration.

```javascript
{
  // console.log(message); // ReferenceError

  let message = "Hello!";

  console.log(message);
}
```

Even though the declaration is processed when the scope is entered, the variable cannot be accessed before its initialization.

The same behavior applies to `const`.

```javascript
{
  // console.log(version); // ReferenceError

  const version = "1.0.0";

  console.log(version);
}
```

# 🗁 Block Scope in Switch Statements

A `switch` statement has a shared lexical scope for its case clauses unless additional blocks are introduced.

This can cause redeclaration errors.

```javascript
const role = "admin";

switch (role) {
  case "admin": {
    const message = "Administrator";
    console.log(message);
    break;
  }

  case "user": {
    const message = "Regular user";
    console.log(message);
    break;
  }
}
```

Using braces creates a separate block for each case.

This allows both cases to declare a variable named `message` independently.

# 🗁 Block Scope in Standalone Blocks

JavaScript also allows standalone blocks that are not associated with conditional statements or loops.

```javascript
{
  const temporaryValue = 100;

  console.log(temporaryValue);
}

{
  const temporaryValue = 200;

  console.log(temporaryValue);
}
```

Both declarations are valid because they belong to separate block scopes.

Standalone blocks can be useful when a temporary variable should remain isolated within a limited section of code.

# 🗁 Block Scope and Closures

Closures can retain access to variables declared inside block scopes.

```javascript
function createTasks() {
  const tasks = [];

  for (let i = 0; i < 3; i++) {
    tasks.push(() => {
      return i;
    });
  }

  return tasks;
}

const tasks = createTasks();

console.log(tasks[0]()); // 0
console.log(tasks[1]()); // 1
console.log(tasks[2]()); // 2
```

The `let` declaration in the loop creates a distinct binding for each iteration.

Each arrow function closes over the binding from its corresponding iteration.

With `var`, the behavior is different:

```javascript
function createTasks() {
  const tasks = [];

  for (var i = 0; i < 3; i++) {
    tasks.push(() => i);
  }

  return tasks;
}

const tasks = createTasks();

console.log(tasks[0]()); // 3
console.log(tasks[1]()); // 3
console.log(tasks[2]()); // 3
```

All three callbacks share the same function-scoped `i` variable.

# 🗁 Block Scope vs Function Scope

| Feature | Block Scope | Function Scope |
| --- | --- | --- |
| Boundary | Nearest enclosing block | Function body |
| Main declarations | `let`, `const` | `var`, `let`, `const` |
| Accessible outside block | No for `let` and `const` | Yes within the same function |
| Nested scope access | Can access outer lexical variables | Can access outer lexical variables |
| Common use | Loops, conditions, temporary variables | Function-local variables |

# 🗁 Best Practices

* Prefer `const` for bindings that do not need reassignment.
* Use `let` when reassignment is necessary.
* Avoid `var` in modern JavaScript unless its function-scoping behavior is specifically required.
* Declare variables in the narrowest scope possible.
* Use blocks to isolate temporary variables.
* Avoid unnecessary variable shadowing.
* Use braces around `switch` case bodies when declaring lexical variables.
* Understand the Temporal Dead Zone when working with `let` and `const`.
* Use block-scoped declarations in loops when creating callbacks that depend on the iteration value.

# 🗁 Summary

* A block is a section of code enclosed in curly braces.
* `let` and `const` create block-scoped bindings.
* `var` is function-scoped rather than block-scoped.
* Variables declared inside a block cannot be accessed outside that block.
* Nested blocks can access variables from their outer lexical environments.
* Variable shadowing allows an inner block to declare an identifier with the same name as an outer variable.
* `let` and `const` are subject to the Temporal Dead Zone.
* Block scope is essential for writing predictable and maintainable JavaScript.

# 🗁 Practice Exercises

* Create a standalone block and declare variables using `let` and `const`.
* Compare `var`, `let`, and `const` inside an `if` statement.
* Demonstrate block scope inside a `for` loop.
* Create nested blocks and access variables from outer scopes.
* Demonstrate variable shadowing using nested blocks.
* Create separate blocks that declare variables with identical names.
* Demonstrate the Temporal Dead Zone using `let`.
* Demonstrate the Temporal Dead Zone using `const`.
* Create a `switch` statement with separate block scopes for each case.
* Create callbacks inside a loop and compare the behavior of `var` and `let`.
