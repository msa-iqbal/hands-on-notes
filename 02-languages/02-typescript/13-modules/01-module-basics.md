# Module Basics

Modules are a fundamental feature of TypeScript that allow developers to organize code into separate, reusable, and maintainable files.

A module is a file that has its own scope and explicitly shares functionality through `export` and `import`.

Modules help organize large applications by separating related functionality into independent files.

They are commonly used for:

* Organizing application code.
* Reusing functions, classes, interfaces, and types.
* Separating responsibilities.
* Managing dependencies.
* Avoiding global scope pollution.
* Building scalable application architectures.

# 🗁 What Is a Module?

A module is a file that contains its own declarations and explicitly controls which declarations are available to other files.

Consider two TypeScript files:

```text
src/
├── math.ts
└── app.ts
```

The `math.ts` file:

```typescript
export function add(a: number, b: number): number {
  return a + b;
}
```

The `app.ts` file:

```typescript
import { add } from "./math";

const result = add(10, 20);

console.log(result);
```

Here:

* `math.ts` defines and exports the `add()` function.
* `app.ts` imports the function.
* The imported function can be used in another file.

This makes the `add()` function reusable without duplicating its implementation.

# 🗁 Why Use Modules?

Without modules, applications can become difficult to maintain as the codebase grows.

For example, imagine placing every function, class, and variable inside a single file:

```typescript
function add(a: number, b: number): number {
  return a + b;
}

function subtract(a: number, b: number): number {
  return a - b;
}

function multiply(a: number, b: number): number {
  return a * b;
}

function divide(a: number, b: number): number {
  return a / b;
}
```

This may work for a small program, but larger applications benefit from separating related functionality.

A modular structure:

```text
src/
├── math/
│   ├── addition.ts
│   ├── subtraction.ts
│   ├── multiplication.ts
│   └── division.ts
│
├── users/
│   ├── user.model.ts
│   └── user.service.ts
│
└── app.ts
```

Each file has a focused responsibility.

# 🗁 Module Scope

Every module has its own scope.

Declarations inside a module are not automatically available in other modules.

Example:

`math.ts`

```typescript
const PI = 3.14159;

function calculateArea(radius: number): number {
  return PI * radius * radius;
}
```

`app.ts`

```typescript
console.log(PI);

calculateArea(5);
```

The second file cannot access `PI` or `calculateArea` because they have not been exported.

To make them available:

```typescript
export const PI = 3.14159;

export function calculateArea(radius: number): number {
  return PI * radius * radius;
}
```

The declarations are now accessible through module imports.

# 🗁 Exporting from a Module

The `export` keyword makes declarations available to other modules.

You can export:

* Variables.
* Functions.
* Classes.
* Interfaces.
* Type aliases.
* Enums.
* Other supported declarations.

Example:

```typescript
export const appName = "My Application";

export function greet(name: string): string {
  return `Hello, ${name}`;
}

export interface User {
  id: number;
  name: string;
}

export type UserRole = "admin" | "customer";
```

Other files can import these declarations.

# 🗁 Importing from a Module

The `import` keyword allows one module to use declarations exported by another module.

```typescript
import {
  appName,
  greet,
  User,
  UserRole,
} from "./app";
```

You can now use the imported declarations:

```typescript
console.log(appName);

console.log(greet("Alex"));

const user: User = {
  id: 101,
  name: "Alex",
};

const role: UserRole = "admin";
```

The import path identifies the module from which the declarations are loaded.

# 🗁 Named Exports

Named exports allow a module to export multiple declarations using their individual names.

Example:

`math.ts`

```typescript
export function add(a: number, b: number): number {
  return a + b;
}

export function subtract(a: number, b: number): number {
  return a - b;
}

export const PI = 3.14159;
```

Importing:

`app.ts`

```typescript
import { add, subtract, PI } from "./math";

console.log(add(10, 5));

console.log(subtract(10, 5));

console.log(PI);
```

Named imports normally use the same names as their corresponding exports.

# 🗁 Default Exports

A module can also provide one default export.

Example:

`user.ts`

```typescript
export default class User {
  constructor(
    public id: number,
    public name: string
  ) {}
}
```

Importing:

`app.ts`

```typescript
import User from "./user";

const user = new User(101, "Alex");

console.log(user.name);
```

Unlike named imports, a default import does not need to use the original declaration name.

A module can have one default export and multiple named exports.

# 🗁 Named Export vs Default Export

| Feature | Named Export | Default Export |
| --- | --- | --- |
| Number per module | Multiple | One |
| Import syntax | Named braces | No braces |
| Import name | Must match unless aliased | Can choose another name |
| Export keyword | `export` | `export default` |
| Typical use | Multiple related declarations | Primary module declaration |

Example:

```typescript
// math.ts

export const PI = 3.14159;

export function add(a: number, b: number): number {
  return a + b;
}

export default function calculateSquare(value: number): number {
  return value * value;
}
```

Importing:

```typescript
import calculateSquare, { PI, add } from "./math";

console.log(calculateSquare(5));

console.log(add(10, 20));

console.log(PI);
```

# 🗁 Module Imports and File Extensions

The module specifier identifies the module being imported.

```typescript
import { add } from "./math";
```

Depending on the module system, runtime environment, and compiler configuration, TypeScript may resolve this import to a source file such as:

```text
math.ts
```

For native Node.js ESM projects using TypeScript's Node-aware module resolution, source imports commonly specify the emitted JavaScript extension:

```typescript
import { add } from "./math.js";
```

TypeScript can resolve the `.js` specifier to the corresponding `.ts` source file during compilation.

The correct extension strategy depends on the project's module system and runtime.

