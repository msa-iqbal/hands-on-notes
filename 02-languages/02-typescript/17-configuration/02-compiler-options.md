# Compiler Options

TypeScript compiler options control how the TypeScript compiler analyzes source code, performs type checking, and generates JavaScript output.

These options are configured inside the `compilerOptions` property of `tsconfig.json`.

Compiler options determine important aspects of a project, including:

* JavaScript language compatibility.
* Module generation.
* Type-checking strictness.
* Source and output directories.
* JavaScript generation.
* Declaration file generation.
* Source map generation.
* Module resolution.
* Runtime compatibility.

# 🗁 Where Compiler Options Are Defined

Compiler options are configured inside `tsconfig.json`.

```json
{
  "compilerOptions": {
    "target": "ES2022",
    "module": "NodeNext",
    "strict": true,
    "outDir": "./dist"
  }
}
```

The TypeScript compiler reads these options when compiling the project.

# 🗁 Target

The `target` option determines the JavaScript language version TypeScript generates.

```json
{
  "compilerOptions": {
    "target": "ES2022"
  }
}
```

Available targets include:

* `ES3`
* `ES5`
* `ES2015`
* `ES2016`
* `ES2017`
* `ES2018`
* `ES2019`
* `ES2020`
* `ES2021`
* `ES2022`
* `ES2023`
* `ES2024`
* `ESNext`

Example:

```typescript
const greet = (name: string): string => {
  return `Hello, ${name}`;
};
```

With a modern target, TypeScript can preserve modern JavaScript syntax.

With an older target, some syntax may be transformed into older JavaScript equivalents.

The `target` option does not automatically provide polyfills for unsupported runtime APIs.

# 🗁 Module

The `module` option controls the module format TypeScript uses when generating JavaScript.

```json
{
  "compilerOptions": {
    "module": "NodeNext"
  }
}
```

Common module options include:

* `CommonJS`
* `ES2015`
* `ES2020`
* `ES2022`
* `ESNext`
* `Node16`
* `NodeNext`
* `Preserve`

Example TypeScript:

```typescript
import { add } from "./math.js";

export const result = add(10, 20);
```

The emitted JavaScript module format depends on the configured module option and, for Node.js-oriented modes, the project's module context.

# 🗁 Strict

The `strict` option enables a collection of strict type-checking options.

```json
{
  "compilerOptions": {
    "strict": true
  }
}
```

It enables checks such as:

* `noImplicitAny`
* `strictNullChecks`
* `strictFunctionTypes`
* `strictBindCallApply`
* `strictPropertyInitialization`
* `noImplicitThis`
* `useUnknownInCatchVariables`
* `alwaysStrict`

These checks help identify potential problems during development.

For example:

```typescript
function greet(name: string) {
  return `Hello, ${name}`;
}

greet("John");
```

The parameter type is explicit, so TypeScript can validate function calls.

With strict null checking enabled:

```typescript
let username: string = "John";

// TypeScript error
username = null;
```

The `strict` option is strongly recommended for new TypeScript projects.

# 🗁 RootDir

The `rootDir` option specifies the root directory of the source files whose directory structure TypeScript preserves in the output.

```json
{
  "compilerOptions": {
    "rootDir": "./src"
  }
}
```

Example project:

```text
my-project/
├── src/
│   ├── index.ts
│   └── utils/
│       └── helper.ts
├── dist/
└── tsconfig.json
```

When compiling with `outDir: "./dist"`, the source directory structure is preserved relative to `rootDir`.

```text
dist/
├── index.js
└── utils/
    └── helper.js
```

`rootDir` does not determine which files belong to the project. File selection is controlled by `files`, `include`, imports, and other project inclusion mechanisms.

# 🗁 OutDir

The `outDir` option specifies where generated JavaScript and related output files should be placed.

```json
{
  "compilerOptions": {
    "outDir": "./dist"
  }
}
```

Example:

```text
my-project/
├── src/
│   └── index.ts
├── dist/
│   └── index.js
└── tsconfig.json
```

Without `outDir`, generated files are generally written alongside their source files.

