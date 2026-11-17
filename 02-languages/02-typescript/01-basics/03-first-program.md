# Your First TypeScript Program

Writing your first TypeScript program is a practical way to understand how TypeScript code is written, checked, compiled, and executed.

TypeScript extends JavaScript with static type checking. You write TypeScript source code, the TypeScript compiler checks it and can transform it into JavaScript, and a JavaScript runtime executes the resulting code.

# 🗁 Create a Project Directory

Create a directory for your first TypeScript project:

```bash
mkdir typescript-first-program
cd typescript-first-program
```

Initialize an npm project:

```bash
npm init -y
```

Install TypeScript as a development dependency:

```bash
npm install --save-dev typescript
```

# 🗁 Create a TypeScript File

Create a file named `index.ts`:

```typescript
const message: string = "Hello, TypeScript!";

console.log(message);
```

Explanation:

* `const` declares a variable that cannot be reassigned.
* `message` is the variable name.
* `: string` declares the variable's type.
* `"Hello, TypeScript!"` is the assigned string value.
* `console.log()` prints the message to the console.

The `.ts` extension identifies the file as TypeScript source code.

# 🗁 Compile TypeScript into JavaScript

Use the TypeScript compiler installed in the project:

```bash
npx tsc index.ts
```

The compiler generates an `index.js` file:

```javascript
var message = "Hello, TypeScript!";
console.log(message);
```

The exact generated JavaScript can vary depending on the TypeScript compiler version and configuration.

The important point is that TypeScript-specific syntax, such as the `: string` annotation, is removed during compilation.

# 🗁 Run the JavaScript Program

Node.js executes the generated JavaScript:

```bash
node index.js
```

Expected output:

```text
Hello, TypeScript!
```

The complete workflow is:

```text
index.ts
   |
   v
TypeScript Compiler (tsc)
   |
   v
index.js
   |
   v
Node.js
   |
   v
Hello, TypeScript!
```

# 🗁 Understand Type Checking

One of TypeScript's main advantages is detecting type-related mistakes before runtime.

Modify `index.ts`:

```typescript
let age: number = 25;

age = "twenty-five";

console.log(age);
```

Compile the file:

```bash
npx tsc index.ts
```

TypeScript reports an error similar to:

```text
error TS2322: Type 'string' is not assignable to type 'number'.
```

The compiler detects that `age` was declared as a number but later assigned a string.

Correct the program:

```typescript
let age: number = 25;

age = 30;

console.log(age);
```

Now the type assignment is valid.

# 🗁 TypeScript Can Emit JavaScript Despite Errors

By default, TypeScript may still generate JavaScript output even when type errors exist.

For example:

```typescript
let age: number = 25;

age = "twenty-five";
```

The compiler reports the type error, but it may still produce `index.js`.

To prevent JavaScript emission when errors exist, use:

```bash
npx tsc index.ts --noEmitOnError
```

This is useful in build processes where invalid TypeScript should not produce output files.

# 🗁 Create a Program with a Function

TypeScript supports functions with explicit parameter and return types.

```typescript
function add(a: number, b: number): number {
    return a + b;
}

const result: number = add(10, 20);

console.log(result);
```

Expected output:

```text
30
```

Explanation:

* `a: number` declares the first parameter as a number.
* `b: number` declares the second parameter as a number.
* `: number` after the parameter list declares the return type.
* `add(10, 20)` calls the function with two numeric arguments.

Passing an incompatible argument produces a type error:

```typescript
add(10, "20");
```

# 🗁 Create a Program with an Interface

Interfaces describe the expected structure of objects.

```typescript
interface User {
    id: number;
    name: string;
    email: string;
}

const user: User = {
    id: 1,
    name: "John",
    email: "john@example.com"
};

console.log(user.name);
console.log(user.email);
```

Expected output:

```text
John
john@example.com
```

The `User` interface requires an object to contain:

* `id` with a number value.
* `name` with a string value.
* `email` with a string value.

If a required property is missing or has an incompatible type, TypeScript reports an error.

# 🗁 Run TypeScript Without a Separate Compilation Step

During development, tools such as `tsx` can execute TypeScript files directly without requiring you to manually generate JavaScript first.

Install `tsx`:

```bash
npm install --save-dev tsx
```

Run the program:

```bash
npx tsx index.ts
```

This is convenient for development and experimentation.

However, `tsx` does not replace TypeScript's static type checker. Run the compiler separately when you want to check types:

```bash
npx tsc --noEmit
```

# 🗁 Add npm Scripts

You can define reusable commands in `package.json`.

```json
{
  "scripts": {
    "build": "tsc index.ts",
    "start": "node index.js",
    "dev": "tsx index.ts",
    "typecheck": "tsc --noEmit"
  }
}
```

Run the commands using npm:

```bash
npm run build
npm start
npm run dev
npm run typecheck
```

These scripts make common development tasks easier to execute.

# 🗁 Complete Project Structure

After following the examples, your project can look like this:

```text
typescript-first-program/
│
├── node_modules/
│
├── index.ts
├── index.js
├── package.json
├── package-lock.json
└── .gitignore
```

The `.gitignore` file can exclude generated and dependency directories:

```gitignore
node_modules/
dist/
```

For larger projects, generated JavaScript is commonly placed in a separate output directory such as `dist/`.

# 🗁 Best Practices

* Use `.ts` extensions for TypeScript source files.
* Prefer `const` when a variable does not need reassignment.
* Use explicit types when they improve clarity.
* Let TypeScript infer straightforward types when explicit annotations are unnecessary.
* Avoid `any` unless there is a specific reason to bypass type checking.
* Use interfaces or type aliases to describe object structures.
* Run type checking before building or deploying applications.
* Keep generated JavaScript separate from source files in larger projects.
* Use `tsx` or similar tools for convenient development execution.
* Remember that TypeScript types are generally removed during compilation and do not provide runtime validation.

# 🗁 Summary

You have learned how to:

* Create a TypeScript project.
* Write your first `.ts` file.
* Declare variables with types.
* Compile TypeScript into JavaScript.
* Execute JavaScript using Node.js.
* Detect type errors with the TypeScript compiler.
* Write typed functions and interfaces.
* Execute TypeScript using `tsx`.
* Configure basic npm scripts.

TypeScript development generally follows this workflow:

```text
Write TypeScript
       |
       v
Check Types
       |
       v
Compile to JavaScript
       |
       v
Run JavaScript
       |
       v
Application Output
```
