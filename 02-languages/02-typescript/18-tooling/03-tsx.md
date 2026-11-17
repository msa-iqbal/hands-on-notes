# tsx

`tsx` is a TypeScript execution tool for Node.js that allows developers to run TypeScript and modern JavaScript files without manually compiling them first.

It is designed as a fast development tool with support for TypeScript, JSX, ECMAScript modules, CommonJS, and modern Node.js workflows.

# 🗁 What Is tsx?

Node.js traditionally executes JavaScript files. TypeScript files need to be transformed before Node.js can execute them.

A traditional workflow looks like this:

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

With `tsx`, you can execute TypeScript files directly:

```text
TypeScript (.ts)
       |
       v
      tsx
       |
       v
 TypeScript Transformation
       |
       v
     Node.js
       |
       v
  Program Execution
```

Example:

```typescript
const message: string = "Hello from tsx!";

console.log(message);
```

Execute it:

```bash
npx tsx index.ts
```

# 🗁 Installing tsx

Install `tsx` as a development dependency:

```bash
npm install --save-dev tsx
```

For projects that need Node.js type definitions:

```bash
npm install --save-dev @types/node
```

# 🗁 Running TypeScript Files

Create a TypeScript file:

**index.ts**

```typescript
interface User {
  id: number;
  name: string;
}

const user: User = {
  id: 1,
  name: "Alice",
};

console.log(`User: ${user.name}`);
```

Execute it:

```bash
npx tsx index.ts
```

Output:

```text
User: Alice
```

`tsx` transforms TypeScript syntax and executes the resulting code through Node.js.

# 🗁 Running Files with npm Scripts

You can configure `tsx` inside `package.json`.

```json
{
  "scripts": {
    "dev": "tsx src/index.ts"
  },
  "devDependencies": {
    "tsx": "^4.0.0"
  }
}
```

Run:

```bash
npm run dev
```

This provides a convenient development command.

# 🗁 Watch Mode

One of the useful features of `tsx` is its built-in watch mode.

Watch a TypeScript file:

```bash
npx tsx watch src/index.ts
```

When a watched file changes, `tsx` automatically restarts the application.

You can configure it through npm scripts:

```json
{
  "scripts": {
    "dev": "tsx watch src/index.ts"
  }
}
```

Run:

```bash
npm run dev
```

Watch mode is useful for backend development, command-line applications, and development scripts.

# 🗁 Working with ES Modules

`tsx` supports modern ECMAScript module syntax.

Example:

```typescript
import { readFile } from "node:fs/promises";

const content = await readFile("package.json", "utf8");

console.log(content);
```

Execute:

```bash
npx tsx index.ts
```

You can use ESM syntax without manually configuring a separate TypeScript loader command.

For projects that use ESM, keep the package and TypeScript module settings consistent with the intended runtime behavior.

# 🗁 Working with CommonJS

`tsx` can also execute TypeScript in CommonJS-oriented projects.

Example:

```typescript
import fs from "node:fs";

const content = fs.readFileSync("package.json", "utf8");

console.log(content);
```

Run:

```bash
npx tsx index.ts
```

The actual module behavior depends on the project's package configuration and TypeScript settings.

# 🗁 Running JSX and TSX Files

`tsx` supports JSX and TSX syntax.

Create a file:

**component.tsx**

```tsx
interface ButtonProps {
  label: string;
}

function Button({ label }: ButtonProps) {
  return <button>{label}</button>;
}

console.log(Button({ label: "Click me" }));
```

Execute:

```bash
npx tsx component.tsx
```

The example demonstrates JSX transformation. Executing a React component in a real application still requires the appropriate React runtime and rendering environment.

# 🗁 Importing JSON Files

Modern Node.js applications frequently work with JSON configuration and data files.

Example:

**config.json**

```json
{
  "appName": "My Application",
  "port": 3000
}
```

Import it:

```typescript
import config from "./config.json" with { type: "json" };

console.log(config.appName);
console.log(config.port);
```

The JSON import syntax and runtime support depend on the Node.js version and module configuration. For broader compatibility, you can also read JSON using Node.js filesystem APIs.

