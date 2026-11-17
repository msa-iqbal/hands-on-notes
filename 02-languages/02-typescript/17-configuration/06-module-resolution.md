# TypeScript Module Resolution

Module resolution is the process TypeScript uses to locate files and determine which module an import statement refers to.

When you import a module, TypeScript must resolve its path, identify the corresponding source or declaration file, and understand the types exported by that module.

Understanding module resolution is essential when working with Node.js, frontend frameworks, package managers, and modern build tools.

# 🗁 What Is Module Resolution?

Consider the following project:

```text
src/
├── index.ts
└── utils/
    └── math.ts
```

Inside `index.ts`:

```typescript
import { add } from "./utils/math.js";

console.log(add(10, 20));
```

TypeScript must locate the module referenced by `./utils/math.js`.

It examines the import path, the current module resolution strategy, file extensions, package configuration, and available declaration files.

This process is called module resolution.

# 🗁 The `moduleResolution` Option

TypeScript provides the `moduleResolution` compiler option to control how imported modules are located.

Example:

```json
{
  "compilerOptions": {
    "moduleResolution": "NodeNext"
  }
}
```

The selected strategy determines how TypeScript searches for modules and interprets package exports.

# 🗁 Available Module Resolution Strategies

Common module resolution strategies include:

| Strategy | Description |
| --- | --- |
| `Classic` | Legacy TypeScript module resolution strategy |
| `Node10` | Traditional Node.js resolution behavior, previously called `Node` |
| `Node16` | Node.js-aware resolution for modern CommonJS and ESM behavior |
| `NodeNext` | Tracks modern Node.js module resolution behavior |
| `Bundler` | Resolution strategy designed for modern bundlers |

The appropriate strategy depends on the runtime and build system.

# 🗁 Classic Resolution

`Classic` is the original TypeScript module resolution strategy.

It is mainly relevant to older TypeScript projects.

Example:

```json
{
  "compilerOptions": {
    "moduleResolution": "Classic"
  }
}
```

It does not model modern Node.js package resolution rules.

For new applications, prefer a strategy designed for the actual runtime or bundler.

# 🗁 Node10 Resolution

`Node10` models the traditional Node.js CommonJS module resolution algorithm.

Example:

```json
{
  "compilerOptions": {
    "module": "CommonJS",
    "moduleResolution": "Node10"
  }
}
```

Consider this import:

```typescript
import { add } from "./utils/math";
```

TypeScript can search for files such as:

```text
utils/
├── math.ts
├── math.tsx
├── math.d.ts
└── math/
    └── index.ts
```

The exact lookup behavior depends on the imported path and available files.

`Node10` is useful when maintaining older projects that rely on traditional Node.js module resolution.

# 🗁 Node16 Resolution

`Node16` models the module resolution behavior of Node.js 16 and its support for both CommonJS and ECMAScript modules.

Example:

```json
{
  "compilerOptions": {
    "module": "Node16",
    "moduleResolution": "Node16",
    "target": "ES2022"
  }
}
```

It considers:

* The importing file's module format.
* File extensions.
* The nearest applicable `package.json`.
* Package `exports` and `imports` fields.
* Node.js module resolution rules.

For example, an ESM import in a Node.js project commonly uses a JavaScript extension:

```typescript
import { add } from "./math.js";
```

Even when the source file is named `math.ts`, TypeScript can resolve the `.js` specifier to the corresponding TypeScript source file.

This allows the emitted JavaScript import path to remain valid.

# 🗁 NodeNext Resolution

`NodeNext` follows modern Node.js module resolution behavior.

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

It supports modern Node.js package features, including:

* ECMAScript modules.
* CommonJS modules.
* Package `exports`.
* Package `imports`.
* Conditional exports.
* Extension-aware module resolution.

It is generally appropriate for modern Node.js projects.

Unlike a fixed historical module model, `NodeNext` evolves with TypeScript's understanding of modern Node.js behavior.

# 🗁 Bundler Resolution

`Bundler` is designed for projects where a bundler handles module loading and packaging.

Example:

```json
{
  "compilerOptions": {
    "module": "ESNext",
    "moduleResolution": "Bundler",
    "target": "ES2022"
  }
}
```

It supports modern package resolution features while allowing behavior commonly used by bundlers.

For example:

```typescript
import { formatDate } from "./utils";
```

This extensionless import can be accepted under bundler resolution.

Bundler resolution is commonly used with:

* Vite.
* Webpack.
* Rollup.
* esbuild.
* Modern frontend frameworks.

It should not be used as a substitute for Node.js resolution when emitted JavaScript will run directly in Node.js without a bundler.

