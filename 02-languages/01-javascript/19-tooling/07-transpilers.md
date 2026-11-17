# JavaScript Transpilers

A JavaScript transpiler is a development tool that transforms source code written in one version or form of JavaScript into another version or form of JavaScript.

Transpilers help developers use modern language features while maintaining compatibility with environments that may not support those features directly.

They are also used to transform languages such as TypeScript and JSX into JavaScript.

# 🗁 What Is a Transpiler?

A transpiler is a type of source-to-source compiler.

It converts code from one programming language or language version into another representation at approximately the same abstraction level.

For example, a transpiler can convert modern JavaScript into older JavaScript syntax.

Modern JavaScript:

```javascript
const greet = (name) => {
  return `Hello, ${name}!`;
};
```

Transpiled JavaScript:

```javascript
"use strict";

var greet = function (name) {
  return "Hello, " + name + "!";
};
```

The transformed code performs the same basic operation but uses syntax supported by older JavaScript environments.

# 🗁 Why Do We Need Transpilers?

JavaScript evolves continuously through ECMAScript specifications.

Modern JavaScript introduces features such as:

* Arrow functions
* Classes
* Template literals
* Destructuring
* Optional chaining
* Nullish coalescing
* Async functions
* Private class fields
* New syntax and language constructs

However, different JavaScript environments support different language features.

A transpiler helps bridge these compatibility differences.

Common reasons to use transpilers include:

* Supporting older browsers.
* Supporting older JavaScript runtimes.
* Using modern JavaScript syntax.
* Transforming TypeScript into JavaScript.
* Transforming JSX into JavaScript.
* Applying custom language transformations.
* Integrating experimental syntax into development workflows.

# 🗁 How Transpilation Works

A typical transpilation process involves several stages.

<AsyncImage query="JavaScript Babel transpiler compilation pipeline source code parser AST transformation generator diagram" aspectRatio="16:9" width="100%"/>

**Source code**

The developer writes modern JavaScript or another supported language.

```javascript
const square = (number) => number * number;
```

**Parsing**

The transpiler analyzes the source code and converts it into an Abstract Syntax Tree (AST).

An AST represents the structure of the source program.

**Transformation**

The transpiler applies transformation rules to the AST.

For example, an arrow function may be converted into a traditional function expression.

**Code generation**

The transpiler generates JavaScript from the transformed AST.

```javascript
var square = function (number) {
  return number * number;
};
```

**Output**

The generated JavaScript can then be executed by the target JavaScript environment or passed to another build tool.

# 🗁 Transpiler vs Compiler vs Interpreter

These terms describe different approaches to processing and executing source code.

| Transpiler | Compiler | Interpreter |
| --- | --- | --- |
| Transforms source code into another source representation | Translates source code into another executable representation | Executes program instructions through an execution environment |
| Usually preserves a similar abstraction level | May produce machine code, bytecode, or another target format | Commonly executes code without requiring a separate native executable |
| Example: Babel | Example: GCC | Example: Python interpreter |

A transpiler is technically a specialized form of compiler because it translates source code into another source representation.

Modern JavaScript engines also use compilation techniques internally, including Just-In-Time (JIT) compilation.

# 🗁 Popular JavaScript Transpilers

Several tools are used to transform JavaScript and JavaScript-related languages.

<AsyncImageGroup query={["Babel JavaScript compiler logo","TypeScript programming language logo","SWC JavaScript compiler logo","Sucrase JavaScript compiler logo"]} aspectRatio="5:3" layout="carousel"/>

**Babel**

<text color="secondary" size="sm">JavaScript compiler and transformation toolchain</text>

<text color="default"> <Entity value="Babel" category="software" disambig="JavaScript compiler and source transformation tool"/> </text> is a widely used JavaScript compiler that transforms modern JavaScript syntax into code compatible with specified target environments.

Key features:

* Modern JavaScript syntax transformation.
* JSX transformation.
* Plugin-based architecture.
* Preset-based configuration.
* Polyfill integration through related tooling.
* Custom AST transformations.

Common use cases:

* React applications.
* Legacy browser support.
* JavaScript library development.
* Custom source transformations.

Documentation: <https://babeljs.io/>

**TypeScript Compiler**

<text color="secondary" size="sm">Compiler for TypeScript</text>

<text color="default"><Entity value="TypeScript" category="software" disambig="Typed superset of JavaScript"/> </text> provides a compiler named `tsc` that checks TypeScript types and emits JavaScript.

TypeScript:

```typescript
interface User {
  id: number;
  name: string;
}

const user: User = {
  id: 1,
  name: "Alice"
};
```

Generated JavaScript:

```javascript
const user = {
  id: 1,
  name: "Alice"
};
```

TypeScript-specific type annotations and interfaces are removed during compilation.

Key features:

* Type checking.
* JavaScript generation.
* Declaration file generation.
* Module transformation.
* Configurable compilation targets.