# 🗁 Using tsx with Type Checking

`tsx` focuses on executing TypeScript code. It does not perform full TypeScript type checking as part of normal execution.

Consider this example:

```typescript
const age: number = "25";

console.log(age);
```

This code contains a TypeScript type error.

`tsx` may still execute it because it transforms TypeScript syntax without performing the full type-checking process.

Use the TypeScript compiler separately:

```bash
npx tsc --noEmit
```

This checks the project without generating JavaScript files.

A useful development setup is:

```json
{
  "scripts": {
    "dev": "tsx watch src/index.ts",
    "typecheck": "tsc --noEmit"
  }
}
```

Run the development server:

```bash
npm run dev
```

Run type checking:

```bash
npm run typecheck
```

# 🗁 Using tsx with Node.js

You can use `tsx` as a Node.js loader.

For example:

```bash
node --import tsx src/index.ts
```

This registers `tsx` with Node.js so that TypeScript files can be loaded through its module system.

You can also configure it in `package.json`:

```json
{
  "scripts": {
    "dev": "node --import tsx src/index.ts"
  }
}
```

For most straightforward development workflows, the direct `tsx` command is simpler:

```bash
tsx src/index.ts
```

# 🗁 tsx vs ts-node

Both tools execute TypeScript files directly in Node.js, but their implementation and development experience differ.

| Feature | tsx | ts-node |
| --- | --- | --- |
| TypeScript execution | Yes | Yes |
| Full type checking | No | Yes, by default |
| Watch mode | Built in | Requires additional tooling or Node.js watch mode |
| ESM support | Yes | Yes, through its ESM loader |
| CommonJS support | Yes | Yes |
| JSX/TSX transformation | Yes | Depends on compiler configuration |
| Development experience | Fast and simple | More compiler-integrated |
| Separate type checking | Requires `tsc` | Can use built-in checking |

# 🗁 tsx vs tsc

These tools solve different problems.

| Feature | tsx | tsc |
| --- | --- | --- |
| Primary purpose | Execute TypeScript | Compile and type-check TypeScript |
| Direct execution | Yes | No |
| Type checking | No | Yes |
| JavaScript output | No separate build output by default | Yes, unless `noEmit` is enabled |
| Watch mode | Yes | Yes |
| Development scripts | Excellent fit | Usually combined with Node.js |
| Production compilation | Not a replacement for a build process | Commonly used |

A typical workflow:

```bash
# Start development
npx tsx watch src/index.ts

# Check TypeScript types
npx tsc --noEmit

# Build JavaScript
npx tsc -p tsconfig.build.json

# Execute compiled output
node dist/index.js
```

# 🗁 Common Errors

**Cannot find module**

Possible causes:

* Incorrect import paths.
* Missing dependencies.
* Incorrect file extensions.
* Incompatible module configuration.

Verify the import path and install the required packages.

**TypeScript errors are not reported**

`tsx` does not perform full type checking during normal execution.

Run:

```bash
npx tsc --noEmit
```

**Unsupported runtime features**

`tsx` transforms TypeScript syntax, but it does not automatically make every JavaScript runtime feature available in an older Node.js version.

Check the Node.js version and required runtime APIs.

**JSX transformation problems**

Verify that the JSX configuration in `tsconfig.json` matches the framework or runtime you are using.

# 🗁 Best Practices

* Install `tsx` locally as a development dependency.
* Use `tsx watch` for development workflows.
* Run `tsc --noEmit` separately for type checking.
* Keep Node.js and TypeScript configurations consistent.
* Use a dedicated build process for production deployments.
* Avoid assuming that successful execution means the code is type-safe.
* Use the direct `tsx` command for simple TypeScript execution.

# 🗁 Summary

`tsx` is a fast TypeScript execution tool for Node.js.

Its main capabilities include:

* Direct execution of TypeScript files.
* Built-in watch mode.
* Support for ECMAScript modules and CommonJS.
* JSX and TSX transformation.
* Integration with Node.js.
* Development-friendly execution without manual compilation.

It is particularly useful for modern Node.js development, backend applications, scripts, and development tooling.
