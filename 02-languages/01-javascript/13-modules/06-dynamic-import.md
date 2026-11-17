# JavaScript Dynamic Import

Dynamic import is a JavaScript feature that allows modules to be loaded on demand rather than being loaded immediately through static `import` declarations.

It uses the `import()` function-like syntax and returns a Promise that resolves to a module namespace object.

Dynamic imports are particularly useful for:

* Lazy loading application features.
* Reducing initial JavaScript bundle size.
* Loading modules conditionally.
* Loading code after a user interaction.
* Implementing route-based code splitting.
* Loading optional dependencies.
* Improving application startup performance.

# 🗁 What Is Dynamic Import?

Static imports are declared at the top level of a module and are resolved as part of the module's dependency graph.

Dynamic imports allow a module to be loaded when the application actually needs it.

**Static import**

```javascript
import { calculateTotal } from "./calculator.js";

console.log(calculateTotal(100, 2));
```

The dependency is declared statically.

**Dynamic import**

```javascript
import("./calculator.js")
    .then((module) => {
        console.log(module.calculateTotal(100, 2));
    });
```

The module is loaded dynamically, and the returned Promise provides access to its exports.

# 🗁 Dynamic Import Syntax

The general syntax is:

```javascript
import(moduleSpecifier);
```

The argument identifies the module to load.

It can be a string literal or an expression that evaluates to a module specifier.

**Using a string literal**

```javascript
import("./utils.js");
```

**Using a variable**

```javascript
const modulePath = "./utils.js";

import(modulePath);
```

**Using a template literal**

```javascript
const feature = "dashboard";

import(`./features/${feature}.js`);
```

Dynamic import returns a Promise.

```javascript
const modulePromise = import("./utils.js");

console.log(modulePromise instanceof Promise);
```

# 🗁 Importing Named Exports Dynamically

When a module contains named exports, access them through the resolved module namespace object.

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
import("./math.js")
    .then((math) => {
        console.log(math.add(10, 5));
        console.log(math.subtract(10, 5));
    });
```

**Output**

```text
15
5
```

The resolved object contains the module's exported bindings.

# 🗁 Importing Default Exports Dynamically

A default export is available through the `default` property of the resolved module namespace object.

`greeting.js`

```javascript
export default function greet(name) {
    return `Hello, ${name}!`;
}
```

`app.js`

```javascript
import("./greeting.js")
    .then((module) => {
        console.log(module.default("Muhammad"));
    });
```

**Output**

```text
Hello, Muhammad!
```

The `default` property contains the module's default export.

# 🗁 Dynamic Import with Async/Await

Dynamic imports work naturally with `async` and `await`.

```javascript
async function loadCalculator() {
    const calculator = await import("./calculator.js");

    console.log(calculator.add(10, 20));
}

loadCalculator();
```

This approach often produces cleaner code than chaining multiple `.then()` calls.

**Importing a default export**

```javascript
async function loadGreeting() {
    const module = await import("./greeting.js");

    const greet = module.default;

    console.log(greet("Alex"));
}

loadGreeting();
```

**Destructuring named exports**

```javascript
async function loadMath() {
    const { add, subtract } = await import("./math.js");

    console.log(add(20, 10));
    console.log(subtract(20, 10));
}

loadMath();
```

# 🗁 Dynamic Import with Conditional Loading

A module can be loaded only when a particular condition is satisfied.

```javascript
const isAdmin = true;

async function initializeApplication() {
    if (isAdmin) {
        const adminModule = await import("./admin.js");

        adminModule.initializeAdminPanel();
    } else {
        const userModule = await import("./user.js");

        userModule.initializeUserPanel();
    }
}

initializeApplication();
```

Only the selected module is requested by this code path.

This is useful when different users or application states require different functionality.

# 🗁 Dynamic Import on User Interaction

Dynamic imports can load functionality after a user performs an action.

For example, a large editor module does not need to load until the user clicks an Edit button.

```javascript
const editButton = document.querySelector("#editButton");

editButton.addEventListener("click", async () => {
    const editor = await import("./editor.js");

    editor.initializeEditor();
});
```

`editor.js`

```javascript
export function initializeEditor() {
    console.log("Editor initialized.");
}
```

This pattern is commonly used for optional features, dialogs, editors, charts, and other components that users may not immediately need.

# 🗁 Dynamic Import with Error Handling

Because dynamic import returns a Promise, loading failures can be handled with `.catch()` or `try...catch`.

**Using Promise catch**

```javascript
import("./dashboard.js")
    .then((module) => {
        module.initializeDashboard();
    })
    .catch((error) => {
        console.error("Module loading failed:", error);
    });
