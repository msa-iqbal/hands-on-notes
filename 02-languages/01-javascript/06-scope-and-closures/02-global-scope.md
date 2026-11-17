# Global Scope

**Global scope** is the outermost scope in a JavaScript execution environment. Variables and functions declared in the global scope can generally be accessed from other scopes within the same environment.

Global scope is useful for sharing application-wide values and functionality. However, excessive use of global variables can lead to naming conflicts, unexpected modifications, and tightly coupled code.

# 🗁 Understanding Global Scope

A variable declared outside functions and blocks can belong to the global scope.

```javascript
const applicationName = "My Application";
let version = "1.0.0";

function displayInfo() {
  console.log(applicationName);
  console.log(version);
}

displayInfo();

// Accessible from the global scope
console.log(applicationName);
console.log(version);
```

Both variables can be accessed inside `displayInfo()` because the function can resolve identifiers from its outer lexical environment.

# 🗁 Global Variables

Global variables are identifiers declared in the global scope.

```javascript
const siteName = "Example Website";

function showSiteName() {
  console.log(siteName);
}

function updatePageTitle() {
  document.title = siteName;
}

showSiteName();
updatePageTitle();
```

Both functions can access `siteName` without receiving it as a parameter.

Global variables are commonly used for:

* Application-wide configuration.
* Shared constants.
* Global utilities.
* Environment-level APIs.
* Application initialization.

However, dependencies become less explicit when many functions rely on global variables.

# 🗁 Global Scope in Browser JavaScript

Browser JavaScript has a global environment associated with the browser's global object, commonly accessible through `globalThis` or `window`.

In classic browser scripts, top-level `var` declarations create properties on the global object.

```javascript
var language = "JavaScript";

console.log(window.language); // JavaScript
console.log(globalThis.language); // JavaScript
```

Top-level `let` and `const` declarations behave differently.

```javascript
let framework = "React";
const runtime = "Node.js";

console.log(window.framework); // undefined
console.log(window.runtime);  // undefined
```

These lexical declarations belong to the global lexical environment, not the global object.

# 🗁 Global Scope in Node.js

Node.js has a different module system and global environment from browser classic scripts.

In CommonJS modules, top-level variables are scoped to the current module.

```javascript
// app.js

const applicationName = "My Node Application";

function initialize() {
  console.log(applicationName);
}

initialize();
```

The `applicationName` variable is not automatically available in other CommonJS modules.

Node.js provides `globalThis` as the standard cross-environment reference to the global object.

```javascript
globalThis.applicationName = "My Application";

console.log(globalThis.applicationName);
```

The global object can be accessed from different modules within the same Node.js environment.

In Node.js ES modules, top-level declarations are also module-scoped rather than automatically global.

# 🗁 The `globalThis` Object

`globalThis` provides a standard way to access the global object across JavaScript environments.

It works in:

* Browsers.
* Node.js.
* Web Workers.
* Other modern JavaScript environments.

```javascript
console.log(globalThis);
```

You can explicitly create a global property:

```javascript
globalThis.appConfig = {
  name: "My Application",
  version: "1.0.0",
};

console.log(globalThis.appConfig.name);
```

This makes `appConfig` accessible through the global object.

For application architecture, explicit module exports and dependency injection are generally preferable to creating unnecessary global properties.

# 🗁 Global Variables vs Global Object Properties

A global binding and a property on the global object are related but not identical.

In browser classic scripts:

```javascript
var first = "JavaScript";
let second = "TypeScript";
const third = "Python";

console.log(first); // JavaScript
console.log(window.first); // JavaScript

console.log(second); // TypeScript
console.log(window.second); // undefined

console.log(third); // Python
console.log(window.third); // undefined
```

The distinction is:

| Declaration | Global Binding | Global Object Property |
| --- | --- | --- |
| Top-level `var` in classic browser script | Yes | Yes |
| Top-level `let` in classic browser script | Yes | No |
| Top-level `const` in classic browser script | Yes | No |
| Top-level declaration in ES module | Module-scoped | No |
| Explicit assignment to `globalThis` | Global object property | Yes |

The behavior of global declarations depends on the execution environment and script type.

# 🗁 Implicit Global Variables

In non-strict JavaScript, assigning a value to an undeclared identifier can create a property on the global object.

```javascript
function createGlobal() {
  applicationStatus = "Running";
}

createGlobal();

console.log(globalThis.applicationStatus);
```

This is an implicit global assignment.

It is problematic because the variable is created without an explicit declaration.

Strict mode prevents this behavior.

```javascript
"use strict";

function createGlobal() {
  applicationStatus = "Running"; // ReferenceError
}

createGlobal();
```

JavaScript modules are automatically strict, so implicit global assignments are also rejected in modules.

Always declare variables using `const` or `let`.

# 🗁 Global Namespace Pollution

