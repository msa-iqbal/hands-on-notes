# JavaScript Strict Mode

Strict mode is a JavaScript execution mode that enforces stricter parsing and error handling rules.

It helps developers write more reliable JavaScript by preventing certain error-prone behaviors and identifying common programming mistakes.

Strict mode was introduced in ECMAScript 5 (ES5).

Without strict mode, JavaScript allows some legacy behaviors that can make programs difficult to debug. Strict mode changes the behavior of certain operations and turns some previously silent mistakes into runtime errors.

# 🗁 What Is Strict Mode?

Strict mode enables a restricted set of JavaScript language rules.

It helps developers:

* Detect accidental variable assignments.
* Prevent accidental creation of global variables.
* Avoid certain unsafe or confusing language behaviors.
* Identify invalid assignments.
* Make `this` behavior more predictable.
* Prevent duplicate parameter names in functions.
* Write more maintainable JavaScript code.

Strict mode does not change JavaScript into a statically typed language or automatically make every program secure or error-free.

# 🗁 Enabling Strict Mode

Strict mode is enabled by placing the following directive at the beginning of a script or function body:

```javascript
"use strict";
```

The directive must appear before ordinary statements in the relevant scope.

Example:

```javascript
"use strict";

const message = "Strict mode is enabled";

console.log(message);
```

Output:

```text
Strict mode is enabled
```

The string `"use strict"` is recognized as a strict mode directive when it appears in the appropriate position in a script or function body.

# 🗁 Strict Mode vs Non-Strict Mode

JavaScript can execute code in strict or non-strict mode.

| Feature | Non-strict mode | Strict mode |
| --- | --- | --- |
| Undeclared variable assignment | May create a global property | Throws `ReferenceError` |
| Assigning to a read-only property | May fail silently | Throws `TypeError` |
| Assigning to a non-writable property | May fail silently | Throws `TypeError` |
| Duplicate function parameters | Generally allowed in ordinary functions | Syntax error |
| Plain function-call `this` | Usually refers to the global object | `undefined` |
| Some legacy syntax | Permitted | Restricted or rejected |

# 🗁 Preventing Accidental Global Variables

In non-strict mode, assigning a value to an undeclared identifier can create a property on the global object.

Example:

```javascript
username = "Muhammad";

console.log(username);
```

In a classic browser script, this can create a global variable.

In strict mode, the same assignment throws a `ReferenceError`.

```javascript
"use strict";

username = "Muhammad";

console.log(username);
```

Error:

```text
ReferenceError: username is not defined
```

Correct approach:

```javascript
"use strict";

const username = "Muhammad";

console.log(username);
```

Strict mode helps identify missing variable declarations instead of silently creating global properties.

# 🗁 Preventing Assignment to Read-Only Properties

Strict mode throws a `TypeError` when code attempts to assign a value to a non-writable property.

Example:

```javascript
"use strict";

const user = {};

Object.defineProperty(user, "name", {
    value: "Muhammad",
    writable: false
});

user.name = "Ahmed";
```

Error:

```text
TypeError: Cannot assign to read only property 'name'
```

Without strict mode, the assignment would generally fail silently.

The property remains unchanged:

```javascript
console.log(user.name);
```

Output:

```text
Muhammad
```

# 🗁 Preventing Duplicate Function Parameters

Strict mode does not allow duplicate parameter names in ordinary function declarations and expressions.

Incorrect:

```javascript
"use strict";

function add(a, a) {
    return a + a;
}
```

This produces a `SyntaxError`.

Correct:

```javascript
"use strict";

function add(a, b) {
    return a + b;
}

console.log(add(10, 20));
```

Output:

```text
30
```

Duplicate parameter names are also prohibited in several other modern function forms, independently of strict mode.

# 🗁 Understanding `this` in Strict Mode

The behavior of `this` changes in strict mode when a function is called without an explicit receiver.

**Non-strict mode:**

```javascript
function showThis() {
    console.log(this);
}

showThis();
```

In a classic browser script, `this` inside the function refers to the global object (`window`).

**Strict mode:**

```javascript
"use strict";

function showThis() {
    console.log(this);
}

showThis();
```

Output:

```text
undefined
```

In strict mode, a plain function call does not automatically substitute the global object for `this`.

This makes function behavior more predictable.

# 🗁 Preventing Invalid Property Operations

Strict mode throws errors for certain operations that may fail silently in non-strict mode.

Example:

```javascript
"use strict";

const user = {};

Object.defineProperty(user, "age", {
    value: 25,
    writable: false
});

user.age = 30;
```

Error:

```text
TypeError: Cannot assign to read only property 'age'
```

Another example involves deleting a non-configurable property:

```javascript
"use strict";

const user = {};

Object.defineProperty(user, "name", {
    value: "Muhammad",
    configurable: false
});

delete user.name;
```

This throws a `TypeError`.

Strict mode makes these failed operations visible instead of allowing them to pass silently.

