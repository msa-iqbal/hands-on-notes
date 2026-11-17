# JavaScript CommonJS

CommonJS (CJS) is a JavaScript module system originally designed for environments such as Node.js.

It provides a way to organize JavaScript applications into separate files and share functionality between them.

CommonJS uses two primary mechanisms:

* `require()` to import modules.
* `module.exports` and `exports` to export functionality.

Although modern JavaScript supports the standardized ECMAScript Module (ESM) system, CommonJS remains widely used in the Node.js ecosystem, particularly in existing projects and packages.

# 🗁 What Is CommonJS?

CommonJS is a module specification that defines how JavaScript files can expose and consume functionality.

Each CommonJS module has its own scope and receives access to module-related variables such as:

* `module`
* `exports`
* `require`
* `__filename`
* `__dirname`

These variables are provided by the CommonJS module system in Node.js.

**Example**

`math.js`

```javascript
function add(a, b) {
    return a + b;
}

module.exports = {
    add
};
```

`app.js`

```javascript
const math = require("./math");

console.log(math.add(10, 20));
```

**Output**

```text
30
```

The `math.js` file exports functionality, and `app.js` imports and uses it.

# 🗁 CommonJS Module Structure

A typical CommonJS project may look like this:

```text
project/
├── package.json
├── app.js
├── math.js
└── utils/
    └── logger.js
```

Each JavaScript file can act as an independent module.

A module can export functions, classes, objects, arrays, or other values.

# 🗁 module.exports

`module.exports` defines the value that a CommonJS module exposes to other modules.

It can export a single function:

```javascript
// greeting.js

function greet(name) {
    return `Hello, ${name}!`;
}

module.exports = greet;
```

Importing:

```javascript
const greet = require("./greeting");

console.log(greet("Muhammad"));
```

**Output**

```text
Hello, Muhammad!
```

It can also export an object containing multiple values:

```javascript
// math.js

function add(a, b) {
    return a + b;
}

function subtract(a, b) {
    return a - b;
}

module.exports = {
    add,
    subtract
};
```

Importing:

```javascript
const math = require("./math");

console.log(math.add(10, 5));
console.log(math.subtract(10, 5));
```

**Output**

```text
15
5
```

# 🗁 Exporting Multiple Values

A CommonJS module can expose multiple functions, variables, and classes through an object.

```javascript
// user.js

const ROLE = "Developer";

function createUser(name, email) {
    return {
        name,
        email,
        role: ROLE
    };
}

function validateUser(user) {
    return Boolean(user?.name && user?.email);
}

class UserService {
    findUser(id) {
        return {
            id,
            name: "Muhammad"
        };
    }
}

module.exports = {
    ROLE,
    createUser,
    validateUser,
    UserService
};
```

Importing:

```javascript
const userModule = require("./user");

const user = userModule.createUser(
    "Muhammad",
    "user@example.com"
);

console.log(userModule.ROLE);
console.log(userModule.validateUser(user));

const service = new userModule.UserService();

console.log(service.findUser(1));
```

# 🗁 The exports Object

CommonJS provides an `exports` variable as a reference to the initial `module.exports` object.

This allows properties to be added directly.

```javascript
// math.js

exports.add = function (a, b) {
    return a + b;
};

exports.subtract = function (a, b) {
    return a - b;
};
```

Importing:

```javascript
const math = require("./math");

console.log(math.add(10, 5));
console.log(math.subtract(10, 5));
```

Both `module.exports` and `exports` can expose module functionality, but they are not interchangeable in every situation.

# 🗁 Difference Between exports and module.exports

Initially, Node.js establishes a relationship similar to:

```javascript
exports = module.exports;
```

Both refer to the same object initially.

**Adding properties through exports**

```javascript
exports.name = "JavaScript";
```

This works because the property is added to the shared object.

**Adding properties through module.exports**

```javascript
module.exports.name = "JavaScript";
```

This also works.

**Reassigning exports**

```javascript
exports = {
    name: "JavaScript"
};
```

This does not replace the exported module value.

It only reassigns the local `exports` variable.

**Reassigning module.exports**

```javascript
module.exports = {
    name: "JavaScript"
};
```

This replaces the value exposed by the module.

The key rule is:

* Use `exports.property` to add properties to the existing export object.
* Use `module.exports` when replacing the entire exported value.

# 🗁 Exporting a Single Function

A module can export a function directly.

