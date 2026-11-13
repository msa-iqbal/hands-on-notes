# Use Strict Directive

The `"use strict"` directive is a special JavaScript directive that enables strict mode within a script or function.

Strict mode enforces stricter language rules, helps detect programming mistakes, and prevents certain error-prone JavaScript behaviors.

Although strict mode was introduced in ECMAScript 5 (ES5), modern JavaScript modules and class bodies automatically execute in strict mode.

This document focuses on the syntax, placement, scope, behavior, and practical usage of the `"use strict"` directive.

# 🗁 What Is the `"use strict"` Directive?

The `"use strict"` directive tells the JavaScript engine to interpret the applicable code using strict mode rules.

Syntax:

```javascript
"use strict";
```

It is a string literal with special meaning when placed in a valid directive position.

Example:

```javascript
"use strict";

const name = "Muhammad";

console.log(name);
```

Output:

```text
Muhammad
```

The directive must appear before ordinary statements in the relevant script or function body.

# 🗁 Placement of the Directive

The placement of `"use strict"` determines which code executes in strict mode.

There are two common ways to enable it explicitly:

* At the beginning of a script.
* At the beginning of a function body.

**At the beginning of a script**

```javascript
"use strict";

const message = "Strict mode is enabled";

console.log(message);
```

The entire script executes in strict mode.

**At the beginning of a function**

```javascript
function greet() {
    "use strict";

    const message = "Hello, JavaScript!";

    console.log(message);
}

greet();
```

Only the function body is explicitly placed in strict mode.

# 🗁 Directive Prologue

A directive prologue is a sequence of string literal expression statements at the beginning of a script or function body.

JavaScript recognizes directives such as `"use strict"` within this sequence.

Example:

```javascript
"use strict";
"custom directive";

const message = "Hello";

console.log(message);
```

Both string literals appear in the directive prologue.

However, `"use strict"` must be a valid directive expression to activate strict mode.

**Important:** The directive must appear before ordinary statements.

Example:

```javascript
const message = "Hello";

"use strict";

console.log(message);
```

Here, `"use strict"` does not enable strict mode because an ordinary statement appeared before it.

The string is simply an expression statement.

# 🗁 Single Quotes and Double Quotes

Both single and double quotation marks can be used.

```javascript
'use strict';
```

```javascript
"use strict";
```

Both forms activate strict mode when placed correctly.

JavaScript treats these string literals as equivalent for this purpose.

# 🗁 Strict Mode Does Not Activate Inside Parentheses

The directive must be a valid string literal expression without wrapping parentheses.

Example:

```javascript
("use strict");

username = "Muhammad";
```

The parenthesized expression does not activate strict mode.

In a classic non-strict script, the assignment may create a global property.

Correct:

```javascript
"use strict";

username = "Muhammad";
```

This throws a `ReferenceError` because `username` has not been declared.

# 🗁 Strict Mode Does Not Activate After Other Statements

Consider this example:

```javascript
console.log("Starting program");

"use strict";

username = "Muhammad";
```

The `"use strict"` expression appears after an executable statement.

It therefore does not activate strict mode.

In a classic non-strict browser script, the assignment may create a global property.

Correct:

```javascript
"use strict";

console.log("Starting program");

const username = "Muhammad";
```

Place the directive at the beginning of the script.

# 🗁 Applying Strict Mode to Individual Functions

You can enable strict mode inside a function without changing the strictness of the surrounding script.

Example:

```javascript
function strictFunction() {
    "use strict";

    undeclaredVariable = 100;
}

function normalFunction() {
    anotherVariable = 200;
}

strictFunction();
```

Calling `strictFunction()` throws a `ReferenceError`.

The strict directive inside that function does not affect `normalFunction()`.

This technique can be useful when gradually improving older JavaScript code.

# 🗁 Strict Mode and Function Expressions

Strict mode can also be enabled inside function expressions.

Example:

```javascript
const greet = function () {
    "use strict";

    console.log("Hello from strict mode");
};

greet();
```

Output:

```text
Hello from strict mode
```

