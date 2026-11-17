# ECMAScript Modules (ES Modules)

ECMAScript Modules (ES Modules or ESM) are the standardized JavaScript module system introduced in ECMAScript 2015 (ES6).

ES modules provide a built-in mechanism for dividing JavaScript applications into independent files and sharing functionality through `import` and `export`.

They are supported by modern browsers, Node.js, and JavaScript development tools.

ES modules are the foundation of modern JavaScript application architecture.

# 🗁 What Are ES Modules?

An ES module is a JavaScript file that uses the ECMAScript module system.

A file becomes an ES module when it is loaded or interpreted as a module by its host environment.

For example:

```javascript
// math.js

export function add(a, b) {
    return a + b;
}
```

Another module can import the exported function:

```javascript
// app.js

import { add } from "./math.js";

console.log(add(10, 20));
```

**Output**

```text
30
```

The `math.js` file provides functionality, while `app.js` consumes it.

# 🗁 ES Module Characteristics

ES modules have several important characteristics:

* Each module has its own scope.
* Modules execute in strict mode automatically.
* Imports and exports are statically analyzable.
* Imported bindings are live bindings.
* Module dependencies are resolved before evaluation.
* A module is normally evaluated once per module identity within a module loader environment.
* Modules support asynchronous loading.
* Modules can use top-level `await` in supported environments.

These characteristics make ES modules suitable for large applications and complex dependency structures.

# 🗁 Enabling ES Modules in Browsers

Browsers support ES modules through the `type="module"` attribute.

**Project structure**

```text
project/
├── index.html
├── app.js
└── math.js
```

`index.html`

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ES Modules</title>
</head>
<body>
    <h1>ES Modules Example</h1>

    <script type="module" src="./app.js"></script>
</body>
</html>
```

`math.js`

```javascript
export function add(a, b) {
    return a + b;
}
```

`app.js`

```javascript
import { add } from "./math.js";

console.log(add(10, 20));
```

The browser loads `app.js` as a module and resolves its dependency on `math.js`.

**Important:** Module scripts should generally be served over HTTP or HTTPS rather than opened directly through the `file://` protocol.

# 🗁 Enabling ES Modules in Node.js

Node.js supports ES modules through multiple mechanisms.

**Using the `.mjs` extension**

```text
project/
├── app.mjs
└── math.mjs
```

`math.mjs`

```javascript
export function add(a, b) {
    return a + b;
}
```

`app.mjs`

```javascript
import { add } from "./math.mjs";

console.log(add(10, 20));
```

Run the application:

```bash
node app.mjs
```

**Using package.json**

Alternatively, use `.js` files with the package configuration:

```json
{
    "type": "module"
}
```

Now `.js` files in the package scope are interpreted as ES modules.

Example:

```text
project/
├── package.json
├── app.js
└── math.js
```

Run:

```bash
node app.js
```

The `.mjs` extension explicitly identifies an ES module, while `.cjs` explicitly identifies a CommonJS module.

# 🗁 Named Exports

Named exports allow a module to expose multiple declarations.

```javascript
// math.js

export const PI = 3.14159;

export function add(a, b) {
    return a + b;
}

export function subtract(a, b) {
    return a - b;
}

export function multiply(a, b) {
    return a * b;
}
```

Import them using their exported names:

```javascript
import { PI, add, subtract, multiply } from "./math.js";

console.log(PI);
console.log(add(10, 5));
console.log(subtract(10, 5));
console.log(multiply(10, 5));
```

Named imports must match the exported names unless an alias is used.

# 🗁 Default Exports

A module can provide one default export.

```javascript
// user.js

export default class User {
    constructor(name) {
        this.name = name;
    }

    greet() {
        return `Hello, ${this.name}`;
    }
}
```

Importing the default export:

```javascript
import User from "./user.js";

const user = new User("Muhammad");

console.log(user.greet());
```

The importing module can choose any local name for a default export.

# 🗁 Combining Named and Default Exports

A module can contain both named exports and a default export.

```javascript
// user.js

export const ROLE = "Developer";

export function validateUser(user) {
    return Boolean(user?.name);
}

export default class User {
    constructor(name) {
        this.name = name;
    }
}
```

Import both kinds:

```javascript
import User, {
    ROLE,
    validateUser
} from "./user.js";

const user = new User("Muhammad");

console.log(ROLE);
console.log(validateUser(user));
```

The default import is written separately from the named imports.

# 🗁 Import Aliases

Aliases allow imported declarations to use different local names.

