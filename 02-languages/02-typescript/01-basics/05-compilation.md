# TypeScript Compilation

TypeScript is a programming language that extends JavaScript with static type checking and additional development features.

Browsers and standard JavaScript runtimes do not natively execute TypeScript syntax. TypeScript source code is generally transformed into JavaScript before execution.

This transformation process is commonly called **TypeScript compilation** or **transpilation**.

The TypeScript compiler, known as `tsc`, performs several important tasks:

* Parses TypeScript source code.
* Checks types and reports type errors.
* Validates language syntax.
* Transforms TypeScript syntax into JavaScript.
* Generates JavaScript output files.
* Optionally generates source maps and declaration files.

# 🗁 How TypeScript Compilation Works

A typical TypeScript development workflow follows this process:

```text
TypeScript Source Code
       |
       v
    index.ts
       |
       v
TypeScript Compiler (tsc)
       |
       v
Parsing and Type Checking
       |
       v
JavaScript Transformation
       |
       v
    index.js
       |
       v
JavaScript Runtime
       |
       v
Application Output
```

The TypeScript compiler checks the source code and generates JavaScript according to the configured compiler options.

# 🗁 TypeScript Source Code

Consider the following TypeScript program:

```typescript
const username: string = "John";
const age: number = 25;

function greet(name: string): string {
    return `Hello, ${name}!`;
}

console.log(greet(username));
console.log(age);
```

This program contains TypeScript-specific syntax:

* `username: string`
* `age: number`
* `name: string`
* `: string` as the function return type

These annotations help the compiler detect incompatible assignments and function arguments.

# 🗁 Compile TypeScript into JavaScript

Create a file named `index.ts` and execute:

```bash
npx tsc index.ts
```

The compiler generates an `index.js` file.

A possible output is:

```javascript
const username = "John";
const age = 25;

function greet(name) {
    return `Hello, ${name}!`;
}

console.log(greet(username));
console.log(age);
```

Notice that TypeScript type annotations have been removed.

The generated JavaScript can now be executed using Node.js:

```bash
node index.js
```

Expected output:

```text
Hello, John!
25
```

# 🗁 Compilation vs Transpilation

These terms are closely related but have slightly different meanings.

**Compilation** generally refers to transforming source code into another form, such as machine code, bytecode, or another programming language.

**Transpilation** specifically refers to transforming source code from one programming language or language version into another.

TypeScript is commonly described as being transpiled into JavaScript because its source code is transformed into JavaScript.

However, the TypeScript compiler also performs type checking and other compilation-related tasks.

# 🗁 Type Checking During Compilation

Type checking is one of TypeScript's most important features.

Consider:

```typescript
let price: number = 100;

price = "One hundred";
```

Compile the file:

```bash
npx tsc index.ts
```

The compiler reports an error similar to:

```text
error TS2322: Type 'string' is not assignable to type 'number'.
```

This happens because the variable `price` is declared with the `number` type.

Correct the code:

```typescript
let price: number = 100;

price = 200;
```

Now the assignment is valid.

Type checking helps detect many mistakes before the application runs.

# 🗁 TypeScript Can Generate JavaScript Even When Errors Exist

By default, the TypeScript compiler may generate JavaScript files even when type errors are reported.

For example:

```typescript
let age: number = 25;

age = "Twenty-five";
```

Run:

```bash
npx tsc index.ts
```

The compiler reports the type error and may still emit `index.js`.

To prevent output generation when compilation errors occur, use:

```bash
npx tsc index.ts --noEmitOnError
```

You can also configure this behavior in `tsconfig.json`:

```json
{
    "compilerOptions": {
        "noEmitOnError": true
    }
}
```

This is useful in production build workflows where invalid source code should not produce build artifacts.

# 🗁 Understanding Compiler Options

TypeScript provides compiler options that control how source code is checked and transformed.

For example:

```bash
npx tsc index.ts --target ES2020 --module commonjs
```

This command specifies:

* `--target ES2020`: Generates JavaScript targeting ECMAScript 2020.
* `--module commonjs`: Uses CommonJS module syntax in generated JavaScript.

Some commonly used compiler options include:

| Option | Purpose |
| --- | --- |
| `target` | Specifies the JavaScript language version to generate |
| `module` | Controls the generated module format |
| `outDir` | Specifies the output directory |
| `rootDir` | Specifies the root directory of source files |
| `strict` | Enables strict type-checking options |
| `noEmit` | Checks code without generating JavaScript |
| `noEmitOnError` | Prevents output generation when errors exist |
| `sourceMap` | Generates source map files |
| `declaration` | Generates TypeScript declaration files |
| `removeComments` | Removes comments from generated JavaScript |

# 🗁 Compile Using a tsconfig.json File

For real-world projects, compiler options are usually stored in a `tsconfig.json` file.

Create the configuration:

```bash
npx tsc --init
```

A simple configuration could look like this:

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

Project structure:

```text
typescript-project/
│
├── src/
│   └── index.ts
│
├── dist/
│
├── package.json
└── tsconfig.json
```

Create `src/index.ts`:

```typescript
const message: string = "Hello from TypeScript!";

console.log(message);
```

Compile the project:

```bash
npx tsc
```

The compiler reads `tsconfig.json` and generates the JavaScript output inside `dist/`.

Run the generated file:

