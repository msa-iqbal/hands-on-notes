# JavaScript Module Basics

JavaScript modules provide a way to organize code into separate, reusable, and maintainable files.

Instead of writing an entire application in one large JavaScript file, modules allow developers to divide functionality into smaller units with clearly defined responsibilities.

Each module can contain variables, functions, classes, and other declarations that can be shared with other modules through imports and exports.

Modules are fundamental to modern JavaScript application development.

# 🗁 What Are JavaScript Modules?

A JavaScript module is an independent file containing code that can be imported into other files.

For example, consider an application with two files:

```text
project/
├── index.html
└── app.js
```

The `app.js` file contains the application's JavaScript code.

As an application grows, its functionality can be separated into multiple modules:

```text
project/
├── index.html
├── app.js
└── modules/
    ├── math.js
    ├── user.js
    └── logger.js
```

Each module handles a particular responsibility.

* `math.js` — mathematical operations.
* `user.js` — user-related functionality.
* `logger.js` — logging utilities.
* `app.js` — application entry point.

# 🗁 Why Use Modules?

Modules help solve several problems associated with large JavaScript applications.

**Code organization**

Related functionality can be grouped into separate files.

**Code reusability**

A function or class can be imported into multiple modules without rewriting its implementation.

**Maintainability**

Smaller files are easier to understand, debug, test, and modify.

**Encapsulation**

Module-scoped declarations are not automatically exposed to the global scope.

**Dependency management**

Modules explicitly declare which functionality they depend on.

**Separation of concerns**

Different parts of an application can have independent responsibilities.

# 🗁 JavaScript Module Scope

Every JavaScript module has its own module scope.

Variables and functions declared inside a module are not automatically available in other modules or the global scope.

Consider this file:

`math.js`

```javascript
const taxRate = 0.15;

function calculateTax(amount) {
    return amount * taxRate;
}
```

The declarations `taxRate` and `calculateTax` belong to the module scope.

Another JavaScript module cannot directly access them unless they are exported.

This prevents accidental global variable conflicts.

# 🗁 Import and Export

JavaScript modules use two primary mechanisms to share functionality.

* `export` — makes declarations available to other modules.
* `import` — accesses declarations exported by another module.

**Example**

`math.js`

```javascript
export function add(a, b) {
    return a + b;
}

export function subtract(a, b) {
    return a - b;
}
```

`app.js`

```javascript
import { add, subtract } from "./math.js";

console.log(add(10, 5));
console.log(subtract(10, 5));
```

**Output**

```text
15
5
```

The functions are exported from `math.js` and imported into `app.js`.

The detailed import and export mechanisms are covered in separate topics.

# 🗁 Named Exports

A module can export multiple declarations by name.

```javascript
// math.js

export const PI = 3.14159;

export function square(number) {
    return number * number;
}

export function cube(number) {
    return number * number * number;
}
```

Another module can import the required declarations:

```javascript
import { PI, square, cube } from "./math.js";

console.log(PI);
console.log(square(4));
console.log(cube(3));
```

Named exports are useful when a module provides multiple related utilities.

# 🗁 Default Exports

A module can also provide one primary default export.

```javascript
// greeting.js

export default function greet(name) {
    return `Hello, ${name}!`;
}
```

Importing the default export:

```javascript
import greet from "./greeting.js";

console.log(greet("Muhammad"));
```

A module can have only one default export.

Named exports and default exports can also coexist in the same module.

# 🗁 Module Loading

JavaScript modules are loaded and evaluated by the JavaScript host environment.

The host environment determines how module files are located and loaded.

Common environments include:

* Web browsers.
* Node.js.
* JavaScript bundlers.
* Development tools and build systems.

Modules generally follow a dependency graph.

For example:

```text
app.js
 ├── math.js
 ├── user.js
 │    └── validation.js
 └── logger.js
```

Here, `app.js` depends on three modules, and `user.js` also depends on `validation.js`.

The module system resolves these dependencies so that required code can be used.

# 🗁 Static Module Structure

ES modules use static `import` and `export` declarations.

These declarations must appear at the top level of a module rather than inside ordinary functions, loops, or conditional blocks.

Valid:

```javascript
import { add } from "./math.js";

function calculate() {
    return add(10, 20);
}
```

Invalid:

```javascript
function calculate() {
    import { add } from "./math.js";
}
```

The static structure allows JavaScript tools and engines to analyze module dependencies before executing the module.

For conditional or runtime-dependent loading, JavaScript provides dynamic `import()`.

# 🗁 Module Evaluation

JavaScript modules are evaluated according to their dependency relationships.

A dependency is generally evaluated before the module that imports it.

Consider:

`logger.js`

```javascript
console.log("Logger module loaded");
```

`app.js`

```javascript
import "./logger.js";

console.log("Application started");
```

**Output**

```text
Logger module loaded
Application started
```

The imported module is evaluated before the importing module's body executes.

A module is normally evaluated only once per module identity within a given module loader environment. Subsequent imports reuse its evaluated module instance.

# 🗁 Module Strict Mode

JavaScript modules automatically execute in strict mode.

You do not need to write `"use strict"` at the beginning of a module.

For example:

```javascript
// app.js

"use strict";

function calculate() {
    return 10 + 20;
}
```

The `"use strict"` directive is unnecessary because modules already enforce strict mode.