```

**Using try...catch**

```javascript
async function loadDashboard() {
    try {
        const dashboard = await import("./dashboard.js");

        dashboard.initializeDashboard();
    } catch (error) {
        console.error("Module loading failed:", error);
    }
}

loadDashboard();
```

Potential failures include:

* An invalid module specifier.
* A module that cannot be resolved.
* A network failure when loading modules in a browser.
* A syntax error while evaluating the imported module.
* An exception thrown during module evaluation.

# 🗁 Dynamic Import with Variable Paths

A module specifier can be constructed dynamically.

```javascript
async function loadLanguage(language) {
    const module = await import(`./languages/${language}.js`);

    module.initialize();
}

loadLanguage("en");
```

This can be useful for language modules, themes, and feature modules.

However, dynamically constructed paths should be controlled carefully.

For example, avoid allowing arbitrary external input to determine unrestricted module paths:

```javascript
const modulePath = userInput;

await import(modulePath);
```

Prefer mapping supported values to known module paths:

```javascript
const languageModules = {
    en: "./languages/en.js",
    fr: "./languages/fr.js",
    de: "./languages/de.js"
};

async function loadLanguage(language) {
    const modulePath = languageModules[language];

    if (!modulePath) {
        throw new Error("Unsupported language.");
    }

    return import(modulePath);
}
```

This makes supported modules explicit and prevents unexpected module resolution.

# 🗁 Dynamic Import with Promise Methods

Since dynamic import returns a Promise, standard Promise methods can be used.

**Using `then()`**

```javascript
import("./user.js")
    .then((module) => {
        console.log(module.default);
    });
```

**Using `catch()`**

```javascript
import("./user.js")
    .catch((error) => {
        console.error(error);
    });
```

**Using `finally()`**

```javascript
import("./user.js")
    .finally(() => {
        console.log("Import operation finished.");
    });
```

**Using `Promise.all()`**

Multiple modules can be loaded concurrently.

```javascript
async function loadApplicationModules() {
    const [userModule, productModule, orderModule] =
        await Promise.all([
            import("./user.js"),
            import("./product.js"),
            import("./order.js")
        ]);

    console.log(userModule);
    console.log(productModule);
    console.log(orderModule);
}

loadApplicationModules();
```

`Promise.all()` rejects if any of the supplied Promises rejects.

# 🗁 Dynamic Import with Promise.allSettled()

When each module should be handled independently, use `Promise.allSettled()`.

```javascript
async function loadModules() {
    const results = await Promise.allSettled([
        import("./user.js"),
        import("./product.js"),
        import("./analytics.js")
    ]);

    for (const result of results) {
        if (result.status === "fulfilled") {
            console.log("Module loaded:", result.value);
        } else {
            console.error("Module failed:", result.reason);
        }
    }
}

loadModules();
```

Unlike `Promise.all()`, `Promise.allSettled()` waits for every import operation to settle, whether fulfilled or rejected.

# 🗁 Dynamic Import in Browser Applications

Dynamic imports can be used directly in browser JavaScript modules.

Example project structure:

```text
project/
├── index.html
├── main.js
└── modules/
    ├── home.js
    └── settings.js
```

`index.html`

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Dynamic Import</title>
</head>
<body>
    <button id="homeButton">Home</button>
    <button id="settingsButton">Settings</button>

    <script type="module" src="./main.js"></script>
</body>
</html>
```

`main.js`

```javascript
const homeButton = document.querySelector("#homeButton");
const settingsButton = document.querySelector("#settingsButton");

homeButton.addEventListener("click", async () => {
    const home = await import("./modules/home.js");

    home.render();
});

settingsButton.addEventListener("click", async () => {
    const settings = await import("./modules/settings.js");

    settings.render();
});
```

`modules/home.js`

```javascript
export function render() {
    console.log("Home page loaded.");
}
```

`modules/settings.js`

```javascript
export function render() {
    console.log("Settings page loaded.");
}
```

The browser loads `main.js` initially. The other modules are requested when their corresponding buttons are clicked.

For browser-native ES modules, serve the application through an HTTP server rather than relying on direct `file://` access.

# 🗁 Dynamic Import and Lazy Loading

Lazy loading means delaying the loading of a resource until it is needed.