# 🗁 Module Resolution

Module resolution is the process TypeScript uses to locate the file or package referenced by an import.

For example:

```typescript
import { User } from "./models/user";
```

TypeScript needs to locate the corresponding module.

The resolution process depends on compiler settings such as:

* `module`.
* `moduleResolution`.
* `baseUrl`.
* `paths`.

Common module resolution strategies include:

* `bundler`.
* `node16`.
* `nodenext`.
* `node10` for legacy projects.

Modern projects should generally use a resolution strategy appropriate to their runtime or bundler.

# 🗁 TypeScript Modules vs JavaScript Modules

TypeScript modules use the same fundamental module concepts as JavaScript.

The major difference is that TypeScript additionally supports type-related declarations.

Example:

```typescript
// user.ts

export interface User {
  id: number;
  name: string;
}

export function createUser(
  id: number,
  name: string
): User {
  return { id, name };
}
```

Importing:

```typescript
import { createUser, User } from "./user";

const user: User = createUser(101, "Alex");
```

The `User` interface is available during type checking but does not produce a JavaScript runtime object.

The `createUser` function is a runtime value and is included in the emitted JavaScript.

# 🗁 What Makes a File a Module?

A TypeScript file is treated as a module when it contains a top-level `import` or `export` declaration.

Example:

```typescript
// math.ts

export function add(a: number, b: number): number {
  return a + b;
}
```

This file is a module.

A file without top-level imports or exports is generally treated as a script unless compiler settings such as `moduleDetection` classify it differently.

Script files can share declarations through global scope, while modules have their own scope.

You can explicitly make a file a module with:

```typescript
export {};
```

This does not export a useful declaration, but it establishes module scope.

# 🗁 Module Scope vs Global Scope

Consider two files.

`first.ts`

```typescript
const appName = "Application";
```

`second.ts`

```typescript
const appName = "Dashboard";
```

If both files are treated as scripts, the declarations can conflict because they share global scope.

Adding an export makes the first file a module:

`first.ts`

```typescript
export const appName = "Application";
```

Now its declaration belongs to the module's scope rather than the global scope.

# 🗁 Organizing Modules by Responsibility

A practical application should organize modules according to their responsibilities.

Example:

```text
src/
├── config/
│   └── app.config.ts
│
├── models/
│   └── user.model.ts
│
├── services/
│   └── user.service.ts
│
├── utils/
│   └── validation.ts
│
└── app.ts
```

Example responsibilities:

| Module | Responsibility |
| --- | --- |
| `app.config.ts` | Application configuration |
| `user.model.ts` | User-related types and models |
| `user.service.ts` | User-related business logic |
| `validation.ts` | Reusable validation functions |
| `app.ts` | Application entry point |

This structure makes code easier to locate, reuse, test, and maintain.

# 🗁 Practical Example: A Simple User Module

Consider a small application with separate user-related modules.

Project structure:

```text
src/
├── users/
│   ├── user.types.ts
│   ├── user.service.ts
│   └── user.utils.ts
│
└── app.ts
```

`user.types.ts`

```typescript
export interface User {
  id: number;
  name: string;
  email: string;
}

export type UserRole = "admin" | "customer";
```

`user.service.ts`

```typescript
import type { User } from "./user.types";

export function getUser(id: number): User {
  return {
    id,
    name: "Alex",
    email: "alex@example.com",
  };
}
```

`user.utils.ts`

```typescript
import type { User } from "./user.types";

export function getDisplayName(user: User): string {
  return user.name.toUpperCase();
}
```

`app.ts`

```typescript
import { getUser } from "./users/user.service";

import { getDisplayName } from "./users/user.utils";

const user = getUser(101);

console.log(getDisplayName(user));
```

This example demonstrates how different modules can collaborate while keeping their responsibilities separate.

# 🗁 Common Module Errors

**Missing export**

```typescript
// math.ts

function add(a: number, b: number): number {
  return a + b;
}
```

Attempting to import `add` from another module will fail because it is not exported.

Correct implementation:

```typescript
export function add(a: number, b: number): number {
  return a + b;
}
```

**Incorrect named import**

```typescript
import { calculate } from "./math";
```

If the module exports `add` instead of `calculate`, the import will fail.

**Incorrect default import**

```typescript
import { User } from "./user";
```

If `user.ts` provides `User` as its default export, the correct syntax is:

```typescript
import User from "./user";
```

**Incorrect module path**

```typescript
import { User } from "./models/user";
```

The import fails if the referenced module cannot be resolved.

Check the file location, filename, extension requirements, and module resolution configuration.

# 🗁 Best Practices

* Keep modules focused on a clear responsibility.
* Use named exports for modules that expose multiple related declarations.
* Use default exports when a module has one clear primary declaration.
* Avoid unnecessary global declarations.
* Keep import paths consistent throughout the project.
* Prefer explicit type-only imports when importing declarations used only as types.
* Avoid circular dependencies where possible.
* Organize large applications by feature or domain.
* Export only declarations that other modules actually need.
* Configure module resolution according to the runtime or bundler.

# 🗁 Summary

TypeScript modules provide a structured way to organize and reuse application code.

Key concepts:

* A module is a file with its own scope.
* `export` makes declarations available to other modules.
* `import` allows modules to reuse exported declarations.
* Named exports support multiple exported declarations.
* Default exports provide one primary export per module.
* Module resolution determines how imported files are located.
* TypeScript modules support both runtime values and compile-time types.
* Modules prevent accidental global scope conflicts.
* Well-organized modules improve maintainability and scalability.

Understanding module basics is essential before working with advanced imports, exports, module resolution, and declaration merging.