Using a separate output directory keeps source code and compiled files organized.

# 🗁 NoEmit

The `noEmit` option prevents TypeScript from generating output files.

```json
{
  "compilerOptions": {
    "noEmit": true
  }
}
```

TypeScript still performs type checking.

This is useful when another tool handles compilation or bundling.

For example:

* Vite
* esbuild
* SWC
* Babel
* Other JavaScript build tools

Example:

```bash
tsc --noEmit
```

This checks the project without generating JavaScript files.

# 🗁 Declaration

The `declaration` option generates `.d.ts` declaration files alongside the compiled output.

```json
{
  "compilerOptions": {
    "declaration": true
  }
}
```

Example source:

```typescript
export function add(a: number, b: number): number {
  return a + b;
}
```

Generated declaration:

```typescript
export declare function add(a: number, b: number): number;
```

Declaration files describe the types and public API of a module without containing its implementation.

They are especially useful when publishing TypeScript libraries.

# 🗁 DeclarationDir

The `declarationDir` option specifies a separate directory for generated declaration files.

```json
{
  "compilerOptions": {
    "declaration": true,
    "declarationDir": "./types"
  }
}
```

Example output:

```text
my-project/
├── src/
├── dist/
│   └── index.js
├── types/
│   └── index.d.ts
└── tsconfig.json
```

This allows JavaScript output and type declarations to be stored separately.

# 🗁 SourceMap

The `sourceMap` option generates `.map` files that map compiled JavaScript back to the original TypeScript source.

```json
{
  "compilerOptions": {
    "sourceMap": true
  }
}
```

Example output:

```text
dist/
├── index.js
└── index.js.map
```

Source maps are useful for debugging compiled applications.

They allow browser developer tools and compatible debuggers to display the original TypeScript source.

# 🗁 RemoveComments

The `removeComments` option removes comments from generated JavaScript files.

```json
{
  "compilerOptions": {
    "removeComments": true
  }
}
```

Example source:

```typescript
// Calculate the total
function add(a: number, b: number): number {
  return a + b;
}
```

Generated JavaScript:

```javascript
function add(a, b) {
  return a + b;
}
```

This option affects emitted comments, not comments in the original source files.

# 🗁 AllowJs

The `allowJs` option allows JavaScript files to be included in a TypeScript project.

```json
{
  "compilerOptions": {
    "allowJs": true
  }
}
```

This is useful when gradually migrating a JavaScript project to TypeScript.

Example project:

```text
src/
├── index.ts
├── app.js
└── utils.js
```

With `allowJs` enabled, TypeScript can include both `.ts` and `.js` files.

# 🗁 CheckJs

The `checkJs` option enables type checking for JavaScript files included in the project.

```json
{
  "compilerOptions": {
    "allowJs": true,
    "checkJs": true
  }
}
```

Example JavaScript:

```javascript
// @ts-check

function add(a, b) {
  return a + b;
}

add(10, "20");
```

TypeScript can report problems in JavaScript files when it can determine incompatible types.

JSDoc annotations can provide additional type information.

# 🗁 NoEmitOnError

The `noEmitOnError` option prevents TypeScript from generating output when compilation reports errors.

```json
{
  "compilerOptions": {
    "noEmitOnError": true
  }
}
```

Without this option, TypeScript can emit JavaScript even when type-checking errors exist.

This option is useful when a build should only produce output after successful type checking.

# 🗁 SkipLibCheck

The `skipLibCheck` option skips type checking of declaration files (`.d.ts`).

```json
{
  "compilerOptions": {
    "skipLibCheck": true
  }
}
```

This can reduce compilation time in projects that depend on many external type declarations.

It does not disable type checking of the application's own TypeScript source files.

Use it deliberately because it can hide type inconsistencies inside declaration files.

# 🗁 EsModuleInterop

The `esModuleInterop` option improves interoperability between CommonJS and ES module imports.

```json
{
  "compilerOptions": {
    "esModuleInterop": true
  }
}
```

It enables compatibility behavior for importing CommonJS modules using ES-style default imports.

