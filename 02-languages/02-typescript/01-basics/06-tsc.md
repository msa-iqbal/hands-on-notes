# TypeScript Compiler (tsc)

The TypeScript Compiler, commonly known as `tsc`, is the official command-line compiler for TypeScript.

It is responsible for checking TypeScript code, reporting type errors, and optionally generating JavaScript output.

The compiler is distributed through the `typescript` npm package.

You can use `tsc` to:

* Compile TypeScript files into JavaScript.
* Perform static type checking.
* Configure JavaScript output versions.
* Configure module systems.
* Generate declaration files.
* Generate source maps.
* Compile entire TypeScript projects.
* Monitor files for changes.
* Validate code without generating output.

# 🗁 Install TypeScript Compiler

The recommended approach for most projects is to install TypeScript locally as a development dependency.

```bash
npm install --save-dev typescript
```

Verify the installed compiler version:

```bash
npx tsc --version
```

Example output:

```text
Version 5.x.x
```

The exact version depends on the installed TypeScript release.

# 🗁 Local vs Global Installation

TypeScript can be installed locally or globally.

**Local installation:**

```bash
npm install --save-dev typescript
```

Run the compiler:

```bash
npx tsc
```

Advantages:

* Each project can use its own TypeScript version.
* Team members can use the same compiler version.
* The compiler version is recorded in `package.json` and the lockfile.
* CI environments can install the project's dependencies consistently.

**Global installation:**

```bash
npm install --global typescript
```

Run the compiler directly:

```bash
tsc --version
```

Global installation makes the compiler available from different directories, but different projects may require different TypeScript versions.

For reproducible project builds, local installation is generally preferred.

# 🗁 Check Compiler Version

Use:

```bash
npx tsc --version
```

This displays the version of the TypeScript compiler available through the current project.

You can also inspect the installed package:

```bash
npm list typescript
```

To see globally installed TypeScript:

```bash
npm list --global typescript
```

# 🗁 Create a TypeScript Project

Create a project directory:

```bash
mkdir typescript-compiler
cd typescript-compiler
```

Initialize npm:

```bash
npm init -y
```

Install TypeScript:

```bash
npm install --save-dev typescript
```

Create a source directory:

```bash
mkdir src
```

Create `src/index.ts`:

```typescript
const message: string = "Hello from TypeScript Compiler!";

console.log(message);
```

Your project structure:

```text
typescript-compiler/
│
├── node_modules/
│
├── src/
│   └── index.ts
│
├── package.json
└── package-lock.json
```

# 🗁 Initialize tsconfig.json

The `tsconfig.json` file defines the compiler configuration for a TypeScript project.

Generate a configuration file:

```bash
npx tsc --init
```

This creates:

```text
tsconfig.json
```

The generated configuration may contain many commented compiler options.

A simplified configuration for this project:

```json
{
    "compilerOptions": {
        "target": "ES2020",
        "module": "CommonJS",
        "rootDir": "./src",
        "outDir": "./dist",
        "strict": true,
        "noEmitOnError": true,
        "sourceMap": true
    },
    "include": [
        "src/**/*.ts"
    ]
}
```

# 🗁 Compile a Single TypeScript File

You can compile an individual TypeScript file without creating a `tsconfig.json` file.

Create `index.ts`:

```typescript
const username: string = "John";

console.log(`Hello, ${username}!`);
```

Compile:

```bash
npx tsc index.ts
```

The compiler generates:

```text
index.js
```

Run the generated JavaScript:

```bash
node index.js
```

Expected output:

```text
Hello, John!
```

When you specify input files directly, `tsc` does not use the project's `tsconfig.json` in the same way as a project compilation. Use the appropriate command-line options or compile the project without specifying source files to apply the project configuration.

# 🗁 Compile an Entire Project

When a project contains `tsconfig.json`, execute:

```bash
npx tsc
```

The compiler reads the configuration and compiles the included TypeScript files.

With the earlier configuration, the output directory will look like this:

```text
dist/
└── index.js
```