Documentation: <https://www.typescriptlang.org/>

**SWC**

<text color="secondary" size="sm">High-performance JavaScript and TypeScript compiler</text>

<text color="default"><Entity value="SWC" category="software" disambig="Rust-based JavaScript and TypeScript compiler"/> </text> is a fast compiler written in Rust.

Key features:

* JavaScript transformation.
* TypeScript transformation.
* JSX and TSX transformation.
* Minification.
* Configurable target environments.
* Fast compilation.

Common use cases:

* Modern frontend frameworks.
* Large JavaScript projects.
* Build pipelines requiring fast transformations.

Documentation: <https://swc.rs/>

**Sucrase**

<text color="secondary" size="sm">Fast development-oriented JavaScript transformer</text>

<text color="default"><Entity value="Sucrase" category="software" disambig="JavaScript and TypeScript syntax transformer"/> </text> transforms TypeScript, JSX, and selected modern JavaScript syntax with a focus on development speed.

It is designed primarily for development workflows rather than comprehensive production compilation.

Documentation: <https://sucrase.com/>

# 🗁 Babel

Babel is one of the most important tools in the JavaScript ecosystem.

It allows developers to write modern JavaScript while transforming syntax for older environments.

**Installing Babel**

Install Babel's core package:

```bash
npm install --save-dev @babel/core
```

Install the command-line interface:

```bash
npm install --save-dev @babel/cli
```

Install a preset for modern JavaScript:

```bash
npm install --save-dev @babel/preset-env
```

**Basic Babel configuration**

Create a `babel.config.json` file:

```json
{
  "presets": [
    "@babel/preset-env"
  ]
}
```

**Transpile JavaScript**

Source:

```javascript
// src/index.js

const greet = (name) => {
  return `Hello, ${name}!`;
};

console.log(greet("Alice"));
```

Run:

```bash
npx babel src/index.js --out-file dist/index.js
```

Generated output:

```javascript
"use strict";

var greet = function (name) {
  return "Hello, ".concat(name, "!");
};

console.log(greet("Alice"));
```

The exact output depends on the Babel version and configuration.

# 🗁 Babel Presets

A Babel preset is a collection of plugins that configure multiple transformations.

Presets simplify configuration by grouping related transformation rules.

Common presets include:

**@babel/preset-env**

Transforms modern JavaScript syntax according to target environments.

```bash
npm install --save-dev @babel/preset-env
```

Configuration:

```json
{
  "presets": [
    "@babel/preset-env"
  ]
}
```

You can specify target environments:

```json
{
  "presets": [
    [
      "@babel/preset-env",
      {
        "targets": {
          "chrome": "100",
          "firefox": "100"
        }
      }
    ]
  ]
}
```

Babel uses the target configuration to determine which transformations are necessary.

**@babel/preset-react**

Transforms React-related JSX syntax.

```bash
npm install --save-dev @babel/preset-react
```

Configuration:

```json
{
  "presets": [
    "@babel/preset-react"
  ]
}
```

JSX:

```jsx
const element = <h1>Hello, world!</h1>;
```

Transformed JavaScript using the automatic runtime:

```javascript
import { jsx as _jsx } from "react/jsx-runtime";

const element = _jsx("h1", {
  children: "Hello, world!"
});
```

The exact transformation depends on the configured JSX runtime.

**@babel/preset-typescript**

Removes TypeScript-specific syntax during transformation.

```bash
npm install --save-dev @babel/preset-typescript
```

Configuration:

```json
{
  "presets": [
    "@babel/preset-typescript"
  ]
}
```

Important: Babel's TypeScript preset transforms TypeScript syntax but does not perform TypeScript type checking. Use `tsc --noEmit` or another type-checking workflow when type checking is required.

# 🗁 Babel Plugins

Babel plugins perform individual transformations.

For example, a plugin can transform arrow functions into regular functions.

Install:

```bash
npm install --save-dev @babel/plugin-transform-arrow-functions
```

Configuration:

```json
{
  "plugins": [
    "@babel/plugin-transform-arrow-functions"
  ]
}
```

Babel presets are collections of plugins, while plugins provide individual transformation capabilities.

When both are configured, Babel applies transformations according to its plugin and preset ordering rules.

# 🗁 Polyfills

A polyfill provides an implementation of a JavaScript feature that is missing from an environment.

Transpilation and polyfilling solve different compatibility problems.

A transpiler transforms syntax.

A polyfill provides runtime functionality.

Consider:

```javascript
const result = users.flatMap((user) => user.roles);
```

A transpiler may transform the arrow function syntax, but it cannot automatically guarantee that the target environment supports `Array.prototype.flatMap()`.

A polyfill can provide the missing method.

Common examples include:

* `Promise`
* `Array.prototype.flatMap()`
* `Array.prototype.includes()`
* `Object.fromEntries()`
* `URL`

**Using core-js with Babel**

