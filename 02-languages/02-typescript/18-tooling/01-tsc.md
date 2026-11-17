# TypeScript Compiler (tsc)

The TypeScript Compiler (`tsc`) is the official command-line compiler provided by TypeScript.

It converts TypeScript code into JavaScript, performs static type checking, generates declaration files, and processes TypeScript project configurations.

The `tsc` command is one of the most important tools in the TypeScript development workflow.

# 🗁 What Is `tsc`?

`tsc` stands for TypeScript Compiler.

It is installed through the TypeScript package and provides a command-line interface for compiling and checking TypeScript files.

For example:

```typescript
const username: string = "Alice";

console.log(`Hello, ${username}!`);
```

Compile the file:

```bash
tsc index.ts
```

TypeScript generates:

```javascript
var username = "Alice";
console.log("Hello, ".concat(username, "!"));
```

The generated JavaScript depends on the configured target.

# 🗁 Installing TypeScript

Install TypeScript globally:

```bash
npm install --global typescript
```

Check the installed version:

```bash
tsc --version
```

For most projects, a local development dependency is preferable:

```bash
npm install --save-dev typescript
```

A local installation makes the compiler version reproducible across development environments and CI pipelines.

Run the local compiler through:

```bash
npx tsc --version
```

Or use an npm script:

```json
{
  "scripts": {
    "typecheck": "tsc --noEmit"
  },
  "devDependencies": {
    "typescript": "^5.0.0"
  }
}
```

The version shown is illustrative; use the version appropriate for your project.

# 🗁 Compiling a Single File

Compile a TypeScript file:

```bash
tsc index.ts
```

Compile multiple files:

```bash
tsc index.ts app.ts utils.ts
```

By default, TypeScript generates JavaScript files in the corresponding output locations unless output options are configured.

# 🗁 Compiling with a Target

Use the `--target` option to specify the ECMAScript version of the generated JavaScript.

```bash
tsc index.ts --target ES2022
```

Another example:

```bash
tsc index.ts --target ES5
```

The target determines which JavaScript syntax features TypeScript transforms or preserves.

# 🗁 Compiling with a Module System

Use the `--module` option to configure module output.

Example:

```bash
tsc index.ts --module CommonJS
```

For modern ECMAScript modules:

```bash
tsc index.ts --module ESNext
```

For Node.js-aware module behavior:

```bash
tsc index.ts --module NodeNext
```

The module option should match the runtime or build system.

# 🗁 Using `tsconfig.json`

A `tsconfig.json` file defines the compiler configuration for a TypeScript project.

Create one using:

```bash
tsc --init
```