For example:

```typescript
import express from "express";
```

The exact compatibility behavior depends on the module system and runtime.

# 🗁 ForceConsistentCasingInFileNames

The `forceConsistentCasingInFileNames` option ensures that imports use consistent file-name casing.

```json
{
  "compilerOptions": {
    "forceConsistentCasingInFileNames": true
  }
}
```

Consider this file:

```text
src/
└── UserService.ts
```

An import using different casing may work on some case-insensitive file systems but fail on case-sensitive systems.

```typescript
import { UserService } from "./userservice";
```

Enabling this option helps detect inconsistent casing during development.

# 🗁 Incremental

The `incremental` option enables incremental compilation.

```json
{
  "compilerOptions": {
    "incremental": true
  }
}
```

TypeScript stores information about the previous compilation in a `.tsbuildinfo` file.

Future compilations can reuse this information to avoid unnecessary work.

This is particularly useful in large projects.

# 🗁 NoUnusedLocals and NoUnusedParameters

These options identify unused variables and function parameters.

```json
{
  "compilerOptions": {
    "noUnusedLocals": true,
    "noUnusedParameters": true
  }
}
```

Example:

```typescript
function greet(name: string, age: number): string {
  return `Hello, ${name}`;
}
```

If `age` is unused, TypeScript reports a diagnostic when `noUnusedParameters` is enabled.

These options help keep code clean and reduce unnecessary declarations.

# 🗁 NoFallthroughCasesInSwitch

The `noFallthroughCasesInSwitch` option reports switch cases that unintentionally continue into the next case.

```json
{
  "compilerOptions": {
    "noFallthroughCasesInSwitch": true
  }
}
```

Example:

```typescript
const status = 200;

switch (status) {
  case 200:
    console.log("Success");

  case 404:
    console.log("Not found");
    break;
}
```

The missing `break` after the first case causes fallthrough.

TypeScript can report this problem when the option is enabled.

# 🗁 A Practical Configuration Example

A general Node.js TypeScript application might use the following configuration:

```json
{
  "compilerOptions": {
    "target": "ES2022",
    "module": "NodeNext",
    "moduleResolution": "NodeNext",
    "strict": true,
    "rootDir": "./src",
    "outDir": "./dist",
    "sourceMap": true,
    "declaration": true,
    "noEmitOnError": true,
    "forceConsistentCasingInFileNames": true,
    "skipLibCheck": true
  },
  "include": ["src/**/*"],
  "exclude": ["node_modules", "dist"]
}
```

This configuration provides:

* Modern JavaScript output.
* Node.js-oriented module handling.
* Strict type checking.
* Separate source and output directories.
* Source maps for debugging.
* Declaration files for type distribution.
* Consistent import casing.
* No generated output when compilation fails.

The appropriate module settings depend on the runtime and package configuration. `NodeNext` is intended for projects following Node.js ESM and CommonJS module rules.

# 🗁 Best Practices

* Enable `strict` for stronger type safety.
* Choose a `target` compatible with the supported runtime.
* Select the correct module system for the environment.
* Keep generated files in a separate output directory.
* Use `noEmit` when another tool handles compilation.
* Enable `declaration` when building reusable TypeScript libraries.
* Use source maps to simplify debugging.
* Consider incremental compilation for large projects.
* Enable consistent file-name casing.
* Avoid enabling options without understanding their impact.
* Review compiler options when upgrading TypeScript.

# 🗁 Summary

Compiler options control TypeScript's type checking and output generation.

Important options include:

* `target` — JavaScript language version.
* `module` — Module format and behavior.
* `strict` — Strict type-checking options.
* `rootDir` — Expected source directory structure.
* `outDir` — Generated output directory.
* `noEmit` — Prevents output generation.
* `declaration` — Generates `.d.ts` files.
* `sourceMap` — Generates source maps.
* `allowJs` — Includes JavaScript files.
* `checkJs` — Type-checks JavaScript files.
* `skipLibCheck` — Skips checking declaration files.
* `incremental` — Enables incremental compilation.
