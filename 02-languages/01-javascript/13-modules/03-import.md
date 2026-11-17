# JavaScript Import

The `import` statement is used to access functionality exported by another JavaScript module.

It allows developers to reuse variables, functions, classes, objects, and other declarations without duplicating their implementations.

JavaScript ES modules support several import patterns, including named imports, default imports, namespace imports, side-effect imports, and aliased imports.

# 🗁 Import Syntax

The basic syntax for importing named exports is:

```javascript
import { name } from "./module.js";
```

* `import` — keyword used to import functionality.
* `{ name }` — exported declaration to import.
* `"./module.js"` — module specifier identifying the source module.

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

# 🗁 Named Imports

Named imports allow you to import specific declarations exported by name.

**Module**

```javascript
// user.js

export const username = "Muhammad";

export const role = "Developer";

export function greet() {
    return `Hello, ${username}!`;
}

export class User {
    constructor(name) {
        this.name = name;
    }
}
```

**Importing declarations**

```javascript
import {
    username,
    role,
    greet,
    User
} from "./user.js";

console.log(username);
console.log(role);
console.log(greet());

const user = new User("Alex");

console.log(user.name);
```

Named imports must match the corresponding exported names unless aliases are used.

# 🗁 Importing Multiple Declarations

Multiple named exports can be imported using a single import statement.

```javascript
import {
    add,
    subtract,
    multiply,
    divide
} from "./math.js";
```

This is generally preferable to writing separate import statements for every declaration from the same module.

You can also organize imports across multiple lines for readability.

# 🗁 Import Aliases

The `as` keyword allows an imported declaration to have a different local name.

**Syntax**

```javascript
import {
    originalName as localName
} from "./module.js";
```

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
import {
    add as sum,
    subtract as difference
} from "./math.js";

console.log(sum(10, 5));
console.log(difference(10, 5));
```

**Output**

```text
15
5
```

Import aliases are useful when:

* Multiple modules export declarations with identical names.
* A more descriptive local name is preferred.
* An imported declaration conflicts with an existing local identifier.

# 🗁 Default Imports

A default import accesses the default export of another module.

**Module**

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

**Import**

```javascript
import User from "./user.js";

const user = new User("Muhammad");

console.log(user.greet());
```

Unlike named imports, a default import does not require curly braces.

The local name can be chosen by the importing module.

```javascript
import ApplicationUser from "./user.js";
```

Both `User` and `ApplicationUser` refer to the same default export.

# 🗁 Combining Default and Named Imports

A module can provide both a default export and named exports.

**Module**

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

**Import**

```javascript
import User, {
    ROLE,
    validateUser
} from "./user.js";

const user = new User("Muhammad");

console.log(ROLE);
console.log(validateUser(user));
```

The default import appears before the named imports.

# 🗁 Namespace Imports

Namespace imports collect all named exports from a module into one namespace object.

**Syntax**

```javascript
import * as namespace from "./module.js";
```

**Example**

`math.js`

```javascript
export const PI = 3.14159;

export function add(a, b) {
    return a + b;
}

export function multiply(a, b) {
    return a * b;
}
```

`app.js`

```javascript
import * as math from "./math.js";

console.log(math.PI);
console.log(math.add(10, 5));
console.log(math.multiply(10, 5));
```

**Output**

```text
3.14159
15
50
```

Namespace imports are useful when:

* A module exports many related declarations.
* You want to group imported functionality under one identifier.
* You want to avoid conflicts between imported names.

The namespace object also provides access to a default export through its `default` property, if one exists.

```javascript
import * as userModule from "./user.js";

const User = userModule.default;
```

# 🗁 Side-Effect Imports

A side-effect import loads and evaluates a module without importing any particular declaration.

**Syntax**

```javascript
import "./module.js";
```

**Example**

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

**Output**

```text
Logger initialized
[APP]: Application started
```

Side-effect imports are commonly used for:

* Application initialization.
* Polyfills.
* Global styles in supported build systems.
* Event registration.
* Plugin registration.

Because side-effect imports execute module code, they should be used deliberately.

# 🗁 Importing from Nested Directories

Relative module paths can reference files in different directories.

**Project structure**

```text
project/
├── app.js
├── utils/
│   ├── math.js
│   └── string.js
└── services/
    └── user.js
```

Importing from a nested directory:

```javascript
import { add } from "./utils/math.js";

import { capitalize } from "./utils/string.js";

import { getUser } from "./services/user.js";
```

Relative paths are resolved from the importing module's location.

# 🗁 Relative Module Specifiers

Relative imports use path prefixes to identify modules.

| Prefix | Meaning | Example |
| --- | --- | --- |
| `./` | Current directory | `./math.js` |
| `../` | Parent directory | `../utils/math.js` |
| `/` | Root-relative URL in browser module resolution | `/modules/math.js` |

**Current directory**

```javascript
import { add } from "./math.js";
```

**Parent directory**

```javascript
import { add } from "../math.js";
```

**Root-relative browser URL**

```javascript
import { add } from "/modules/math.js";
```

A leading `/` in a browser module specifier refers to the origin's URL root, not necessarily the application's source-code root.

# 🗁 Absolute and Package Specifiers

Not all module specifiers use relative paths.

A package or bare specifier identifies a module by name.

```javascript
import React from "react";

import express from "express";
```

These are examples of package imports commonly used in development environments.

Node.js resolves package specifiers through its package resolution rules.

Browsers generally require an import map or a bundler/development server to resolve bare package specifiers.

**Browser import map example**

```html
<script type="importmap">
{
    "imports": {
        "math": "./modules/math.js"
    }
}
</script>

