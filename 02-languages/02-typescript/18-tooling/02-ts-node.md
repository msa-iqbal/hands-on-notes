# ts-node

`ts-node` is a TypeScript execution tool that allows developers to run TypeScript files directly in Node.js without manually compiling them into JavaScript first.

It integrates TypeScript compilation with Node.js execution, making it useful for development scripts, command-line tools, backend applications, and quick experimentation.

# 🗁 What Is ts-node?

Normally, Node.js executes JavaScript files rather than TypeScript files.

A traditional TypeScript workflow looks like this:

```text
TypeScript (.ts)
       |
       v
TypeScript Compiler (tsc)
       |
       v
JavaScript (.js)
       |
       v
Node.js
       |
       v
Program Execution
```

With `ts-node`, you can execute TypeScript files directly:

```text
TypeScript (.ts)
       |
       v
     ts-node
       |
       v
 TypeScript Compilation
       |
       v
    Node.js
       |
       v
 Program Execution
```

Example:

```typescript
const message: string = "Hello from TypeScript!";

console.log(message);
```

Execute it using:

```bash
npx ts-node index.ts
```

You do not need to manually generate a JavaScript file first.

# 🗁 Installing ts-node

Install `typescript` and `ts-node` as development dependencies:

```bash
npm install --save-dev typescript ts-node
```

For projects that use Node.js type definitions, install:

```bash
npm install --save-dev @types/node
```

Your development dependencies will include:

```json
{
  "devDependencies": {
    "@types/node": "^22.0.0",
    "ts-node": "^10.9.0",
    "typescript": "^5.0.0"
  }
}
```

The versions are illustrative. Use compatible versions appropriate for your project.

# 🗁 Running TypeScript Files

Create a file:

**index.ts**

```typescript
function greet(name: string): string {
  return `Hello, ${name}!`;
}

console.log(greet("Alice"));
```

Run it:

```bash
npx ts-node index.ts
```

Output:

```text
Hello, Alice!
```

`ts-node` handles TypeScript execution without requiring a separate manual compilation step.

# 🗁 Using ts-node with npm Scripts

You can define an npm script to execute TypeScript files.

**package.json**

```json
{
  "scripts": {
    "dev": "ts-node src/index.ts"
  },
  "devDependencies": {
    "ts-node": "^10.9.0",
    "typescript": "^5.0.0"
  }
}
```

Run:

```bash
npm run dev
```

This makes the development workflow easier and more consistent.

# 🗁 Using ts-node with tsconfig.json

`ts-node` reads the TypeScript project configuration and uses its compiler options.

Example:

**tsconfig.json**

```json
{
  "compilerOptions": {
    "target": "ES2022",
    "module": "CommonJS",
    "strict": true,
    "esModuleInterop": true
  }
}
```

Execute:

```bash
npx ts-node index.ts
```

The configured compiler options influence how `ts-node` transforms TypeScript before execution.

You can also override compiler options through command-line arguments:

```bash
npx ts-node --compiler-options '{"module":"CommonJS"}' index.ts
```

# 🗁 Using ts-node in CommonJS Projects

CommonJS is the traditional Node.js module system.

A CommonJS-oriented TypeScript configuration:

```json
{
  "compilerOptions": {
    "target": "ES2022",
    "module": "CommonJS",
    "moduleResolution": "Node10",
    "strict": true,
    "esModuleInterop": true
  }
}
```

Example:

```typescript
import fs from "node:fs";

const content = fs.readFileSync("package.json", "utf8");

console.log(content);
```

Run:

```bash
npx ts-node index.ts
```

This workflow is suitable for projects configured to use CommonJS.

# 🗁 Using ts-node with ES Modules

Modern Node.js projects can use ECMAScript modules.

A typical package configuration:

**package.json**

```json
{
  "type": "module"
}
```

**tsconfig.json**

```json
{
  "compilerOptions": {
    "target": "ES2022",
    "module": "NodeNext",
    "moduleResolution": "NodeNext",
    "strict": true
  }
}
```

For projects using the `ts-node` ESM loader:

```bash
node --loader ts-node/esm src/index.ts
```

You can also configure an npm script:

```json
{
  "scripts": {
    "dev": "node --loader ts-node/esm src/index.ts"
  }
}
```

The ESM loader integrates TypeScript execution with Node.js's module loading system.

ESM loader behavior can vary across Node.js versions, so verify compatibility with the versions used by your project.

# 🗁 Using ts-node with Watch Mode

`ts-node` itself does not provide the same built-in watch workflow as `tsc --watch`.

You can combine it with Node.js's watch mode:

```bash
node --watch --import ts-node/register src/index.ts
```

This example uses the CommonJS registration hook through Node.js's preload mechanism.

For projects that use ESM, use an appropriate ESM loader configuration instead.

Another option is to use a development tool such as `nodemon`:

```bash
npm install --save-dev nodemon
```

Configure:

```json
{
  "scripts": {
    "dev": "nodemon --watch src --exec ts-node src/index.ts"
  }
}
```

This restarts the process when watched files change.

# 🗁 Using ts-node with Transpile Only

By default, `ts-node` performs type checking during execution.

For faster development execution, you can use transpile-only mode:

```bash
npx ts-node --transpile-only index.ts
```

Or configure it in `tsconfig.json`:

```json
{
  "ts-node": {
    "transpileOnly": true
  }
}
```

This skips full type checking and focuses on transforming TypeScript into executable JavaScript.

However, skipping type checking means TypeScript errors may not be reported during execution.

You should run a separate type-checking command:

```bash
npx tsc --noEmit
```

# 🗁 Using ts-node with REPL

`ts-node` provides an interactive REPL (Read-Eval-Print Loop) for executing TypeScript expressions.

Start it:

```bash
npx ts-node
```

Example:

```typescript
const age: number = 25;

age + 5;
```

Output:

```text
30
```

The REPL is useful for experimenting with TypeScript syntax and testing small pieces of code.

# 🗁 Useful ts-node Options

| Option | Purpose |
| --- | --- |
| `-P` | Specifies a TypeScript configuration file |
| `-C` | Selects a compiler implementation |
| `-T` | Enables transpile-only mode |
| `--transpile-only` | Skips full type checking |
| `--compiler-options` | Overrides compiler options |
| `--showConfig` | Displays the resolved configuration |
| `--files` | Loads files specified through `files`, `include`, and `exclude` |
| `--cwd` | Sets the working directory |

Examples:

```bash
npx ts-node --showConfig
```

```bash
npx ts-node --transpile-only index.ts
```

```bash
npx ts-node -P tsconfig.dev.json index.ts
```

# 🗁 ts-node vs tsc

Both tools work with TypeScript, but their primary purposes differ.

| Feature | ts-node | tsc |
| --- | --- | --- |
| Primary purpose | Execute TypeScript files | Compile and type-check TypeScript |
| Generates JavaScript files | Usually no | Yes, unless `noEmit` is enabled |
| Executes code | Yes | No |
| Type checking | Yes, by default | Yes |
| Watch support | Through additional tooling or Node.js | Built-in |
| Development scripts | Useful | Usually combined with Node.js |
| Production builds | Not usually preferred | Commonly used |

A common development workflow is:

```bash
npx ts-node src/index.ts
```

A production-oriented build workflow is:

```bash
npx tsc -p tsconfig.build.json
```

Then execute the generated JavaScript:

```bash
node dist/index.js
```

# 🗁 Common Errors

**Cannot find module**

Possible causes:

* Incorrect import paths.
* Missing dependencies.
* Incompatible module configuration.
* Incorrect file extensions.

Verify your import paths and module settings.

**Unknown file extension `.ts`**

This commonly occurs when Node.js is trying to load a TypeScript file without an appropriate TypeScript loader.

For ESM projects, configure and use the appropriate `ts-node` ESM loader.

**TypeScript compilation errors**

`ts-node` performs type checking by default.

Run the compiler separately to inspect project errors:

```bash
npx tsc --noEmit
```

**Module system incompatibility**

Using CommonJS execution settings with an ESM package, or the reverse, can cause module loading errors.

Ensure that `package.json`, `tsconfig.json`, and the execution command use compatible module settings.

# 🗁 Best Practices

* Install `ts-node` locally as a development dependency.
* Keep TypeScript compiler settings in `tsconfig.json`.
* Use it for development scripts and command-line tools.
* Use `--transpile-only` only when a separate type-checking step exists.
* Keep CommonJS and ESM configurations consistent.
* Use a dedicated production build process.
* Prefer executing compiled JavaScript in production when that fits the deployment architecture.
* Verify compatibility between Node.js, TypeScript, and `ts-node` versions.

# 🗁 Summary

`ts-node` allows developers to execute TypeScript files directly in Node.js without manually compiling them first.

Its main capabilities include:

* Direct TypeScript execution.
* Integration with `tsconfig.json`.
* CommonJS and ESM execution workflows.
* Interactive REPL support.
* Transpile-only execution.
* Integration with development scripts and watch tools.

It is particularly useful for development workflows, scripts, and small TypeScript applications.