```javascript
import {
    add as sum,
    subtract as difference
} from "./math.js";

console.log(sum(10, 5));
console.log(difference(10, 5));
```

Aliases are useful when:

* Two modules export declarations with identical names.
* A local name should be more descriptive.
* An imported name conflicts with an existing declaration.

# 🗁 Export Aliases

Export aliases allow declarations to be exposed under different names.

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

Consumers import the exported names:

```javascript
import {
    sum,
    difference
} from "./math.js";
```

# 🗁 Import Everything with Namespace Imports

Namespace imports collect all named exports into a single namespace object.

```javascript
import * as math from "./math.js";

console.log(math.add(10, 5));
console.log(math.subtract(10, 5));
console.log(math.PI);
```

The namespace object exposes the module's exported bindings.

A default export, when present, is available through the namespace object's `default` property.

```javascript
import * as userModule from "./user.js";

const User = userModule.default;
```

# 🗁 Side-Effect Imports

A module can be imported without importing any particular declaration.

```javascript
import "./logger.js";
```

This loads and evaluates the module for its side effects.

For example:

`logger.js`

```javascript
console.log("Logger initialized");

window.appLogger = {
    log(message) {
        console.log(`[APP]: ${message}`);
    }
};
```

`app.js`

```javascript
import "./logger.js";

window.appLogger.log("Application started");
```

Side-effect imports are useful for initialization code, polyfills, and modules that register functionality.

However, unnecessary global modifications should generally be avoided.

# 🗁 Live Bindings

ES modules use live bindings for imported values.

An import does not create an independent copy of an exported variable. It provides access to the exported binding.

**Example**

`counter.js`

```javascript
export let count = 0;

export function increment() {
    count++;
}
```

`app.js`

```javascript
import {
    count,
    increment
} from "./counter.js";

console.log(count);

increment();

console.log(count);
```

**Output**

```text
0
1
```

The imported `count` reflects the updated exported value.

Imported bindings are read-only from the importing module.

This is invalid:

```javascript
import { count } from "./counter.js";

count = 10;
```

Only the exporting module can reassign its own exported binding.

# 🗁 Static Module Structure

ES modules use static import and export declarations.

They must appear at the top level of the module.

Valid:

```javascript
import { add } from "./math.js";

function calculate() {
    return add(10, 20);
}
```

Invalid:

```javascript
if (true) {
    import { add } from "./math.js";
}
```

Static module declarations allow JavaScript engines and development tools to analyze dependencies before execution.

When conditional loading is necessary, use dynamic `import()`.

# 🗁 Module Dependency Graph

ES modules form a dependency graph based on their imports.

Consider:

```text
app.js
├── math.js
│   └── constants.js
├── user.js
│   └── validation.js
└── logger.js
```

The graph describes how modules depend on one another.

A module loader resolves these dependencies and evaluates modules according to their relationships.

Understanding dependency graphs is important when designing large applications.

# 🗁 Module Execution Order

A module's dependencies are generally evaluated before its own module body.

Example:

`logger.js`

```javascript
console.log("Logger initialized");
```

`math.js`

```javascript
console.log("Math initialized");

export function add(a, b) {
    return a + b;
}
```

`app.js`

```javascript
import "./logger.js";
import { add } from "./math.js";

console.log("Application started");

console.log(add(10, 20));
```

**Output**

```text
Logger initialized
Math initialized
Application started
30
```

The dependencies are evaluated before the importing module's body executes.

For complex graphs, shared dependencies are generally evaluated only once per module identity.

# 🗁 Module Caching

Module loaders maintain module records so that the same module is not repeatedly evaluated for every import.

For example:

`counter.js`

```javascript
console.log("Counter module initialized");

export let count = 0;

export function increment() {
    count++;
}
```

`first.js`

```javascript
import { increment } from "./counter.js";

increment();
```

`second.js`

```javascript
import { count } from "./counter.js";

console.log(count);
```

`app.js`

```javascript
import "./first.js";
import "./second.js";
```

Assuming all imports resolve to the same module identity, `counter.js` is evaluated once and its shared state is reused.

This behavior is useful for shared application state and singleton-like module patterns.

# 🗁 Top-Level Await

ES modules support top-level `await` in environments that implement it.

It allows asynchronous operations directly at the module's top level without wrapping them in an async function.

```javascript
const response = await fetch("/api/users");

const users = await response.json();

console.log(users);
```

Top-level `await` makes the module's evaluation asynchronous.