Dynamic import provides a native JavaScript mechanism for lazy-loading modules.

**Without lazy loading**

```javascript
import { initializeChart } from "./chart.js";
import { initializeEditor } from "./editor.js";
import { initializeMap } from "./map.js";

initializeChart();
```

All three dependencies belong to the initial static module dependency graph.

**With lazy loading**

```javascript
async function openChart() {
    const chart = await import("./chart.js");

    chart.initializeChart();
}

async function openEditor() {
    const editor = await import("./editor.js");

    editor.initializeEditor();
}

async function openMap() {
    const map = await import("./map.js");

    map.initializeMap();
}
```

Each feature can be loaded when required.

Lazy loading can reduce the amount of JavaScript needed during initial application startup.

# 🗁 Dynamic Import and Code Splitting

Code splitting divides application code into smaller bundles or chunks.

Dynamic imports provide an important splitting point for modern bundlers.

For example:

```javascript
async function loadReports() {
    const reports = await import("./features/reports.js");

    reports.initializeReports();
}
```

A bundler such as Vite, webpack, or Rollup can use this dynamic import boundary to generate a separate chunk.

The generated chunk can then be downloaded when the dynamic import executes.

Important distinction:

* Dynamic import defines a runtime module-loading boundary.
* Code splitting is a build-tool optimization that may use that boundary.
* Native browser modules can also load modules dynamically without a bundler.

# 🗁 Static Import vs Dynamic Import

| Feature | Static Import | Dynamic Import |
| --- | --- | --- |
| Syntax | `import { value } from "./module.js"` | `import("./module.js")` |
| Loading | Part of the static dependency graph | Requested when executed |
| Return value | No Promise | Promise |
| Usage | Top-level module declarations | Expressions and runtime logic |
| Conditional loading | Not directly supported | Supported |
| Lazy loading | Not inherently | Commonly used |
| Error handling | Module loading affects dependency graph | Promise rejection can be handled |
| Code splitting | Bundler-dependent | Common splitting boundary |

**Static import example**

```javascript
import { add } from "./math.js";

console.log(add(10, 20));
```

**Dynamic import example**

```javascript
const math = await import("./math.js");

console.log(math.add(10, 20));
```

Use static imports for dependencies that are always needed. Use dynamic imports when loading should happen conditionally or on demand.

# 🗁 Dynamic Import vs CommonJS require()

Dynamic import is part of the JavaScript module system, while `require()` belongs to the CommonJS module system.

**Dynamic import**

```javascript
async function loadModule() {
    const module = await import("./module.js");

    console.log(module.default);
}
```

**CommonJS require**

```javascript
const module = require("./module.js");

console.log(module);
```

| Feature | Dynamic Import | CommonJS require |
| --- | --- | --- |
| Module system | ECMAScript Modules | CommonJS |
| Loading behavior | Asynchronous | Synchronous |
| Return value | Promise | Module exports |
| Conditional loading | Supported | Supported |
| Browser support | Native in modern browsers | Requires an appropriate runtime or bundler |
| Default export access | `module.default` | Depends on the exported value and interoperability |

Dynamic import can also be used in CommonJS environments that support ECMAScript dynamic import.

# 🗁 Dynamic Import in Node.js

Node.js supports dynamic import for loading ECMAScript modules.

```javascript
async function loadModule() {
    const module = await import("./utils.mjs");

    console.log(module.add(10, 20));
}

loadModule();
```

Dynamic import is also supported in CommonJS files:

```javascript
async function loadESModule() {
    const module = await import("./utils.mjs");

    console.log(module.default);
}

loadESModule();
```

This provides a way for CommonJS code to load ECMAScript modules.

# 🗁 Module Namespace Object

Dynamic import resolves to a module namespace object.

This object provides access to the module's exported bindings.

`user.js`

```javascript
export const name = "Muhammad";

export const role = "Developer";

export default function greet() {
    return `Hello, ${name}!`;
}
```

`app.js`

```javascript
const module = await import("./user.js");

console.log(module.name);
console.log(module.role);
console.log(module.default());
```

**Output**

```text
Muhammad
Developer
Hello, Muhammad!
```

The namespace object contains:

* `name`: the named export.
* `role`: the named export.
* `default`: the default export.

The module namespace object is not an ordinary mutable object. Its exported bindings are read-only from the importing module's perspective.

# 🗁 Dynamic Import and Module Caching

JavaScript module systems generally cache successfully loaded modules.