# 🗁 How TypeScript Resolves a Relative Import

Consider this project:

```text
project/
├── src/
│   ├── index.ts
│   └── utils/
│       ├── math.ts
│       └── string.ts
└── tsconfig.json
```

Inside `index.ts`:

```typescript
import { add } from "./utils/math.js";
import { capitalize } from "./utils/string.js";
```

TypeScript examines the import specifier and searches for a compatible source or declaration file.

Under Node.js-aware resolution, a `.js` specifier can resolve to a `.ts` source file during compilation.

This is an important distinction:

* The import specifier describes the runtime JavaScript path.
* TypeScript resolves that path to the appropriate TypeScript source or declaration file during type checking.

# 🗁 Package-Based Module Resolution

When importing an installed package, TypeScript searches through package metadata and declaration files.

For example:

```typescript
import express from "express";
```

A package may expose its entry points through `package.json`:

```json
{
  "name": "example-package",
  "type": "module",
  "exports": {
    ".": {
      "import": "./dist/index.js",
      "require": "./dist/index.cjs"
    }
  }
}
```

The `exports` field defines which package entry points are publicly accessible.

Conditional exports allow a package to provide different entry points depending on how it is loaded.

TypeScript uses the active module resolution strategy to interpret these package definitions and locate the relevant type information.

# 🗁 The `exports` Field

The `exports` field controls which package paths consumers can import.

Example:

```json
{
  "name": "my-library",
  "exports": {
    ".": "./dist/index.js",
    "./utils": "./dist/utils.js"
  }
}
```

Consumers can import:

```typescript
import { createApp } from "my-library";
import { formatDate } from "my-library/utils";
```

An internal package file that is not exposed through `exports` may not be importable through a package subpath.

This provides a controlled public API for libraries.

# 🗁 The `imports` Field

The `imports` field defines private package import mappings.

These mappings use a `#` prefix.

Example:

```json
{
  "type": "module",
  "imports": {
    "#utils/*": "./dist/utils/*.js"
  }
}
```

A module inside the package can use:

```typescript
import { formatDate } from "#utils/date";
```

The mapping is intended for internal package imports rather than public package entry points.

Node.js-aware module resolution understands this mechanism.

# 🗁 Resolving Declaration Files

TypeScript can use declaration files to understand the types exported by JavaScript modules.

For example:

```text
src/
├── index.ts
└── math.ts
```

A declaration file might describe a JavaScript module:

```typescript
export declare function add(a: number, b: number): number;
```

Declaration files commonly use the `.d.ts` extension.

When resolving a package, TypeScript may inspect metadata such as:

* `types`
* `typings`
* `exports`
* `typesVersions`

These fields help TypeScript locate the correct type definitions.

# 🗁 Debugging Module Resolution

When TypeScript cannot find an imported module, the `traceResolution` option can help identify the reason.

Configure it in `tsconfig.json`:

```json
{
  "compilerOptions": {
    "traceResolution": true
  }
}
```

Or run:

```bash
tsc --traceResolution
```

The compiler prints detailed information about its module lookup process.

This helps diagnose:

* Incorrect import paths.
* Missing dependencies.
* Incorrect file extensions.
* Package export restrictions.
* Missing declaration files.
* Incompatible module resolution settings.

# 🗁 Choosing the Right Resolution Strategy

| Project type | Recommended starting point |
| --- | --- |
| Legacy TypeScript project | `Node10` or existing configuration |
| Modern Node.js application | `NodeNext` |
| Node.js project targeting Node16 behavior | `Node16` |
| Vite frontend application | `Bundler` |
| Webpack frontend application | `Bundler` |
| Rollup-based application | `Bundler` |
| Browser application using a modern bundler | `Bundler` |

Always verify the configuration against the actual runtime and build pipeline.

# 🗁 Important Considerations

* `moduleResolution` determines how TypeScript locates imported modules.
* `module` determines the module output behavior.
* Node.js-aware resolution follows Node.js package and extension rules.
* Bundler resolution is designed for projects where a bundler handles module processing.
* The `exports` field controls public package entry points.
* The `imports` field defines private package import mappings.
* Declaration files provide type information without requiring TypeScript source code.
* Use `traceResolution` to investigate difficult module resolution problems.

# 🗁 Summary

Module resolution determines how TypeScript locates files referenced by import statements.

Understanding it helps you:

* Configure Node.js and frontend projects correctly.
* Resolve relative and package imports.
* Work with ESM and CommonJS.
* Understand package `exports` and `imports`.
* Diagnose missing module and declaration errors.

Choose a resolution strategy that matches the environment responsible for loading your application.
