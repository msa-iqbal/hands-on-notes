# TypeScript Module

The `module` compiler option determines how TypeScript handles JavaScript modules when compiling TypeScript files.

It controls the module system used in the generated JavaScript, including how `import` and `export` statements are transformed.

Module configuration is especially important when working with Node.js, browsers, frontend frameworks, and build tools.

# 🗁 What Are JavaScript Modules?

JavaScript modules allow developers to organize code into separate files and share functionality between them.

A module can export variables, functions, classes, and types that other modules can import.

Consider two files.

**math.ts**

```typescript
export function add(a: number, b: number): number {
  return a + b;
}
```

**index.ts**

```typescript
import { add } from "./math.js";

console.log(add(10, 20));
```

The `export` keyword makes the function available to other modules, while `import` allows another module to use it.

TypeScript supports JavaScript module syntax and can compile it according to the configured module system.

# 🗁 What Is the `module` Option?

The `module` option determines how TypeScript emits module-related JavaScript code.

Example:

```json
{
  "compilerOptions": {
    "module": "CommonJS"
  }
}
```

With this configuration, TypeScript transforms ES module imports and exports into CommonJS operations.

For example:

**TypeScript source**

```typescript
import { add } from "./math";

export const result = add(10, 20);
```

**CommonJS output**

```javascript
"use strict";

Object.defineProperty(exports, "__esModule", { value: true });

const math_1 = require("./math");

exports.result = (0, math_1.add)(10, 20);
```

The generated output uses `require()` and `exports`, which are associated with CommonJS.

# 🗁 Common Module Options

TypeScript supports several module output strategies.

| Module option | Description |
| --- | --- |
| `None` | Does not emit module-related code |
| `CommonJS` | Emits CommonJS modules |
| `AMD` | Emits Asynchronous Module Definition modules |
| `System` | Emits SystemJS modules |
| `UMD` | Emits Universal Module Definition modules |
| `ES2015` | Emits ECMAScript modules |
| `ES2020` | Supports ES2020 module features such as dynamic `import()` |
| `ES2022` | Supports ES2022 module features |
| `ESNext` | Uses the latest module syntax supported by the compiler |
| `Node16` | Uses Node.js-aware module resolution and emission |
| `NodeNext` | Uses modern Node.js module resolution and emission |
| `Preserve` | Preserves module syntax in the emitted JavaScript |

The appropriate option depends on the runtime and build system.

# 🗁 CommonJS

CommonJS is a module system traditionally used by Node.js.

It uses `require()` to import modules and `module.exports` or `exports` to export functionality.

Example configuration:

```json
{
  "compilerOptions": {
    "module": "CommonJS",
    "target": "ES2020"
  }
}
```

TypeScript source:

```typescript
import { readFileSync } from "node:fs";

export function readConfig(path: string): string {
  return readFileSync(path, "utf8");
}
```

CommonJS output uses `require()` for module loading.

CommonJS is useful when maintaining existing Node.js projects that use this module system.

# 🗁 ES Modules

ECMAScript Modules (ESM) are the standardized JavaScript module system.

They use `import` and `export` statements.

Example configuration:

```json
{
  "compilerOptions": {
    "module": "ESNext",
    "target": "ES2022"
  }
}
```

TypeScript source:

```typescript
import { add } from "./math.js";

export function calculate(): number {
  return add(10, 20);
}
```

The emitted JavaScript preserves the ES module syntax:

```javascript
import { add } from "./math.js";

export function calculate() {
  return add(10, 20);
}
```

ES modules are commonly used in modern browsers, frontend applications, and modern Node.js projects.

# 🗁 Node16

The `Node16` module option models Node.js's dual module system.

Node.js supports both:

* CommonJS modules.
* ECMAScript modules.

TypeScript uses Node.js package rules to determine how files should be interpreted and emitted.

Example:

```json
{
  "compilerOptions": {
    "module": "Node16",
    "moduleResolution": "Node16",
    "target": "ES2022",
    "strict": true
  }
}
```

The module format depends on the file extension and the nearest applicable `package.json` configuration.

For example:

* `.mts` files are treated as ES modules.
* `.cts` files are treated as CommonJS modules.
* `.ts` files follow the package's module configuration.

This option is useful when working with Node.js 16-compatible module behavior.

# 🗁 NodeNext

`NodeNext` models Node.js's modern module behavior.

It is designed to track the module resolution and module system rules supported by current Node.js releases.

Example:

```json
{
  "compilerOptions": {
    "module": "NodeNext",
    "moduleResolution": "NodeNext",
    "target": "ES2022",
    "strict": true
  }
}
```

Like `Node16`, it considers file extensions and package configuration when determining module formats.

For example, a package can declare its module type through `package.json`:

```json
{
  "type": "module"
}
```

With this configuration, ordinary `.js` files in that package scope are interpreted as ES modules by Node.js.

Using:

```json
{
  "type": "commonjs"
}
```

declares CommonJS as the default module type for `.js` files in that package scope.

`NodeNext` is generally appropriate when building applications directly for modern Node.js.

# 🗁 Preserve

The `Preserve` option preserves individual import and export statements in the generated JavaScript.

Example:

```json
{
  "compilerOptions": {
    "module": "Preserve",
    "moduleResolution": "Bundler",
    "target": "ES2022"
  }
}
```

This is useful with modern bundlers that handle module processing themselves.

For example, a project may use:

* Vite
* esbuild
* Rollup
* Webpack

The bundler can process the preserved module syntax as part of its build pipeline.

# 🗁 Module and Module Resolution

The `module` and `moduleResolution` options work together, but they have different responsibilities.

| Option | Responsibility |
| --- | --- |
| `module` | Determines how module syntax is emitted |
| `moduleResolution` | Determines how TypeScript locates imported modules |

For example:

```json
{
  "compilerOptions": {
    "module": "NodeNext",
    "moduleResolution": "NodeNext"
  }
}
```

This configuration uses Node.js-aware module resolution and emission behavior.

Another example:

```json
{
  "compilerOptions": {
    "module": "ESNext",
    "moduleResolution": "Bundler"
  }
}
```

This configuration is commonly used with frontend bundlers.

# 🗁 Choosing the Right Module Option

| Project type | Common choice |
| --- | --- |
| Legacy Node.js CommonJS application | `CommonJS` |
| Modern Node.js application | `NodeNext` |
| Node.js project targeting Node 16 module behavior | `Node16` |
| Browser application using a bundler | `ESNext` or `Preserve` |
| Modern frontend application | `ESNext` or `Preserve` |
| Library built with a bundler | Depends on the library's output requirements |

Do not select a module option only because it is newer. Choose one that matches the runtime and build pipeline.

# 🗁 Important Considerations

* `module` does not determine which JavaScript syntax features are supported. That is primarily controlled by `target`.
* `module` and `moduleResolution` should be configured consistently.
* Node.js uses file extensions and `package.json` metadata to determine module behavior.
* CommonJS and ES modules have different loading and interoperability rules.
* Bundlers may process module syntax independently of TypeScript.
* TypeScript does not automatically convert every CommonJS and ES module interoperability issue into runtime-compatible behavior.
* Node.js projects using `NodeNext` should follow Node.js module resolution rules, including appropriate import paths.

# 🗁 Summary

The `module` option controls how TypeScript handles JavaScript modules during compilation.

It is essential for:

* Choosing between CommonJS and ES modules.
* Configuring Node.js applications.
* Integrating with frontend bundlers.
* Controlling module-related JavaScript output.
* Maintaining compatibility with the target runtime.

Always align the module configuration with the environment that will execute or bundle your application.