Modules that depend on this module must wait for its asynchronous evaluation to complete.

Use it thoughtfully because it can affect application startup and dependent modules.

# 🗁 Circular Dependencies

A circular dependency occurs when two or more modules depend on one another.

Example:

`a.js`

```javascript
import { getB } from "./b.js";

export function getA() {
    return "A";
}

export function callB() {
    return getB();
}
```

`b.js`

```javascript
import { getA } from "./a.js";

export function getB() {
    return `B depends on ${getA()}`;
}
```

Both modules depend on each other.

ES modules support circular dependencies, but initialization order and access to bindings matter.

Accessing a lexical binding before it has been initialized can cause a `ReferenceError`.

Circular dependencies can make applications harder to understand and maintain.

A better architecture often separates shared functionality into a third module.

```text
a.js ─────┐
          ├── shared.js
b.js ─────┘
```

# 🗁 ES Modules vs CommonJS

Node.js supports both ES modules and CommonJS.

| Feature | ES Modules | CommonJS |
| --- | --- | --- |
| Import syntax | `import` | `require()` |
| Export syntax | `export` | `module.exports` |
| Standard | ECMAScript standard | Traditionally Node.js-specific |
| Dependency structure | Statically analyzable | Primarily runtime-based |
| Imported values | Live bindings | Typically exported values or object references |
| Top-level `this` | `undefined` | Module-specific object |
| Dynamic loading | `import()` | `require()` |
| Top-level await | Supported | Not supported in ordinary CommonJS modules |

**ES Modules**

```javascript
import { add } from "./math.js";

export function calculate() {
    return add(10, 20);
}
```

**CommonJS**

```javascript
const { add } = require("./math.cjs");

function calculate() {
    return add(10, 20);
}

module.exports = {
    calculate
};
```

ES modules are the standardized choice for modern JavaScript development.

# 🗁 Common ES Module Errors

**Missing file extension**

In browser ES modules, relative imports generally require the correct file extension.

Incorrect:

```javascript
import { add } from "./math";
```

Correct:

```javascript
import { add } from "./math.js";
```

**Incorrect named import**

```javascript
import { sum } from "./math.js";
```

If `math.js` does not export `sum`, the import fails.

**Using import in a CommonJS context**

A Node.js file interpreted as CommonJS cannot use static ES module import syntax directly.

Configure the package for ESM or use the `.mjs` extension.

**Circular initialization errors**

Accessing an imported lexical binding before initialization can produce a `ReferenceError`.

Review the dependency graph and initialization order.

**Incorrect browser server configuration**

A server that responds with an inappropriate MIME type for JavaScript modules can prevent module loading.

Configure the server to serve JavaScript files with an appropriate JavaScript MIME type.

# 🗁 Best Practices

* Use ES modules for modern JavaScript applications.
* Keep each module focused on a specific responsibility.
* Prefer named exports for modules containing multiple related utilities.
* Use default exports when a module has one primary export.
* Avoid unnecessary circular dependencies.
* Keep dependency graphs understandable.
* Avoid unnecessary side effects during module initialization.
* Use explicit file extensions in browser module imports.
* Use dynamic imports for conditional or on-demand loading.
* Use top-level `await` only when asynchronous module initialization is appropriate.
* Keep ESM and CommonJS conventions consistent within each package.
* Avoid unnecessary global variables and global namespace modifications.

# 🗁 Practice Exercises

* Create a browser application using ES modules.
* Create a mathematical utility module with named exports.
* Create a user module with a default class export.
* Import multiple modules into one application entry point.
* Use namespace imports to access several utilities.
* Rename imported declarations using aliases.
* Create a side-effect module that initializes application logging.
* Demonstrate live bindings by updating an exported variable.
* Build a module dependency graph with at least four modules.
* Create and resolve a circular dependency.
* Use top-level `await` to load configuration data.
* Convert a small CommonJS project into ES modules.

# 🗁 Key Takeaways

* ES modules are the standardized JavaScript module system.
* They use `import` and `export` to manage dependencies.
* Every module has its own scope and automatic strict mode.
* Named and default exports provide different ways to expose functionality.
* Imported values use live bindings.
* Static imports must appear at the module's top level.
* Dynamic imports support runtime loading.
* Module dependencies form a dependency graph.
* Modules are generally evaluated once per module identity.
* Top-level `await` enables asynchronous module initialization.
* Circular dependencies are supported but require careful initialization management.
* ES modules are supported by modern browsers and Node.js.