Global namespace pollution occurs when too many variables, functions, or properties are added to the global environment.

Example:

```javascript
var userName = "Alice";
var userRole = "Admin";
var userStatus = "Active";

function login() {
  console.log("User logged in");
}

function logout() {
  console.log("User logged out");
}
```

As an application grows, global identifiers can conflict with identifiers introduced by other scripts or libraries.

This can cause:

* Naming collisions.
* Unexpected value changes.
* Difficult debugging.
* Hidden dependencies.
* Poor modularity.

# 🗁 Preventing Global Namespace Pollution

**Use Function Scope**

```javascript
(function () {
  const userName = "Alice";
  const userRole = "Admin";

  function login() {
    console.log(`${userName} logged in`);
  }

  login();
})();
```

The internal identifiers are isolated inside the function.

**Use Block Scope**

```javascript
{
  const userName = "Alice";
  const userRole = "Admin";

  console.log(userName);
}
```

Variables declared with `let` and `const` remain inside the block.

**Use ES Modules**

```javascript
// user.js

const userName = "Alice";

export function getUserName() {
  return userName;
}
```

```javascript
// app.js

import { getUserName } from "./user.js";

console.log(getUserName());
```

Modules provide their own scope and expose only explicitly exported functionality.

**Use Namespaces**

A namespace groups related functionality under a single object.

```javascript
const UserService = {
  login() {
    console.log("User logged in");
  },

  logout() {
    console.log("User logged out");
  },
};

UserService.login();
UserService.logout();
```

This example creates one global binding, `UserService`, rather than several unrelated global bindings when placed in a classic script's global scope.

# 🗁 Global Constants and Configuration

Global constants can be useful for values shared across an application.

```javascript
const APP_CONFIG = {
  name: "My Application",
  version: "1.0.0",
  environment: "development",
};

function displayConfig() {
  console.log(APP_CONFIG.name);
  console.log(APP_CONFIG.version);
}

displayConfig();
```

The `const` declaration prevents reassignment of the `APP_CONFIG` binding, but it does not make the object immutable.

```javascript
APP_CONFIG.version = "2.0.0";

console.log(APP_CONFIG.version); // 2.0.0
```

If mutation must be prevented at runtime for a shallow object, use `Object.freeze()`.

```javascript
const APP_CONFIG = Object.freeze({
  name: "My Application",
  version: "1.0.0",
});

APP_CONFIG.version = "2.0.0";

console.log(APP_CONFIG.version); // 1.0.0
```

`Object.freeze()` is shallow. Nested objects require separate freezing if deep immutability is needed.

# 🗁 Global Scope and Script Loading

The way JavaScript files are loaded affects global declarations.

**Classic Script**

```html
<script src="app.js"></script>
```

Top-level `var` declarations in a classic browser script can become global object properties.

**ES Module**

```html
<script type="module" src="app.js"></script>
```

Top-level declarations in an ES module are scoped to that module.

Example:

```javascript
// app.js

const message = "Hello from module";

console.log(message);
```

The `message` variable is not automatically available as a global identifier in other scripts.

# 🗁 Best Practices

* Avoid unnecessary global variables.
* Prefer module scope for application code.
* Use `const` for values that should not be reassigned.
* Use `let` when reassignment is required.
* Never rely on implicit global assignments.
* Use `globalThis` when explicit access to the global object is genuinely required.
* Keep shared configuration centralized and well-defined.
* Use namespaces or modules to organize related functionality.
* Avoid modifying built-in global objects such as `Array`, `Object`, and `Promise`.
* Make dependencies explicit through function parameters or module imports.

# 🗁 Summary

* Global scope is the outermost scope in a JavaScript execution environment.
* Global bindings can be accessed from nested scopes when identifier resolution reaches them.
* Browser classic scripts and JavaScript modules have different global declaration behavior.
* Top-level `var` in classic browser scripts creates a global object property.
* Top-level `let` and `const` in classic browser scripts create global lexical bindings instead.
* Node.js CommonJS and ES modules use module-level scope for top-level declarations.
* `globalThis` provides access to the global object across modern JavaScript environments.
* Excessive global variables cause namespace pollution and hidden dependencies.
* ES modules, block scope, and function scope help keep application code isolated.

# 🗁 Practice Exercises

* Declare a global variable and access it from multiple functions.
* Compare top-level `var`, `let`, and `const` in a browser classic script.
* Access the global object using `globalThis`.
* Create an explicit global property using `globalThis`.
* Demonstrate an implicit global assignment in a non-strict script.
* Repeat the implicit assignment example in strict mode and observe the error.
* Create a namespace object that groups user-related functions.
* Create a configuration object and protect its top-level properties using `Object.freeze()`.
* Compare global declarations in a classic browser script and an ES module.
* Create two JavaScript modules and demonstrate how exports provide controlled access to functionality.