Install:

```bash
npm install core-js
```

Configure Babel:

```json
{
  "presets": [
    [
      "@babel/preset-env",
      {
        "useBuiltIns": "usage",
        "corejs": "3"
      }
    ]
  ]
}
```

With this configuration, Babel can inject required `core-js` imports based on detected usage and target environments.

Polyfill availability depends on the configured targets and the feature being used.

# 🗁 Transpilation vs Polyfilling

| Transpilation | Polyfilling |
| --- | --- |
| Transforms syntax | Provides missing runtime functionality |
| Operates on source code | Supplies runtime implementations |
| Can transform arrow functions | Can provide missing built-in methods |
| Example: Babel | Example: core-js |

For complete browser compatibility, a project may need both.

# 🗁 TypeScript Transpilation

TypeScript extends JavaScript with a static type system.

Browsers and Node.js do not execute TypeScript type annotations directly.

TypeScript must be transformed into JavaScript before execution.

Example:

```typescript
function add(a: number, b: number): number {
  return a + b;
}

console.log(add(10, 20));
```

Compile:

```bash
npx tsc index.ts
```

Generated JavaScript:

```javascript
function add(a, b) {
  return a + b;
}

console.log(add(10, 20));
```

TypeScript compilation also performs type checking unless disabled through compiler options.

For example:

```bash
npx tsc --noEmit
```

This checks the project without generating JavaScript output.

# 🗁 JSX Transformation

JSX is a syntax extension commonly used with React.

It allows developers to write HTML-like markup inside JavaScript.

Example:

```jsx
function App() {
  return <h1>Hello, React!</h1>;
}
```

Browsers cannot execute JSX directly.

A transpiler transforms JSX into JavaScript function calls.

With the automatic React runtime, the result may resemble:

```javascript
import { jsx as _jsx } from "react/jsx-runtime";

function App() {
  return _jsx("h1", {
    children: "Hello, React!"
  });
}
```

JSX transformation is commonly handled by Babel, SWC, or framework-specific build tooling.

# 🗁 Transpilation Targets

A transpilation target describes the environment for which generated JavaScript should be compatible.

For example:

```json
{
  "presets": [
    [
      "@babel/preset-env",
      {
        "targets": {
          "chrome": "100"
        }
      }
    ]
  ]
}
```

This configuration targets Chrome 100.

Targets influence which syntax transformations are necessary.

Other target environments include:

* Chrome
* Firefox
* Safari
* Edge
* Node.js
* Internet Explorer in legacy projects

Choosing targets carefully avoids unnecessary transformations and polyfills.

# 🗁 Transpilation in Modern Build Tools

Modern frontend tools often integrate transpilation directly into their build pipelines.

A typical workflow:

```text
Source Code
    |
    v
TypeScript / JSX / Modern JavaScript
    |
    v
Transpiler
    |
    v
JavaScript Transformation
    |
    v
Bundler
    |
    v
Optimization
    |
    v
Production Assets
```

For example:

* Webpack can integrate Babel through `babel-loader`.
* Vite supports TypeScript and JSX transformations through its build pipeline and plugins.
* Next.js uses integrated compilation tooling.
* SWC can handle JavaScript and TypeScript transformations.

The exact build pipeline depends on the tool and project configuration.

# 🗁 Transpilation vs Bundling

Transpilation and bundling are related but separate operations.

| Transpilation | Bundling |
| --- | --- |
| Transforms source syntax | Combines and organizes modules |
| Produces transformed code | Produces one or more bundles |
| May process individual files | Analyzes dependency graphs |
| Examples: Babel, SWC | Examples: Webpack, Rollup |

A project may use both tools.

For example, Babel transforms modern JavaScript, and Webpack bundles the transformed modules.

# 🗁 Best Practices

* Choose transpilation targets based on actual supported environments.
* Avoid unnecessary syntax transformations.
* Use Babel presets for common transformation requirements.
* Keep Babel plugins and presets updated.
* Understand the difference between transpilation and polyfilling.
* Use TypeScript's compiler or another type checker when type checking is required.
* Do not assume JSX or TypeScript can execute directly in browsers.
* Integrate transpilation into the project's build pipeline.
* Avoid transforming code that the target environment already supports unless necessary.
* Test generated output in the environments the application supports.
* Use source maps to simplify debugging transformed code.

# 🗁 Key Takeaways

* A transpiler converts source code into another source representation.
* JavaScript transpilers help developers use modern syntax across different environments.
* Babel is a widely used JavaScript transformation tool.
* TypeScript's compiler transforms TypeScript into JavaScript and can perform type checking.
* SWC provides high-performance JavaScript and TypeScript transformation.
* Presets group Babel plugins into reusable configurations.
* Polyfills provide missing runtime functionality; transpilers transform syntax.
* JSX requires transformation before standard JavaScript execution.
* Transpilers and bundlers often work together in modern frontend build pipelines.