```javascript
// calculator.js

module.exports = function calculateTotal(price, quantity) {
    return price * quantity;
};
```

Importing:

```javascript
const calculateTotal = require("./calculator");

console.log(calculateTotal(100, 3));
```

**Output**

```text
300
```

The imported variable directly references the exported function.

# 🗁 Exporting a Class

Classes can also be exported through `module.exports`.

```javascript
// User.js

class User {
    constructor(name, email) {
        this.name = name;
        this.email = email;
    }

    getProfile() {
        return {
            name: this.name,
            email: this.email
        };
    }
}

module.exports = User;
```

Importing:

```javascript
const User = require("./User");

const user = new User(
    "Muhammad",
    "user@example.com"
);

console.log(user.getProfile());
```

**Output**

```javascript
{
    name: "Muhammad",
    email: "user@example.com"
}
```

# 🗁 Exporting an Object

An object can be exported directly.

```javascript
// config.js

module.exports = {
    appName: "My Application",
    version: "1.0.0",
    environment: "development",
    debug: true
};
```

Importing:

```javascript
const config = require("./config");

console.log(config.appName);
console.log(config.version);
console.log(config.environment);
```

This pattern is commonly used for configuration modules.

# 🗁 Importing Modules with require()

The `require()` function loads a CommonJS module and returns its exported value.

Syntax:

```javascript
const moduleName = require("module-specifier");
```

**Importing a local module**

```javascript
const math = require("./math");
```

**Importing a Node.js built-in module**

```javascript
const fs = require("node:fs");
```

**Importing an installed package**

```javascript
const express = require("express");
```

The module specifier determines what Node.js resolves.

# 🗁 Named Properties and Destructuring

When a module exports an object, individual properties can be imported through destructuring.

`math.js`

```javascript
function add(a, b) {
    return a + b;
}

function subtract(a, b) {
    return a - b;
}

module.exports = {
    add,
    subtract
};
```

`app.js`

```javascript
const { add, subtract } = require("./math");

console.log(add(10, 5));
console.log(subtract(10, 5));
```

Destructuring provides convenient access to exported properties.

# 🗁 Importing a Module Multiple Times

CommonJS modules are cached after their first successful loading.

When the same resolved module is required again, Node.js normally returns the cached export instead of executing the module again.

`counter.js`

```javascript
console.log("Counter module loaded.");

let count = 0;

function increment() {
    count++;
}

function getCount() {
    return count;
}

module.exports = {
    increment,
    getCount
};
```

`app.js`

```javascript
const first = require("./counter");
const second = require("./counter");

first.increment();

console.log(first.getCount());
console.log(second.getCount());
```

**Output**

```text
Counter module loaded.
1
1
```

The module is evaluated once, and both variables reference the same cached module exports.

# 🗁 CommonJS Module Caching

Node.js maintains a module cache that can be inspected through `require.cache`.

```javascript
const math = require("./math");

console.log(require.cache);
```

A particular module can be inspected using its resolved filename:

```javascript
const modulePath = require.resolve("./math");

console.log(require.cache[modulePath]);
```

A cached module can be removed:

```javascript
delete require.cache[require.resolve("./math")];
```

The next `require()` of that module will load and evaluate it again.

This technique is mainly useful for specialized development and testing scenarios. It should not be used casually in normal application code because it can create multiple instances of modules and unexpected shared-state behavior.

# 🗁 require.resolve()

`require.resolve()` resolves a module specifier into an absolute filename without loading the module itself.

```javascript
const resolvedPath = require.resolve("./math");

console.log(resolvedPath);
```

This can be useful for inspecting module resolution and working with the CommonJS module cache.

It throws an error if the requested module cannot be resolved.

# 🗁 __filename

`__filename` contains the absolute path of the current CommonJS module file.

```javascript
console.log(__filename);
```

Example output:

```text
/home/user/project/app.js
```

It is useful when working with files relative to the current module.

# 🗁 __dirname

`__dirname` contains the absolute path of the directory containing the current CommonJS module.

```javascript
console.log(__dirname);
```

Example output:

```text
/home/user/project
```

It is commonly used with Node.js path utilities.

```javascript
const path = require("node:path");

const configPath = path.join(
    __dirname,
    "config",
    "settings.json"
);

console.log(configPath);
```

# 🗁 Built-in Node.js Modules

CommonJS can load Node.js built-in modules through `require()`.

**File system**