Repeated imports of the same resolved module generally reuse the same module instance rather than evaluating it again.

`counter.js`

```javascript
console.log("Module evaluated.");

export let count = 0;

export function increment() {
    count++;
}
```

`app.js`

```javascript
const first = await import("./counter.js");
const second = await import("./counter.js");

first.increment();

console.log(first.count);
console.log(second.count);
```

**Output**

```text
Module evaluated.
1
1
```

Both imports refer to the same module instance and its live exported bindings.

Module caching is useful for preserving shared module state and avoiding repeated evaluation.

# 🗁 Common Mistakes

**Forgetting to await the Promise**

Incorrect:

```javascript
const module = import("./math.js");

console.log(module.add(10, 20));
```

`module` is a Promise, not the resolved namespace object.

Correct:

```javascript
const module = await import("./math.js");

console.log(module.add(10, 20));
```

**Accessing a default export incorrectly**

Incorrect:

```javascript
const greet = await import("./greeting.js");

greet("Alex");
```

Correct:

```javascript
const module = await import("./greeting.js");

module.default("Alex");
```

Or:

```javascript
const { default: greet } = await import("./greeting.js");

greet("Alex");
```

**Ignoring import failures**

Incorrect:

```javascript
async function loadFeature() {
    const feature = await import("./feature.js");

    feature.initialize();
}
```

If loading fails, the function rejects.

Handle expected failures:

```javascript
async function loadFeature() {
    try {
        const feature = await import("./feature.js");

        feature.initialize();
    } catch (error) {
        console.error("Unable to load feature:", error);
    }
}
```

**Using unsupported or incorrect module paths**

Incorrect:

```javascript
await import("./modules/unknown.js");
```

If the module does not exist or cannot be resolved, the import rejects.

Verify module paths and filenames.

**Assuming dynamic import always creates a separate bundle**

Dynamic import does not guarantee a separate output file.

A bundler may combine modules, optimize chunks, or change the generated output according to its configuration.

# 🗁 Best Practices

* Use dynamic imports for features that are not immediately required.
* Prefer static imports for core dependencies needed during initial execution.
* Use `async` and `await` when it improves readability.
* Handle import failures with appropriate error handling.
* Use `Promise.all()` when independent modules should load concurrently.
* Use `Promise.allSettled()` when each module should be handled independently.
* Keep dynamically constructed module paths controlled and predictable.
* Use dynamic imports to establish useful code-splitting boundaries.
* Avoid excessive code splitting that creates unnecessary network requests.
* Remember that modules are generally cached after successful loading.
* Measure application performance before and after introducing lazy loading.

# 🗁 Quick Reference

| Operation | Syntax |
| --- | --- |
| Dynamic import | `import("./module.js")` |
| Import with `then()` | `import("./module.js").then(module => {})` |
| Import with `await` | `const module = await import("./module.js")` |
| Named export access | `module.name` |
| Default export access | `module.default` |
| Destructure named export | `const { add } = await import("./math.js")` |
| Destructure default export | `const { default: User } = await import("./user.js")` |
| Conditional import | `if (condition) await import("./feature.js")` |
| Error handling | `try { await import("./module.js") } catch (error) {}` |
| Concurrent imports | `Promise.all([import("./a.js"), import("./b.js")])` |
| Independent import results | `Promise.allSettled([import("./a.js"), import("./b.js")])` |

# 🗁 Practice Exercises

* Create a module containing named exports and load it dynamically.
* Create a module with a default export and access it through `module.default`.
* Load a module using `async` and `await`.
* Load different modules based on a condition.
* Load a module after a button click.
* Implement dynamic language loading for an application.
* Load multiple independent modules using `Promise.all()`.
* Handle module loading failures with `try...catch`.
* Create a feature that loads only when a user opens a specific section.
* Build a small application with lazy-loaded dashboard and settings modules.
* Compare static imports and dynamic imports in a browser application.
* Inspect how a bundler generates chunks from dynamic import expressions.

# 🗁 Key Takeaways

* Dynamic import loads JavaScript modules on demand.
* It uses the `import()` syntax.
* It returns a Promise that resolves to a module namespace object.
* Named exports are accessed through their export names.
* Default exports are accessed through the `default` property.
* Dynamic imports work with `async` and `await`.
* They support conditional loading and lazy loading.
* Modern bundlers use dynamic imports as code-splitting boundaries.
* Dynamic imports are supported in browsers and Node.js.
* They are useful for improving application loading performance and organizing optional functionality.
