# JavaScript Export

The `export` statement allows a JavaScript module to make variables, functions, classes, and other declarations available to other modules.

It is one of the fundamental features of ECMAScript Modules (ESM).

Exports make code reusable, encourage modular architecture, and help separate implementation details from public functionality.

JavaScript supports two primary export types:

* **Named exports** — expose declarations using their names.
* **Default exports** — expose one primary value from a module.

A module can use both types of exports.

# 🗁 What Is Export?

An export makes a declaration available outside its module.

Consider a mathematical utility module:

`math.js`

```javascript
export function add(a, b) {
    return a + b;
}
```

Another module can import the exported function:

`app.js`

```javascript
import { add } from "./math.js";

console.log(add(10, 20));
```

**Output**

```text
30
```

Without exporting `add`, another module cannot import it.

# 🗁 Named Exports

Named exports allow a module to expose multiple declarations using their original names.

There are two common ways to create named exports:

* Export declarations directly.
* Export declarations using an export list.

# 🗁 Inline Named Exports

Declarations can be exported directly by placing the `export` keyword before them.

**Exporting a constant**

```javascript
export const PI = 3.14159;
```

**Exporting a function**

```javascript
export function add(a, b) {
    return a + b;
}
```

**Exporting a class**

```javascript
export class User {
    constructor(name) {
        this.name = name;
    }
}
```

**Exporting multiple declarations**

```javascript
// math.js

export const PI = 3.14159;

export function add(a, b) {
    return a + b;
}

export function subtract(a, b) {
    return a - b;
}

export class Calculator {
    multiply(a, b) {
        return a * b;
    }
}
```

Importing the declarations:

```javascript
import {
    PI,
    add,
    subtract,
    Calculator
} from "./math.js";
```

# 🗁 Export Lists

Declarations can also be exported together using an export list.

```javascript
// math.js

const PI = 3.14159;

function add(a, b) {
    return a + b;
}

function subtract(a, b) {
    return a - b;
}

class Calculator {
    multiply(a, b) {
        return a * b;
    }
}

export {
    PI,
    add,
    subtract,
    Calculator
};
```

The declarations are exported after their definitions.

This approach is useful when a module contains many declarations but only some should be publicly accessible.

# 🗁 Export Aliases

The `as` keyword allows a declaration to be exported under a different name.

```javascript
function add(a, b) {
    return a + b;
}

function subtract(a, b) {
    return a - b;
}

export {
    add as sum,
    subtract as difference
};
```

Another module imports the exported names:

```javascript
import {
    sum,
    difference
} from "./math.js";

console.log(sum(10, 5));
console.log(difference(10, 5));
```

**Output**

```text
15
5
```

The original declarations remain named `add` and `subtract` inside their own module.

# 🗁 Default Exports

A default export provides one primary exported value from a module.

It is created using the `export default` syntax.

**Default function export**

```javascript
// greeting.js

export default function greet(name) {
    return `Hello, ${name}!`;
}
```

Importing it:

```javascript
import greet from "./greeting.js";

console.log(greet("Muhammad"));
```

**Default class export**

```javascript
// user.js

export default class User {
    constructor(name) {
        this.name = name;
    }

    greet() {
        return `Hello, ${this.name}!`;
    }
}
```

Importing it:

```javascript
import User from "./user.js";

const user = new User("Muhammad");

console.log(user.greet());
```

A module can have only one default export.

# 🗁 Default Export Expressions

A default export can also export an expression or an existing value.

```javascript
const appName = "My Application";

export default appName;
```

It can also export an object:

```javascript
export default {
    name: "My Application",
    version: "1.0.0",
    environment: "development"
};
```

Or an arrow function:

```javascript
const greet = (name) => {
    return `Hello, ${name}!`;
};

export default greet;
```

# 🗁 Default Export Anonymous Functions and Classes

Default exports can use anonymous function or class declarations.

**Anonymous function**

```javascript
export default function (name) {
    return `Hello, ${name}!`;
}
```

**Anonymous class**

```javascript
export default class {
    constructor(name) {
        this.name = name;
    }
}
```

The importing module chooses the local identifier:

```javascript
import greet from "./greeting.js";

import User from "./user.js";
```

# 🗁 Combining Named and Default Exports

A module can contain one default export and multiple named exports.

```javascript
// user.js

export const ROLE = "Developer";

export const STATUS = "Active";

export function validateUser(user) {
    return Boolean(user?.name);
}

export default class User {
    constructor(name) {
        this.name = name;
    }
}
```

Importing both types:

```javascript
import User, {
    ROLE,
    STATUS,
    validateUser
} from "./user.js";

const user = new User("Muhammad");

console.log(ROLE);
console.log(STATUS);
console.log(validateUser(user));
```