```bash
node dist/index.js
```

Expected output:

```text
Hello from TypeScript!
```

# 🗁 TypeScript Compilation Commands

The TypeScript compiler provides several useful commands.

**Compile a specific file:**

```bash
npx tsc index.ts
```

**Compile the project using `tsconfig.json`:**

```bash
npx tsc
```

**Compile and watch for file changes:**

```bash
npx tsc --watch
```

**Check types without generating JavaScript:**

```bash
npx tsc --noEmit
```

**Display compiler version:**

```bash
npx tsc --version
```

**Display compiler help:**

```bash
npx tsc --help
```

# 🗁 Watch Mode

Watch mode automatically recompiles the project when TypeScript files change.

Run:

```bash
npx tsc --watch
```

The compiler monitors the project files and recompiles them when changes are detected.

This is useful during development because you do not need to manually execute the compiler after every modification.

To stop watch mode, press:

```text
Ctrl + C
```

# 🗁 Source Maps

Source maps connect generated JavaScript code to its original TypeScript source.

When debugging compiled JavaScript, source maps allow browser developer tools and compatible debuggers to display the original TypeScript files.

Enable source maps:

```json
{
    "compilerOptions": {
        "target": "ES2020",
        "sourceMap": true
    }
}
```

Compile the project:

```bash
npx tsc
```

The compiler generates files similar to:

```text
dist/
├── index.js
└── index.js.map
```

The `.map` file contains mapping information between the generated JavaScript and the original TypeScript source.

# 🗁 Declaration Files

TypeScript can generate declaration files with the `.d.ts` extension.

Declaration files describe the types exposed by JavaScript modules or libraries.

Example source file:

```typescript
export function add(a: number, b: number): number {
    return a + b;
}
```

Enable declaration generation:

```json
{
    "compilerOptions": {
        "declaration": true,
        "outDir": "./dist"
    }
}
```

Compile:

```bash
npx tsc
```

The compiler generates JavaScript and a declaration file.

A simplified declaration file looks like this:

```typescript
export declare function add(a: number, b: number): number;
```

Declaration files provide type information without containing the implementation.

They are especially useful when developing reusable TypeScript libraries.

# 🗁 Does Compilation Remove All TypeScript Features?

Most TypeScript-only type information is erased during JavaScript emission.

For example:

```typescript
interface User {
    id: number;
    name: string;
}

const user: User = {
    id: 1,
    name: "John"
};
```

Generated JavaScript:

```javascript
const user = {
    id: 1,
    name: "John"
};
```

However, not every TypeScript feature disappears.

Some TypeScript constructs generate JavaScript runtime code.

For example, an enum:

```typescript
enum Direction {
    Up,
    Down,
    Left,
    Right
}

console.log(Direction.Up);
```

Can generate JavaScript code representing an enum-like object.

The exact output depends on the TypeScript compiler configuration and language target.

# 🗁 Compilation Does Not Guarantee Runtime Correctness

TypeScript checks many errors before execution, but it cannot guarantee that an application will behave correctly at runtime.

Consider an API response:

```typescript
interface User {
    id: number;
    name: string;
}

const user: User = JSON.parse('{"id":"invalid","name":123}');

console.log(user.id);
console.log(user.name);
```

The assignment is accepted because `JSON.parse()` returns `any`, which disables normal type checking for that value.

At runtime, the object contains a string for `id` and a number for `name`.

The declared interface does not validate the actual data.

External data should be validated at runtime before being trusted.

# 🗁 Compilation vs Execution

Compilation and execution are separate processes.

| Compilation | Execution |
| --- | --- |
| Transforms TypeScript into JavaScript | Runs JavaScript code |
| Performed by the TypeScript compiler | Performed by a JavaScript runtime |
| Detects static type errors | Reveals runtime errors |
| Produces JavaScript and optional supporting files | Produces application behavior and output |
| Common command: `npx tsc` | Common command: `node index.js` |

For example:

```bash
npx tsc index.ts
node index.js
```

The first command compiles the TypeScript file. The second executes the generated JavaScript file.

# 🗁 Best Practices

* Use `tsconfig.json` to manage project-wide compiler settings.
* Enable `strict` mode for stronger type checking.
* Use `noEmitOnError` when invalid code should not generate output.
* Keep source files and generated files in separate directories.
* Use `noEmit` when a separate build tool handles JavaScript generation.
* Enable source maps when debugging compiled applications.
* Generate declaration files when publishing reusable TypeScript libraries.
* Use watch mode during development.
* Remember that compilation does not perform runtime validation of external data.
* Run type checking as part of your build or continuous integration process.

# 🗁 Summary

TypeScript compilation transforms TypeScript source code into JavaScript while providing static type checking.

You have learned:

* How TypeScript compilation works.
* The role of the TypeScript compiler.
* How type checking works.
* How to generate JavaScript output.
* How to configure compiler options.
* How to use `tsconfig.json`.
* How to use watch mode.
* How source maps work.
* How declaration files are generated.
* The difference between compilation and execution.
* Why runtime validation is still necessary.

The fundamental workflow is:

```text
Write TypeScript
       |
       v
TypeScript Compiler
       |
       v
Type Checking
       |
       v
JavaScript Generation
       |
       v
JavaScript Runtime
       |
       v
Application Execution
```