A typical configuration:

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
    "skipLibCheck": true
  },
  "include": ["src"],
  "exclude": ["node_modules", "dist"]
}
```

Compile the project:

```bash
tsc
```

When a `tsconfig.json` is found in the current directory, TypeScript uses it to determine which files to compile and which compiler options to apply.

# 🗁 Compiling a Specific Configuration

Use `-p` or `--project` to specify a configuration file or project directory.

```bash
tsc -p tsconfig.json
```

You can also specify another configuration:

```bash
tsc -p tsconfig.build.json
```

This is useful when a project maintains separate configurations for development, testing, and production builds.

# 🗁 Important: `tsc` and Explicit File Arguments

There is an important difference between these commands:

```bash
tsc
```

```bash
tsc index.ts
```

The first command uses the discovered `tsconfig.json`.

The second command compiles the explicitly supplied file and does not use the project configuration in the same way. When input files are specified directly, TypeScript ignores `tsconfig.json`.

If you need to compile a project with its configuration, use:

```bash
tsc -p .
```

Do not combine `--project` with explicit source-file arguments.

# 🗁 Type Checking Without JavaScript Output

Use `--noEmit` to perform type checking without generating JavaScript files.

```bash
tsc --noEmit
```

This is particularly useful when another tool, such as Vite or esbuild, handles JavaScript compilation.

You can also configure it permanently:

```json
{
  "compilerOptions": {
    "noEmit": true,
    "strict": true
  }
}
```

Then run:

```bash
tsc
```

# 🗁 Watching Files

The `--watch` option starts TypeScript's watch mode.

```bash
tsc --watch
```

Or:

```bash
tsc -w
```

TypeScript monitors relevant project files and recompiles when changes are detected.

This is useful during development when you want automatic compilation.

# 🗁 Generating Declaration Files

Use `--declaration` to generate `.d.ts` files.

```bash
tsc --declaration
```

Example:

```typescript
export function greet(name: string): string {
  return `Hello, ${name}!`;
}
```

Generated declaration:

```typescript
export declare function greet(name: string): string;
```

Declaration files describe the public types of a module without containing its implementation.

They are particularly important when developing TypeScript libraries.

# 🗁 Generating Source Maps

Use `--sourceMap` to generate source map files.

```bash
tsc --sourceMap
```

Source maps connect generated JavaScript back to its original TypeScript source.

This improves debugging because browser developer tools and supported debuggers can display the original TypeScript code.

# 🗁 Cleaning and Inspecting Output

Several compiler options help inspect and manage compilation.

| Command | Purpose |
| --- | --- |
| `tsc --version` | Displays the compiler version |
| `tsc --help` | Displays available compiler options |
| `tsc --init` | Creates a TypeScript configuration |
| `tsc --showConfig` | Displays the resolved configuration |
| `tsc --listFiles` | Lists files included in compilation |
| `tsc --explainFiles` | Explains why files are included |
| `tsc --listEmittedFiles` | Lists generated output files |
| `tsc --noEmit` | Checks types without generating output |
| `tsc --watch` | Starts watch mode |
| `tsc -b` | Builds projects and their references |

For example:

```bash
tsc --showConfig
```

This is useful when a configuration inherits settings from other `tsconfig.json` files.

# 🗁 Understanding Compilation Errors

Consider this code:

```typescript
const age: number = "25";
```

Run:

```bash
tsc index.ts
```

TypeScript reports a type error because a string cannot be assigned to a variable declared as `number`.

Correct version:

```typescript
const age: number = 25;
```

The compiler checks types before emitting JavaScript.

However, TypeScript's type system does not guarantee that all runtime errors are prevented. Runtime validation and appropriate error handling are still important.

# 🗁 Using `tsc` with npm Scripts

You can define reusable compiler commands in `package.json`.

```json
{
  "scripts": {
    "build": "tsc -p tsconfig.build.json",
    "typecheck": "tsc --noEmit",
    "watch": "tsc --watch"
  },
  "devDependencies": {
    "typescript": "^5.0.0"
  }
}
```

Run the scripts:

```bash
npm run build
```

```bash
npm run typecheck
```

```bash
npm run watch
```

Package scripts automatically use the project's locally installed TypeScript executable.

# 🗁 Best Practices

* Prefer a local TypeScript installation for project consistency.
* Keep compiler configuration in `tsconfig.json`.
* Use `tsc --noEmit` for standalone type checking.
* Enable `strict` for stronger type safety.
* Use `--watch` during development when continuous compilation is useful.
* Use `tsc -b` for projects that use Project References.
* Generate declaration files when building TypeScript libraries.
* Use npm scripts to standardize common compiler commands.
* Keep generated JavaScript separate from source files.
* Use `tsc --showConfig` when debugging configuration problems.

# 🗁 Summary

The TypeScript Compiler (`tsc`) is the primary command-line tool for compiling and type-checking TypeScript projects.

Its main capabilities include:

* Compiling TypeScript into JavaScript.
* Performing static type checking.
* Reading `tsconfig.json`.
* Generating declaration files.
* Generating source maps.
* Watching files for changes.
* Building projects with references.
* Inspecting compiler configuration and output.

Understanding `tsc` is essential for managing the TypeScript development and build workflow.