Strict mode helps prevent certain unsafe JavaScript behaviors and makes some programming errors easier to detect.

# 🗁 Modules vs Traditional Scripts

JavaScript files can be executed as traditional scripts or as modules.

| Feature | Traditional Script | ES Module |
| --- | --- | --- |
| Scope | Global or function scope | Module scope |
| Strict mode | Not automatic | Automatic |
| Import/export | Not supported directly | Supported |
| Top-level `this` | Usually global object | `undefined` |
| Dependency management | Usually manual | Built-in |
| Deferred execution in browsers | Depends on script attributes | Deferred by default |
| Module caching | Not applicable in the same way | Module instances are reused |

A traditional script can expose declarations globally, while module declarations remain private unless explicitly exported.

# 🗁 Using Modules in Browsers

Browsers support ES modules through the `type="module"` attribute on script elements.

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
    <title>JavaScript Modules</title>
</head>
<body>
    <h1>JavaScript Modules</h1>

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

The browser loads `app.js` as an ES module and resolves its dependency on `math.js`.

**Important browser considerations**

* Module scripts are deferred automatically.
* Module scripts use CORS-based fetching rules.
* Module imports generally require valid URL paths.
* Browser modules should be served through an HTTP server.
* Opening module-based applications directly using the `file://` protocol can cause loading restrictions.

# 🗁 Modules in Node.js

Node.js supports two major JavaScript module systems:

* ECMAScript Modules (ESM).
* CommonJS (CJS).

**ES Modules**

```javascript
import { readFile } from "node:fs/promises";

const content = await readFile("example.txt", "utf8");

console.log(content);
```

**CommonJS**

```javascript
const fs = require("node:fs");

const content = fs.readFileSync("example.txt", "utf8");

console.log(content);
```

ES modules use `import` and `export`, while CommonJS traditionally uses `require()` and `module.exports`.

Node.js determines how JavaScript files are interpreted using factors such as file extensions and the nearest applicable `package.json` configuration.

# 🗁 Common Module File Extensions

| Extension | Typical Purpose |
| --- | --- |
| `.js` | JavaScript file; interpretation depends on environment and configuration |
| `.mjs` | Explicit ES module file in Node.js |
| `.cjs` | Explicit CommonJS file in Node.js |

In browser applications, `.js` files can be used as modules when loaded with `type="module"`.

# 🗁 Common Module Mistakes

**Forgetting to export a declaration**

```javascript
// math.js

function add(a, b) {
    return a + b;
}
```

Attempting to import `add` from this module will fail because it is not exported.

Correct:

```javascript
export function add(a, b) {
    return a + b;
}
```

**Using an incorrect module path**

```javascript
import { add } from "math.js";
```

This is not generally equivalent to importing a relative file.

Correct:

```javascript
import { add } from "./math.js";
```

**Using static imports conditionally**

```javascript
if (true) {
    import { add } from "./math.js";
}
```

Static imports cannot appear inside conditional blocks.

Use dynamic `import()` when conditional loading is required.

**Running browser modules directly from the filesystem**

Opening an HTML file through `file://` may prevent module dependencies from loading.

Use a local development server instead.

**Confusing ESM and CommonJS**

```javascript
import { add } from "./math.js";
```

and:

```javascript
const { add } = require("./math.js");
```

belong to different module systems and are not interchangeable in every environment.

# 🗁 Best Practices

* Keep each module focused on a clear responsibility.
* Use descriptive filenames.
* Prefer explicit named exports for reusable utilities.
* Use default exports when a module has one primary responsibility or value.
* Keep module dependencies understandable and organized.
* Avoid unnecessary circular dependencies.
* Avoid exposing internal implementation details.
* Prefer relative paths for local module dependencies.
* Use consistent module conventions throughout a project.
* Separate browser-specific, server-specific, and shared functionality when appropriate.
* Use dynamic imports when code should be loaded conditionally or on demand.

# 🗁 Quick Reference

| Concept | Description |
| --- | --- |
| Module | Independent JavaScript file |
| Module scope | Scope isolated to a module |
| `export` | Exposes declarations |
| `import` | Accesses exported declarations |
| Named export | Exports declarations by name |
| Default export | Provides one primary default export |
| Static import | Declares dependencies at module scope |
| Dynamic import | Loads a module at runtime |
| ESM | ECMAScript Module system |
| CommonJS | Node.js-oriented module system |

# 🗁 Practice Exercises

* Create a `math.js` module with addition, subtraction, multiplication, and division functions.
* Import those functions into an `app.js` module.
* Create a `user.js` module containing a user object and related functions.
* Export a constant and import it into another module.
* Create a module with one default export.
* Create a module that combines named and default exports.
* Build a small browser application using multiple ES modules.
* Create a module dependency structure with at least three levels.
* Observe module evaluation order using console messages.
* Compare the behavior of traditional scripts and ES modules.

# 🗁 Key Takeaways

* JavaScript modules organize code into independent files.
* Modules improve maintainability, reusability, and encapsulation.
* Every ES module has its own module scope.
* `export` exposes functionality to other modules.
* `import` makes exported functionality available.
* ES modules execute in strict mode automatically.
* Static imports are declared at the top level.
* Dynamic imports support runtime module loading.
* Browsers and Node.js support ES modules.
* Node.js also supports the CommonJS module system.