The directive applies to the function body.

# 🗁 Strict Mode and Immediately Invoked Function Expressions

An Immediately Invoked Function Expression (IIFE) can contain its own strict mode directive.

Example:

```javascript
(function () {
    "use strict";

    const message = "Strict mode inside an IIFE";

    console.log(message);
})();
```

Output:

```text
Strict mode inside an IIFE
```

This pattern was commonly used in older JavaScript applications to create isolated scopes and enable strict mode locally.

Modern ES modules provide their own scope and automatically execute in strict mode.

# 🗁 Strict Mode in ES Modules

ES modules automatically execute in strict mode.

You do not need to add `"use strict"` manually.

Example:

```javascript
// app.js

export function greet() {
    console.log("Hello from an ES module");
}
```

Load the file in HTML:

```html
<script type="module" src="app.js"></script>
```

The module executes in strict mode automatically.

Adding `"use strict"` to a module is unnecessary, although it is generally harmless.

# 🗁 Strict Mode in Class Bodies

JavaScript class bodies automatically execute in strict mode.

Example:

```javascript
class User {
    greet() {
        console.log("Hello from a class method");
    }
}

const user = new User();

user.greet();
```

You do not need to add `"use strict"` inside the class or its methods.

# 🗁 Verifying Strict Mode Behavior

One practical way to verify strict mode is to attempt an operation that strict mode prohibits.

Example:

```javascript
"use strict";

function checkStrictMode() {
    try {
        undeclaredVariable = 100;
    } catch (error) {
        console.log(error.name);
        console.log(error.message);
    }
}

checkStrictMode();
```

Expected output:

```text
ReferenceError
undeclaredVariable is not defined
```

This demonstrates that an undeclared variable assignment is rejected.

# 🗁 Common Mistakes

**Placing the directive after an ordinary statement**

Incorrect:

```javascript
const age = 25;

"use strict";
```

Correct:

```javascript
"use strict";

const age = 25;
```

**Assuming comments prevent strict mode**

Comments before the directive are allowed.

```javascript
// Enable strict mode
"use strict";

const message = "Hello";
```

This correctly enables strict mode.

**Assuming a function directive affects the entire script**

```javascript
function strictFunction() {
    "use strict";
}

function anotherFunction() {
    // This function does not inherit strict mode
}
```

Strict mode is not automatically enabled throughout the surrounding script.

**Using the directive unnecessarily in modules**

```javascript
"use strict";

export const message = "Hello";
```

The directive is redundant because ES modules already use strict mode.

# 🗁 When Should You Use `"use strict"`?

Use `"use strict"` explicitly when working with classic JavaScript scripts that are not modules and need strict mode enforcement.

For example:

```html
<script src="app.js"></script>
```

If `app.js` is a classic script, adding the directive at the beginning enables strict mode.

For modern applications, prefer ES modules:

```html
<script type="module" src="app.js"></script>
```

Modules automatically provide strict mode along with module scope and import/export functionality.

# 🗁 Strict Mode Directive vs Strict Mode

| Feature | `"use strict"` directive | Strict mode |
| --- | --- | --- |
| Meaning | A special directive that enables strict mode | A JavaScript execution mode |
| Syntax | `"use strict";` | No separate syntax |
| Purpose | Activates strict mode in applicable code | Enforces stricter language rules |
| Script support | Yes | Yes |
| Function support | Yes | Yes |
| ES modules | Unnecessary | Enabled automatically |
| Classes | Unnecessary | Enabled automatically |

# 🗁 Key Takeaways

* `"use strict"` is a special JavaScript directive.
* It activates strict mode when placed correctly.
* It can be placed at the beginning of a script or function body.
* It must appear before ordinary statements in the applicable scope.
* Comments before the directive do not prevent activation.
* Parenthesizing the string literal prevents it from being recognized as a directive.
* A function-level directive affects only that function.
* ES modules automatically execute in strict mode.
* Class bodies automatically execute in strict mode.
* Explicit directives are mainly useful when working with classic scripts.