# 🗁 Restricting Legacy Syntax

Strict mode disallows certain legacy language features and syntax.

**Legacy octal literals**

Incorrect:

```javascript
"use strict";

const number = 010;
```

This produces a `SyntaxError` in strict mode.

Use modern numeric notation:

```javascript
"use strict";

const decimal = 10;
const octal = 0o10;

console.log(decimal);
console.log(octal);
```

Output:

```text
10
8
```

**Legacy octal escape sequences**

Strict mode also prohibits legacy octal escape sequences in strings.

Avoid:

```javascript
"use strict";

const value = "\123";
```

Use Unicode escapes or ordinary characters when appropriate.

# 🗁 Strict Mode and `eval()`

Strict mode changes how variables declared inside `eval()` behave.

In non-strict mode, declarations inside a direct `eval()` call can affect the surrounding variable environment.

In strict mode, declarations made inside direct `eval()` remain isolated from the surrounding scope.

Example:

```javascript
"use strict";

eval("var message = 'Hello';");

console.log(typeof message);
```

Output:

```text
undefined
```

The `message` variable does not become a variable in the surrounding scope.

Strict mode also restricts certain uses of `eval` and `arguments` as identifiers.

# 🗁 Applying Strict Mode to a Script

You can enable strict mode for an entire classic JavaScript script.

Example:

```javascript
"use strict";

const name = "Muhammad";

function greet() {
    console.log(`Hello, ${name}!`);
}

greet();
```

All applicable code in that script executes under strict mode.

A directive at the beginning of a script applies to the entire script, not just the first function.

# 🗁 Applying Strict Mode to a Function

You can enable strict mode for an individual function without enabling it for the entire surrounding script.

Example:

```javascript
function strictFunction() {
    "use strict";

    const message = "Strict mode is enabled";

    console.log(message);
}

function nonStrictFunction() {
    const message = "This function inherits the surrounding mode";

    console.log(message);
}
```

The directive applies to the body of `strictFunction()`.

The second function does not automatically become strict merely because another function uses strict mode.

This approach is useful when working with legacy code that cannot immediately be converted to strict mode.

# 🗁 Strict Mode in JavaScript Modules

JavaScript modules automatically execute in strict mode.

You do not need to write `"use strict"` inside an ES module.

Example:

```javascript
// app.js

export function greet() {
    console.log(this);
}

greet();
```

The module executes in strict mode, so the plain function call produces `undefined` for `this`.

This applies to JavaScript modules loaded through module mechanisms such as:

```html
<script type="module" src="app.js"></script>
```

# 🗁 Strict Mode in JavaScript Classes

JavaScript class bodies automatically execute in strict mode.

You do not need to add `"use strict"` inside a class method.

Example:

```javascript
class User {
    showThis() {
        console.log(this);
    }
}

const user = new User();

user.showThis();
```

The method receives the instance as `this` because it is called through `user.showThis()`.

Class methods execute under strict mode regardless of how they are called.

# 🗁 Strict Mode and Arrow Functions

Arrow functions do not have their own `this` binding. They inherit `this` lexically from their surrounding scope.

Strict mode does not change this fundamental behavior.

Example:

```javascript
"use strict";

const user = {
    name: "Muhammad",

    greet() {
        const showName = () => {
            console.log(this.name);
        };

        showName();
    }
};

user.greet();
```

Output:

```text
Muhammad
```

The arrow function uses the `this` value from the surrounding `greet()` method.

# 🗁 Strict Mode Limitations

Strict mode has several important limitations and boundaries:

* It does not automatically validate application data.
* It does not replace error handling with `try...catch`.
* It does not prevent every runtime error.
* It does not make JavaScript statically typed.
* It does not automatically prevent security vulnerabilities.
* It does not change every legacy behavior.
* It applies according to script, function, module, and class boundaries.

For modern JavaScript development, modules and classes already provide strict mode automatically.

# 🗁 Strict Mode Best Practices

* Use ES modules for modern JavaScript applications.
* Prefer `const` and `let` instead of `var`.
* Always declare variables before using them.
* Avoid relying on implicit global variables.
* Use explicit object references when working with `this`.
* Avoid legacy syntax and deprecated language patterns.
* Use strict mode when maintaining classic scripts.
* Keep function behavior predictable and easy to test.

# 🗁 Key Takeaways

* Strict mode is a restricted JavaScript execution mode introduced in ECMAScript 5.
* It is enabled using the `"use strict"` directive.
* It helps identify common programming mistakes.
* Assigning to undeclared variables throws a `ReferenceError`.
* Assigning to non-writable properties throws a `TypeError`.
* Duplicate parameter names are prohibited in strict ordinary functions.
* A plain function call receives `undefined` as `this`.
* Strict mode restricts certain legacy syntax and behaviors.
* ES modules automatically execute in strict mode.
* JavaScript class bodies and methods automatically execute in strict mode.
* Arrow functions inherit `this` from their surrounding lexical scope.