Run the application:

```bash
node dist/index.js
```

# 🗁 Common tsc Commands

| Command | Description |
| --- | --- |
| `npx tsc --version` | Displays the compiler version |
| `npx tsc --help` | Displays compiler help |
| `npx tsc --init` | Creates a TypeScript configuration |
| `npx tsc index.ts` | Compiles a specific file |
| `npx tsc` | Compiles the configured project |
| `npx tsc --watch` | Watches files and recompiles changes |
| `npx tsc --noEmit` | Performs type checking without generating output |
| `npx tsc --noEmitOnError` | Prevents output when errors exist |
| `npx tsc --listFiles` | Lists files included in compilation |
| `npx tsc --showConfig` | Displays the resolved compiler configuration |
| `npx tsc --diagnostics` | Displays compilation performance information |
| `npx tsc --pretty` | Enables formatted compiler output |

# 🗁 Important Compiler Options

Compiler options control how TypeScript checks source code and generates JavaScript.

**Target**

Specifies the JavaScript language version to generate.

```bash
npx tsc --target ES2020
```

**Module**

Specifies the module format used in generated JavaScript.

```bash
npx tsc --module commonjs
```

**OutDir**

Specifies the output directory.

```bash
npx tsc --outDir dist
```

**RootDir**

Specifies the root directory of input source files.

```bash
npx tsc --rootDir src
```

**Strict**

Enables a collection of stricter type-checking options.

```bash
npx tsc --strict
```

**NoEmit**

Checks the code without generating output files.

```bash
npx tsc --noEmit
```

**NoEmitOnError**

Prevents JavaScript output generation when compilation errors exist.

```bash
npx tsc --noEmitOnError
```

# 🗁 Watch Mode

Watch mode continuously monitors TypeScript files and recompiles them when changes occur.

Run:

```bash
npx tsc --watch
```

Short form:

```bash
npx tsc -w
```

Example:

```text
Starting compilation in watch mode...

Found 0 errors. Watching for file changes.
```

When you modify a TypeScript file, the compiler automatically checks and recompiles the project.

Stop watch mode:

```text
Ctrl + C
```

Watch mode is especially useful during development.

# 🗁 Type Checking Without JavaScript Generation

Sometimes another tool handles the build process, and you only need TypeScript to validate types.

Use:

```bash
npx tsc --noEmit
```

This checks the project without generating JavaScript files.

For example, a frontend application may use Vite for development and production builds while using `tsc --noEmit` for static type checking.

This separation allows the build tool and TypeScript compiler to perform their respective tasks.

# 🗁 Generate Declaration Files

Declaration files describe the types exposed by TypeScript or JavaScript modules.

Enable declaration generation:

```bash
npx tsc --declaration
```

Or configure it in `tsconfig.json`:

```json
{
    "compilerOptions": {
        "declaration": true,
        "outDir": "./dist"
    }
}
```

Example source file:

```typescript
export function multiply(a: number, b: number): number {
    return a * b;
}
```

Generated declaration:

```typescript
export declare function multiply(a: number, b: number): number;
```

Declaration files are useful when creating libraries that other TypeScript projects will consume.

# 🗁 Generate Source Maps

Source maps connect generated JavaScript to its original TypeScript source.

Enable them:

```bash
npx tsc --sourceMap
```

Or configure:

```json
{
    "compilerOptions": {
        "sourceMap": true
    }
}
```

The compiler generates:

```text
dist/
├── index.js
└── index.js.map
```

Source maps help developers debug the original TypeScript source instead of inspecting only generated JavaScript.

# 🗁 Show Resolved Configuration

Use:

```bash
npx tsc --showConfig
```

This displays the effective compiler configuration after TypeScript resolves the configuration file and its inherited settings.

It is particularly useful when:

* Extending another `tsconfig.json`.
* Debugging unexpected compiler behavior.
* Working with multiple configurations.
* Verifying compiler options.

# 🗁 Understand File Inclusion

TypeScript uses configuration options to determine which files belong to a project.