This pattern is useful when a module provides a primary class or function alongside supporting constants and utilities.

# 🗁 Re-Exports

A module can re-export declarations from another module without implementing them itself.

This is useful for creating centralized entry points.

**Example structure**

```text
project/
├── app.js
└── modules/
    ├── math.js
    ├── string.js
    └── index.js
```

`math.js`

```javascript
export function add(a, b) {
    return a + b;
}

export function subtract(a, b) {
    return a - b;
}
```

`string.js`

```javascript
export function capitalize(value) {
    return value.charAt(0).toUpperCase() + value.slice(1);
}
```

`index.js`

```javascript
export { add, subtract } from "./math.js";

export { capitalize } from "./string.js";
```

`app.js`

```javascript
import {
    add,
    subtract,
    capitalize
} from "./modules/index.js";

console.log(add(10, 5));
console.log(subtract(10, 5));
console.log(capitalize("javascript"));
```

Instead of importing directly from multiple files, consumers can import from a single entry point.

# 🗁 Exporting Everything from Another Module

The `export * from` syntax re-exports all named exports from another module.

```javascript
// index.js

export * from "./math.js";
export * from "./string.js";
```

Consumers can import the named exports from `index.js`:

```javascript
import {
    add,
    subtract,
    capitalize
} from "./index.js";
```

Important: `export *` does not automatically re-export the default export.

To re-export a default export as a named export:

```javascript
export { default as User } from "./user.js";
```

To re-export a default export as the default export:

```javascript
export { default } from "./user.js";
```

# 🗁 Renaming Re-Exports

Re-exported declarations can be given different public names.

```javascript
export {
    add as sum,
    subtract as difference
} from "./math.js";
```

The original module remains unchanged.

Consumers use the new exported names:

```javascript
import {
    sum,
    difference
} from "./index.js";
```

# 🗁 Exporting Only Selected Declarations

A module can expose only specific declarations while keeping implementation details private.

```javascript
// user.js

const SECRET_KEY = "internal-value";

function generateToken() {
    return "token-123";
}

export function createUser(name) {
    const token = generateToken();

    return {
        name,
        token
    };
}
```

Only `createUser` is exported.

Other modules cannot directly import `SECRET_KEY` or `generateToken`.

This is useful for encapsulating internal implementation details.

# 🗁 Live Bindings

Named exports provide live bindings.

An importing module receives access to the exported binding rather than an independent copy.

```javascript
// counter.js

export let count = 0;

export function increment() {
    count++;
}

export function reset() {
    count = 0;
}
```

`app.js`

```javascript
import {
    count,
    increment,
    reset
} from "./counter.js";

console.log(count);

increment();

console.log(count);

reset();

console.log(count);
```

**Output**

```text
0
1
0
```

The importing module observes changes made by the exporting module.

However, imported bindings cannot be reassigned by the importing module.

```javascript
import { count } from "./counter.js";

// Invalid:
count = 10;
```

# 🗁 Exporting Mutable and Immutable Values

JavaScript modules can export declarations using `const`, `let`, and functions.

**Using const**

```javascript
export const API_URL = "https://api.example.com";
```

The exported binding cannot be reassigned within its module.

**Using let**

```javascript
export let counter = 0;

export function increment() {
    counter++;
}
```

The exporting module can update the binding.

**Using functions**

```javascript
export function calculateTotal(price, quantity) {
    return price * quantity;
}
```

Functions are commonly exported as reusable module utilities.

# 🗁 Exporting Objects

Objects can be exported as named or default exports.

**Named object export**

```javascript
export const config = {
    appName: "My Application",
    version: "1.0.0"
};
```

Importing:

```javascript
import { config } from "./config.js";

console.log(config.appName);
```

**Default object export**

```javascript
const config = {
    appName: "My Application",
    version: "1.0.0"
};

export default config;
```

Importing:

```javascript
import config from "./config.js";
```

The importing module can access and modify mutable object properties, subject to the normal JavaScript object rules.

A `const` export prevents reassignment of the binding, not mutation of the referenced object.

# 🗁 Exporting Asynchronous Functions

Asynchronous functions can be exported like ordinary functions.

```javascript
// userService.js

export async function getUsers() {
    const response = await fetch("/api/users");

    if (!response.ok) {
        throw new Error("Failed to fetch users");
    }

    return response.json();
}
```

Importing and using the function:

```javascript
import { getUsers } from "./userService.js";

async function loadUsers() {
    try {
        const users = await getUsers();

        console.log(users);
    } catch (error) {
        console.error(error);
    }
}

loadUsers();
```

The export mechanism does not change how asynchronous functions behave.

# 🗁 Exporting Types of Declarations