```javascript
const fs = require("node:fs");

const content = fs.readFileSync(
    "example.txt",
    "utf8"
);

console.log(content);
```

**Path utilities**

```javascript
const path = require("node:path");

const filePath = path.join(
    __dirname,
    "files",
    "example.txt"
);

console.log(filePath);
```

**Operating system information**

```javascript
const os = require("node:os");

console.log(os.platform());
console.log(os.arch());
console.log(os.totalmem());
```

Using the `node:` prefix explicitly identifies Node.js built-in modules.

# 🗁 Third-Party Packages

CommonJS can import installed npm packages.

For example, install Express:

```bash
npm install express
```

Import it:

```javascript
const express = require("express");

const app = express();

app.get("/", (req, res) => {
    res.send("Hello from Express!");
});

app.listen(3000, () => {
    console.log("Server running on port 3000");
});
```

Whether `require()` works for a particular package depends on that package's module format and its interoperability support.

# 🗁 CommonJS and package.json

Node.js uses the `package.json` configuration to determine how `.js` files should be interpreted.

A package can declare:

```json
{
    "name": "commonjs-project",
    "version": "1.0.0",
    "type": "commonjs"
}
```

With `"type": "commonjs"`, `.js` files in that package scope are interpreted as CommonJS.

Important file extensions:

| Extension | Module Format |
| --- | --- |
| `.js` | Depends on package configuration and Node.js module detection rules |
| `.cjs` | Always CommonJS |
| `.mjs` | Always ECMAScript Modules |

Using `.cjs` explicitly identifies a CommonJS file, regardless of the package's `"type"` setting.

# 🗁 Creating a CommonJS Project

Create a project:

```bash
mkdir commonjs-project

cd commonjs-project

npm init -y
```

Create the following structure:

```text
commonjs-project/
├── package.json
├── app.js
├── math.js
└── greeting.js
```

`package.json`

```json
{
    "name": "commonjs-project",
    "version": "1.0.0",
    "type": "commonjs",
    "main": "app.js"
}
```

`math.js`

```javascript
function add(a, b) {
    return a + b;
}

function multiply(a, b) {
    return a * b;
}

module.exports = {
    add,
    multiply
};
```

`greeting.js`

```javascript
module.exports = function greet(name) {
    return `Hello, ${name}!`;
};
```

`app.js`

```javascript
const math = require("./math");
const greet = require("./greeting");

console.log(greet("Muhammad"));

console.log(math.add(10, 20));

console.log(math.multiply(5, 4));
```

Run the application:

```bash
node app.js
```

**Output**

```text
Hello, Muhammad!
30
20
```

# 🗁 CommonJS vs ECMAScript Modules

ECMAScript Modules (ESM) are the standardized JavaScript module system.

CommonJS and ESM have different syntax, loading behavior, and interoperability characteristics.

| Feature | CommonJS | ECMAScript Modules |
| --- | --- | --- |
| Import syntax | `require()` | `import` |
| Export syntax | `module.exports` | `export` |
| Default export | Export the value directly | `export default` |
| Loading model | Synchronous `require()` | Static linking with asynchronous module loading |
| Dynamic loading | `require()` or `import()` | `import()` |
| Browser support | Not natively supported by browsers | Natively supported |
| Node.js extension | `.cjs` | `.mjs` |
| Package configuration | `"type": "commonjs"` | `"type": "module"` |
| Scope | Module wrapper | Module scope |

**CommonJS**

```javascript
// math.js

function add(a, b) {
    return a + b;
}

module.exports = { add };
```

```javascript
const { add } = require("./math");

console.log(add(10, 20));
```

**ES Modules**

```javascript
// math.js

export function add(a, b) {
    return a + b;
}
```

```javascript
import { add } from "./math.js";

console.log(add(10, 20));
```

# 🗁 CommonJS and Dynamic Import

CommonJS supports dynamic `import()`.

This is especially useful when CommonJS code needs to load an ECMAScript module.

```javascript
// app.cjs

async function loadModule() {
    const module = await import("./math.mjs");

    console.log(module.add(10, 20));
}

loadModule();
```

`math.mjs`

```javascript
export function add(a, b) {
    return a + b;
}
```

Dynamic import returns a Promise and provides access to the imported module's namespace object.

# 🗁 CommonJS Interoperability with ES Modules

Node.js provides interoperability between CommonJS and ECMAScript Modules, but the behavior depends on module format and the direction of the import.