Example:

```json
{
    "include": [
        "src/**/*.ts"
    ],
    "exclude": [
        "node_modules",
        "dist"
    ]
}
```

`include` specifies files or patterns to include.

`exclude` removes matching files from the initial set discovered through `include`.

However, excluded files can still enter the compilation when imported by an included file or referenced through other compiler mechanisms.

# 🗁 Use tsc with npm Scripts

You can create reusable compiler commands inside `package.json`.

```json
{
    "scripts": {
        "build": "tsc",
        "watch": "tsc --watch",
        "typecheck": "tsc --noEmit",
        "clean-build": "tsc --build"
    },
    "devDependencies": {
        "typescript": "^5.0.0"
    }
}
```

The TypeScript version shown is an illustrative version range; use the version actually installed in your project.

Execute:

```bash
npm run build
npm run watch
npm run typecheck
npm run clean-build
```

The `clean-build` script uses project build mode and is intended for projects configured with project references or other build-mode settings.

# 🗁 Project References and Build Mode

TypeScript supports project references to organize large applications into smaller, independently configured projects.

For example:

```text
my-project/
│
├── packages/
│   ├── shared/
│   │   └── tsconfig.json
│   │
│   └── app/
│       └── tsconfig.json
│
└── tsconfig.json
```

A root configuration can reference the individual projects:

```json
{
    "files": [],
    "references": [
        {
            "path": "./packages/shared"
        },
        {
            "path": "./packages/app"
        }
    ]
}
```

Build the referenced projects:

```bash
npx tsc --build
```

Project build mode understands project dependencies and builds projects in the required order.

This is useful for monorepos and large TypeScript applications.

# 🗁 Troubleshooting Common tsc Problems

**Problem: `tsc: command not found`**

Possible solution:

```bash
npm install --save-dev typescript
```

Then use:

```bash
npx tsc --version
```

**Problem: Cannot find module or its corresponding type declarations**

Possible causes:

* The package is not installed.
* The import path is incorrect.
* Type declarations are missing.
* Module resolution settings are incompatible.

Check the package installation:

```bash
npm install package-name
```

If the package needs separate type declarations, check whether an appropriate `@types` package exists.

**Problem: TypeScript files are not being compiled**

Check:

* The `include` configuration.
* The `exclude` configuration.
* The `rootDir` setting.
* Whether the correct `tsconfig.json` is being used.

Inspect the resolved configuration:

```bash
npx tsc --showConfig
```

**Problem: JavaScript output is not generated**

Check whether:

```json
{
    "compilerOptions": {
        "noEmit": true
    }
}
```

is enabled.

Also check whether `noEmitOnError` is enabled and the project contains compilation errors.

# 🗁 Best Practices

* Prefer local TypeScript installation for project consistency.
* Use `npx tsc` to execute the project's installed compiler.
* Maintain a `tsconfig.json` file for project-wide configuration.
* Enable strict type checking.
* Use `npx tsc --noEmit` when a separate build tool generates JavaScript.
* Use watch mode during development.
* Keep generated output in a dedicated directory.
* Use declaration generation when publishing typed libraries.
* Use source maps to improve debugging.
* Use project references when organizing large TypeScript codebases.
* Integrate type checking into automated build and CI workflows.

# 🗁 Summary

The `tsc` command is the primary command-line interface for the TypeScript compiler.

You have learned:

* How to install TypeScript locally and globally.
* How to check the compiler version.
* How to initialize `tsconfig.json`.
* How to compile individual files and entire projects.
* How to use common compiler commands.
* How to configure compiler options.
* How to use watch mode.
* How to perform type checking without generating JavaScript.
* How to generate declaration files and source maps.
* How to inspect project configuration.
* How to use project references and build mode.
* How to troubleshoot common compiler problems.

The most important commands to remember are:

```bash
npx tsc --version
npx tsc --init
npx tsc
npx tsc index.ts
npx tsc --watch
npx tsc --noEmit
npx tsc --showConfig
npx tsc --build
```