<script type="module">
    import { add } from "math";

    console.log(add(10, 20));
</script>
```

Import maps allow browsers to associate module specifiers with module URLs.

# 🗁 Importing JSON Modules

JSON module imports allow JavaScript applications to consume JSON data through module syntax.

In supported browser environments, JSON modules use import attributes:

```javascript
import config from "./config.json" with {
    type: "json"
};

console.log(config);
```

Node.js also supports JSON modules with import attributes in current releases.

The exact compatibility requirements depend on the JavaScript runtime and version.

For environments where JSON module imports are unavailable or unsuitable, `fetch()` or `JSON.parse()` can be used instead.

# 🗁 Dynamic Import vs Static Import

Static imports and dynamic imports both load modules, but they serve different purposes.

**Static import**

```javascript
import { add } from "./math.js";

console.log(add(10, 20));
```

Static imports are declared at the module's top level and are analyzed as part of the module dependency graph.

**Dynamic import**

```javascript
async function loadMath() {
    const math = await import("./math.js");

    console.log(math.add(10, 20));
}

loadMath();
```

Dynamic imports return a promise and can be used conditionally or at runtime.

Use static imports for regular dependencies and dynamic imports when code should be loaded on demand.

# 🗁 Import Declaration Placement

Static import declarations must appear at the top level of a module.

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

Static imports can appear after other top-level statements syntactically, but placing them at the beginning of a module improves readability.

Dynamic `import()` can be used inside functions, conditions, and other expressions.

```javascript
async function loadFeature(enabled) {
    if (enabled) {
        const feature = await import("./feature.js");

        feature.run();
    }
}
```

# 🗁 Imported Bindings Are Read-Only

An imported binding cannot be reassigned from the importing module.

`counter.js`

```javascript
export let count = 0;

export function increment() {
    count++;
}
```

`app.js`

```javascript
import { count, increment } from "./counter.js";

increment();

console.log(count);

// Invalid:
// count = 10;
```

The imported `count` binding reflects changes made by the exporting module, but the importing module cannot directly reassign it.

This behavior is known as a live binding.

# 🗁 Importing the Same Module Multiple Times

A module can be imported by multiple files.

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

When all imports resolve to the same module identity, the module is evaluated once and its shared state is reused.

This behavior is useful for shared application state and reusable module-level services.

# 🗁 Common Import Errors

**Incorrect named import**

```javascript
import { sum } from "./math.js";
```

If the module does not export `sum`, the import fails.

**Missing relative path prefix**

```javascript
import { add } from "math.js";
```

This is interpreted as a bare module specifier, not a relative file path.

Correct:

```javascript
import { add } from "./math.js";
```

**Incorrect capitalization**

```javascript
import { Add } from "./math.js";
```

JavaScript identifiers are case-sensitive.

If the exported name is `add`, the imported name must match.

**Missing file extension**

```javascript
import { add } from "./math";
```

Browser module resolution generally requires the correct file extension.

Correct:

```javascript
import { add } from "./math.js";
```

**Using static imports inside functions**

```javascript
function loadModule() {
    import { add } from "./math.js";
}
```

Static imports must be declared at the module's top level.

Use dynamic `import()` instead.

# 🗁 Best Practices

* Use named imports when only specific declarations are needed.
* Use namespace imports when grouping many related exports improves clarity.
* Use aliases to resolve naming conflicts.
* Keep import paths clear and consistent.
* Prefer static imports for regular application dependencies.
* Use dynamic imports for conditional or on-demand loading.
* Avoid unnecessary side-effect imports.
* Group imports from the same module together.
* Keep imports organized by external packages, internal modules, and relative files.
* Avoid importing declarations that are not used.
* Do not attempt to reassign imported bindings.
* Use explicit file extensions where required by the runtime.

# 🗁 Quick Reference

| Import Type | Syntax |
| --- | --- |
| Named import | `import { add } from "./math.js"` |
| Multiple named imports | `import { add, subtract } from "./math.js"` |
| Aliased import | `import { add as sum } from "./math.js"` |
| Default import | `import User from "./user.js"` |
| Combined import | `import User, { ROLE } from "./user.js"` |
| Namespace import | `import * as math from "./math.js"` |
| Side-effect import | `import "./logger.js"` |
| Dynamic import | `import("./math.js")` |

# 🗁 Practice Exercises

* Import multiple named functions from a mathematical utility module.
* Import a default-exported class from another module.
* Combine default and named imports in one statement.
* Rename imported declarations using aliases.
* Import an entire module using namespace imports.
* Create a side-effect module that initializes application logging.
* Import modules from nested directories.
* Configure a browser import map and use a bare module specifier.
* Import a JSON module in a supported runtime.
* Demonstrate live bindings by modifying an exported variable.
* Build a small application using multiple modules and organized imports.
* Compare static imports with dynamic imports.

# 🗁 Key Takeaways

* `import` accesses declarations exported by another module.
* Named imports use curly braces and must match exported names.
* Default imports do not require curly braces.
* Aliases allow imported declarations to use different local names.
* Namespace imports group named exports into one object.
* Side-effect imports execute a module without importing a declaration.
* Relative module paths identify local files.
* Bare specifiers commonly identify packages.
* Static imports must appear at the module's top level.
* Dynamic `import()` supports runtime module loading.
* Imported bindings are read-only live bindings.
* Module loaders reuse the same module instance for imports resolving to the same module identity.