A CommonJS module can generally be loaded from an ES module using a default import.

`legacy.cjs`

```javascript
module.exports = {
    name: "Legacy Module"
};
```

`app.mjs`

```javascript
import legacy from "./legacy.cjs";

console.log(legacy.name);
```

Importing CommonJS named exports from ESM may also work when Node.js can statically analyze the CommonJS exports, but this behavior has limitations.

For reliable interoperability, use the default import when consuming a CommonJS module.

# 🗁 Common Mistakes

**Using exports to replace the exported value**

Incorrect:

```javascript
exports = function greet() {
    return "Hello!";
};
```

Correct:

```javascript
module.exports = function greet() {
    return "Hello!";
};
```

**Mixing CommonJS and ESM syntax without understanding the module format**

Incorrect in a CommonJS file:

```javascript
import { add } from "./math.js";
```

Use:

```javascript
const { add } = require("./math.js");
```

Or configure the project to use ESM and use the appropriate module syntax.

**Forgetting the relative path prefix**

Incorrect:

```javascript
const math = require("math.js");
```

Correct for a local file:

```javascript
const math = require("./math.js");
```

Without a relative path prefix, Node.js treats the specifier as a package or built-in module request.

**Assuming require() can load every ES module**

`require()` is primarily designed for CommonJS modules. Support for requiring certain synchronous ES modules exists in newer Node.js versions, but it has restrictions.

Use dynamic `import()` when loading ESM from CommonJS for broader compatibility.

**Assuming every npm package supports require()**

Some packages are ESM-only and do not provide a CommonJS entry point.

Check the package's documentation and module exports before choosing an import mechanism.

# 🗁 Best Practices

* Use one consistent module system within a project whenever practical.
* Prefer ECMAScript Modules for new JavaScript applications when the ecosystem supports them.
* Understand `module.exports` before using the `exports` shorthand.
* Use `exports.property` only when adding properties to the existing export object.
* Use `module.exports` when replacing the exported value.
* Use `node:` prefixes for built-in Node.js modules.
* Use descriptive module filenames and clear module responsibilities.
* Avoid unnecessary modifications to `require.cache`.
* Use dynamic `import()` when CommonJS code needs to load an ES module.
* Check third-party package compatibility before choosing `require()` or `import`.
* Use `.cjs` and `.mjs` when explicit module-format identification is useful.

# 🗁 Quick Reference

| Operation | Syntax |
| --- | --- |
| Import local module | `require("./module")` |
| Import built-in module | `require("node:fs")` |
| Import npm package | `require("express")` |
| Export one value | `module.exports = value` |
| Export multiple values | `module.exports = { add, subtract }` |
| Add an exported property | `exports.add = add` |
| Import with destructuring | `const { add } = require("./math")` |
| Resolve module path | `require.resolve("./module")` |
| Access current filename | `__filename` |
| Access current directory | `__dirname` |
| Inspect module cache | `require.cache` |
| Clear a cached module | `delete require.cache[require.resolve("./module")]` |
| Dynamically import ESM | `await import("./module.mjs")` |

# 🗁 Practice Exercises

* Create a CommonJS module that exports a greeting function.
* Export multiple mathematical functions from one module.
* Create a class and export it using `module.exports`.
* Export an application configuration object.
* Import a module using `require()`.
* Import selected properties using destructuring.
* Compare `exports` and `module.exports` with practical examples.
* Demonstrate why reassigning `exports` does not replace the exported value.
* Inspect `__filename` and `__dirname`.
* Use `require.resolve()` to inspect a module's resolved path.
* Experiment with CommonJS module caching.
* Import Node.js built-in modules using the `node:` prefix.
* Create a CommonJS application using Express.
* Load an ES module dynamically from a CommonJS file.
* Build a small project containing multiple CommonJS modules.

# 🗁 Key Takeaways

* CommonJS is a JavaScript module system widely used in Node.js.
* `require()` loads CommonJS modules.
* `module.exports` defines the value exposed by a module.
* `exports` is initially a reference to `module.exports`.
* Reassigning `exports` does not replace the module's exported value.
* CommonJS modules have their own scope.
* Node.js caches successfully loaded CommonJS modules.
* `__filename` and `__dirname` provide module file and directory paths.
* CommonJS uses synchronous `require()` for module loading.
* Dynamic `import()` allows CommonJS code to load ECMAScript Modules.
* Understanding CommonJS is important when working with existing Node.js applications and packages.