ES modules support exporting many JavaScript declarations.

| Declaration | Export Supported |
| --- | --- |
| `const` | Yes |
| `let` | Yes |
| `var` | Yes |
| Function | Yes |
| Async function | Yes |
| Generator function | Yes |
| Class | Yes |
| Object | Yes |
| Array | Yes |
| Symbol | Yes |
| Promise | Yes |

Example:

```javascript
export const name = "JavaScript";

export let version = 2026;

export var language = "Programming";

export function greet() {
    return "Hello";
}

export async function loadData() {
    return await Promise.resolve("Loaded");
}

export class Developer {
    constructor(name) {
        this.name = name;
    }
}
```

# 🗁 Export Restrictions

ES modules have some important export rules.

**Only one default export per module**

Invalid:

```javascript
export default function greet() {}

export default class User {}
```

A module cannot contain multiple default exports.

**Duplicate exported names are not allowed**

Invalid:

```javascript
const value = 10;

export { value };

export { value };
```

The same export name cannot be declared repeatedly in the same module export list.

**Static export declarations must be at the top level**

Invalid:

```javascript
if (true) {
    export const value = 10;
}
```

Valid:

```javascript
export const value = 10;
```

**Imported bindings cannot be reassigned**

```javascript
import { count } from "./counter.js";

// Invalid:
count = 20;
```

The exporting module controls reassignment of its exported binding.

# 🗁 Common Export Mistakes

**Forgetting the export keyword**

Incorrect:

```javascript
function add(a, b) {
    return a + b;
}
```

Correct:

```javascript
export function add(a, b) {
    return a + b;
}
```

**Confusing named and default exports**

Named export:

```javascript
export function greet() {}
```

Named import:

```javascript
import { greet } from "./greeting.js";
```

Default export:

```javascript
export default function greet() {}
```

Default import:

```javascript
import greet from "./greeting.js";
```

The syntax must match the exported type.

**Trying to export inside a function**

Incorrect:

```javascript
function setup() {
    export const value = 10;
}
```

Export declarations must be at the module's top level.

**Expecting export star to include default exports**

Incorrect assumption:

```javascript
export * from "./user.js";
```

This does not re-export the default export.

Use:

```javascript
export { default } from "./user.js";
```

**Exporting unnecessary implementation details**

Avoid exposing every internal variable and helper function.

Export only the functionality that other modules actually need.

# 🗁 Best Practices

* Export only declarations that should form part of a module's public interface.
* Use named exports for modules containing multiple related utilities.
* Use default exports when a module has one primary value or responsibility.
* Prefer explicit re-exports when creating centralized module entry points.
* Avoid unnecessary `export *` statements that can create naming conflicts.
* Use descriptive and consistent exported names.
* Keep implementation details private whenever possible.
* Avoid creating circular dependencies through unnecessary re-exports.
* Use live bindings carefully when sharing mutable module state.
* Document public module interfaces when building reusable libraries.

# 🗁 Quick Reference

| Export Type | Syntax |
| --- | --- |
| Named function | `export function greet() {}` |
| Named constant | `export const PI = 3.14` |
| Named class | `export class User {}` |
| Export list | `export { greet, User }` |
| Export alias | `export { greet as sayHello }` |
| Default function | `export default function greet() {}` |
| Default class | `export default class User {}` |
| Default value | `export default config` |
| Re-export named | `export { add } from "./math.js"` |
| Re-export all named | `export * from "./math.js"` |
| Re-export default as named | `export { default as User } from "./user.js"` |
| Re-export default | `export { default } from "./user.js"` |

# 🗁 Practice Exercises

* Create a mathematical module with named exports.
* Export constants, functions, and classes from one module.
* Export multiple declarations using an export list.
* Rename exported declarations using aliases.
* Create a module with a default class export.
* Combine named and default exports in one module.
* Create a centralized `index.js` module using re-exports.
* Re-export selected declarations from multiple modules.
* Re-export all named declarations from another module.
* Re-export a default export under a named identifier.
* Demonstrate live bindings using an exported counter.
* Create a module that exposes public functions while keeping helper functions private.
* Build a small application using a centralized module entry point.

# 🗁 Key Takeaways

* `export` makes declarations available to other modules.
* Named exports allow multiple declarations to be exposed by name.
* Default exports provide one primary exported value per module.
* Export lists allow declarations to be exported after their definitions.
* The `as` keyword supports export aliases.
* Modules can re-export declarations from other modules.
* `export *` re-exports named exports, not default exports.
* Exported bindings can be live and reflect changes made by the exporting module.
* Imported bindings cannot be reassigned by importing modules.
* Export declarations must appear at the module's top level.
* A well-designed module exposes a clear public interface while keeping internal implementation details private.
