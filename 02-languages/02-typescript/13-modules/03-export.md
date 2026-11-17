# Export

Exports are a fundamental feature of TypeScript modules that allow declarations to be shared with other files.

By default, declarations inside a module are private to that module. The `export` keyword makes selected declarations accessible to other modules.

Exports help developers:

* Share reusable functions and classes.
* Expose interfaces and type aliases.
* Organize application functionality.
* Create reusable libraries and packages.
* Control module interfaces.
* Build scalable application architectures.

# 🗁 Basic Export Syntax

The `export` keyword makes a declaration available to other modules.

```typescript
export const appName = "My Application";

export function greet(name: string): string {
  return `Hello, ${name}`;
}
```

Another module can import these declarations:

```typescript
import { appName, greet } from "./app";

console.log(appName);

console.log(greet("Alex"));
```

A module can export multiple declarations.

# 🗁 Named Exports

Named exports allow you to export multiple declarations individually.

Example:

`math.ts`

```typescript
export const PI = 3.14159;

export function add(a: number, b: number): number {
  return a + b;
}

export function subtract(a: number, b: number): number {
  return a - b;
}

export class Calculator {
  multiply(a: number, b: number): number {
    return a * b;
  }
}
```

Importing:

`app.ts`

```typescript
import {
  PI,
  add,
  subtract,
  Calculator,
} from "./math";

console.log(PI);

console.log(add(10, 5));

console.log(subtract(10, 5));

const calculator = new Calculator();

console.log(calculator.multiply(10, 5));
```

Each declaration is exported using its own name.

# 🗁 Exporting Variables

You can export variables using `const`, `let`, or `var`.

```typescript
export const API_URL = "https://api.example.com";

export let requestCount = 0;

export var appVersion = "1.0.0";
```

Another module can import them:

```typescript
import {
  API_URL,
  requestCount,
  appVersion,
} from "./config";

console.log(API_URL);

console.log(requestCount);

console.log(appVersion);
```

The exported binding is shared with importing modules.

For example, if the exporting module updates an exported `let` variable, importers observe the updated binding.

Imported bindings themselves are read-only from the importing module.

```typescript
import { requestCount } from "./config";

// Error: Cannot assign to an import.
requestCount = 10;
```

# 🗁 Exporting Functions

Functions can be exported individually.

```typescript
export function calculateArea(
  width: number,
  height: number
): number {
  return width * height;
}

export function calculatePerimeter(
  width: number,
  height: number
): number {
  return 2 * (width + height);
}
```

Importing:

```typescript
import {
  calculateArea,
  calculatePerimeter,
} from "./geometry";

console.log(calculateArea(10, 5));

console.log(calculatePerimeter(10, 5));
```

Exported functions can be reused throughout an application.

# 🗁 Exporting Classes

Classes can also be exported.

```typescript
export class User {
  constructor(
    public id: number,
    public name: string
  ) {}

  getDetails(): string {
    return `${this.id}: ${this.name}`;
  }
}
```

Importing:

```typescript
import { User } from "./user";

const user = new User(101, "Alex");

console.log(user.getDetails());
```

This is useful when organizing application models, services, controllers, and other class-based functionality.

# 🗁 Exporting Interfaces

Interfaces are type declarations and can be exported for use in other modules.

```typescript
export interface User {
  id: number;
  name: string;
  email: string;
}
```

Importing:

```typescript
import type { User } from "./user";

const user: User = {
  id: 101,
  name: "Alex",
  email: "alex@example.com",
};
```

Interfaces exist only during type checking and do not generate JavaScript runtime objects.

# 🗁 Exporting Type Aliases

Type aliases can be exported in the same way as interfaces.

```typescript
export type UserRole = "admin" | "customer";

export type UserStatus = "active" | "inactive";

export type UserId = number;
```

Importing:

```typescript
import type {
  UserRole,
  UserStatus,
  UserId,
} from "./user.types";

const id: UserId = 101;

const role: UserRole = "admin";

const status: UserStatus = "active";
```

Type aliases are useful for sharing reusable type definitions.

# 🗁 Exporting Enums

Enums can be exported to make their members available in other modules.

```typescript
export enum UserRole {
  Admin = "ADMIN",
  Customer = "CUSTOMER",
  Guest = "GUEST",
}
```

Importing:

```typescript
import { UserRole } from "./user-role";

const role = UserRole.Admin;

console.log(role);
```

Unlike interfaces and type aliases, regular enums generate JavaScript runtime code.

# 🗁 Export Lists

You can export declarations after defining them.

Instead of adding `export` to every declaration, use an export list.

```typescript
const API_URL = "https://api.example.com";

function connect(): void {
  console.log("Connected");
}

class Database {
  connect(): void {
    console.log("Database connected");
  }
}

export {
  API_URL,
  connect,
  Database,
};
```

This approach separates declaration definitions from the module's public exports.

# 🗁 Renaming Exports with `as`

You can rename declarations when exporting them.

```typescript
function add(a: number, b: number): number {
  return a + b;
}

function subtract(a: number, b: number): number {
  return a - b;
}

export {
  add as sum,
  subtract as difference,
};
```

Another module imports the exported names:

```typescript
import { sum, difference } from "./math";

console.log(sum(10, 5));

console.log(difference(10, 5));
```

The original declarations remain named `add` and `subtract` inside their defining module.

# 🗁 Default Exports

A module can provide one default export.

Use the `export default` syntax.

```typescript
export default class User {
  constructor(
    public id: number,
    public name: string
  ) {}
}
```

Importing:

```typescript
import User from "./user";

const user = new User(101, "Alex");
```

Default exports are useful when a module has one primary declaration.

A module can have multiple named exports alongside one default export.

# 🗁 Default Function Exports

You can export a function as the default declaration.

```typescript
export default function greet(name: string): string {
  return `Hello, ${name}`;
}
```

Importing:

```typescript
import greet from "./greet";

console.log(greet("Alex"));
```

The importing module can choose its own local name:

```typescript
import sayHello from "./greet";

console.log(sayHello("Alex"));
```

# 🗁 Default Class Exports

A class can also be exported as the default declaration.

```typescript
export default class Calculator {
  add(a: number, b: number): number {
    return a + b;
  }
}
```

Importing:

```typescript
import Calculator from "./calculator";

const calculator = new Calculator();

console.log(calculator.add(10, 20));
```

# 🗁 Default Exports with Named Exports

You can combine default and named exports in one module.

```typescript
export const PI = 3.14159;

export function add(a: number, b: number): number {
  return a + b;
}

export default function square(value: number): number {
  return value * value;
}
```

Importing:

```typescript
import square, { PI, add } from "./math";

console.log(square(5));

console.log(add(10, 20));

console.log(PI);
```

A module can have only one default export, but it can have multiple named exports.

# 🗁 Re-Exports

Re-exports allow one module to expose declarations from another module without defining them again.

Example structure:

```text
src/
├── math.ts
├── user.ts
└── index.ts
```

`math.ts`

```typescript
export function add(a: number, b: number): number {
  return a + b;
}
```

`user.ts`

```typescript
export interface User {
  id: number;
  name: string;
}
```

`index.ts`

```typescript
export { add } from "./math";

export type { User } from "./user";
```

Now another module can import both declarations through `index.ts`:

```typescript
import { add, type User } from "./index";

const result = add(10, 20);

const user: User = {
  id: 101,
  name: "Alex",
};
```

This technique is commonly used to create barrel files.

# 🗁 Re-Exporting Everything

You can re-export all named exports from another module using `export *`.

```typescript
export * from "./math";

export * from "./user";
```

This forwards named exports but does not automatically forward the default export.

To forward a default export, explicitly re-export it:

```typescript
export { default } from "./calculator";
```

You can also rename a default export while re-exporting:

```typescript
export { default as Calculator } from "./calculator";
```

# 🗁 Selective Re-Exports

You can selectively expose declarations from another module.

```typescript
export {
  add,
  subtract as difference,
} from "./math";
```

This exports `add` and exposes `subtract` under the name `difference`.

You can also re-export types:

```typescript
export type {
  User,
  UserRole,
} from "./user.types";
```

Selective re-exports help control which declarations are part of a module's public interface.

# 🗁 Import and Re-Export Together

Sometimes you need to use a declaration inside a module and also expose it to other modules.

```typescript
import { add } from "./math";

export { add };

export function calculateTotal(
  a: number,
  b: number
): number {
  return add(a, b);
}
```

Alternatively, you can use a re-export declaration when you do not need to reference the imported binding locally:

```typescript
export { add } from "./math";
```

# 🗁 Type-Only Exports

TypeScript supports type-only exports through `export type`.

```typescript
export interface User {
  id: number;
  name: string;
}

export type UserRole = "admin" | "customer";
```

You can export them explicitly:

```typescript
export type {
  User,
  UserRole,
};
```

Type-only exports are useful for making the distinction between compile-time declarations and runtime values clear.

They are erased from emitted JavaScript.

# 🗁 Mixed Value and Type Exports

A module can expose both runtime values and type declarations.

```typescript
export interface User {
  id: number;
  name: string;
}

export const DEFAULT_ROLE = "customer";

export function createUser(id: number, name: string): User {
  return {
    id,
    name,
  };
}
```

Importing:

```typescript
import {
  createUser,
  DEFAULT_ROLE,
  type User,
} from "./user";
```

The function and constant are runtime values, while `User` is used only as a type.

# 🗁 Exporting from a Barrel File

A barrel file is a module that centralizes exports from multiple files.

Example structure:

```text
src/
└── users/
    ├── user.model.ts
    ├── user.service.ts
    ├── user.utils.ts
    └── index.ts
```

`user.model.ts`

```typescript
export interface User {
  id: number;
  name: string;
}
```

`user.service.ts`

```typescript
import type { User } from "./user.model";

export function getUser(id: number): User {
  return {
    id,
    name: "Alex",
  };
}
```

`user.utils.ts`

```typescript
import type { User } from "./user.model";

export function getDisplayName(user: User): string {
  return user.name.toUpperCase();
}
```

`index.ts`

```typescript
export type { User } from "./user.model";

export { getUser } from "./user.service";

export { getDisplayName } from "./user.utils";
```

Now other modules can import from a single location:

```typescript
import {
  getUser,
  getDisplayName,
  type User,
} from "./users";
```

This provides a clean public interface for the users feature.

# 🗁 Exporting Namespaces

TypeScript supports namespace declarations and namespace exports.

```typescript
export namespace MathUtils {
  export function add(a: number, b: number): number {
    return a + b;
  }

  export function subtract(a: number, b: number): number {
    return a - b;
  }
}
```

Importing:

```typescript
import { MathUtils } from "./math-utils";

console.log(MathUtils.add(10, 5));
```

Namespaces are an older TypeScript organization mechanism. For modern application code, ES modules are generally preferred.

# 🗁 Public Module Interfaces

A module's public interface consists of the declarations it intentionally exports.

Consider:

```typescript
const databasePassword = "internal-secret";

function connectToDatabase(): void {
  console.log("Database connected");
}

export function getUsers(): string[] {
  connectToDatabase();

  return ["Alex", "Sam"];
}
```

Other modules can access `getUsers`, but they cannot directly import `databasePassword` or `connectToDatabase`.

This allows implementation details to remain private.

A well-designed module exports only what other parts of the application need.

# 🗁 Exporting from an Application Entry Point

An application may have a central entry point that imports functionality from different modules.

```text
src/
├── config/
│   └── app.config.ts
├── users/
│   └── user.service.ts
├── products/
│   └── product.service.ts
└── main.ts
```

`main.ts`

```typescript
import { appName } from "./config/app.config";

import { getUsers } from "./users/user.service";

import { getProducts } from "./products/product.service";

console.log(appName);

console.log(getUsers());

console.log(getProducts());
```

The entry point coordinates application functionality without requiring every implementation to exist in one file.

# 🗁 Common Export Errors

**Missing export keyword**

```typescript
function greet(name: string): string {
  return `Hello, ${name}`;
}
```

Another module cannot import `greet` because it is not exported.

Correct:

```typescript
export function greet(name: string): string {
  return `Hello, ${name}`;
}
```

**Duplicate default exports**

A module cannot declare two default exports.

Incorrect:

```typescript
export default function add(): void {}

export default function subtract(): void {}
```

Use named exports for additional declarations:

```typescript
export default function add(): void {}

export function subtract(): void {}
```

**Re-exporting a nonexistent declaration**

```typescript
export { multiply } from "./math";
```

This fails if `math.ts` does not export `multiply`.

**Confusing type exports with runtime exports**

```typescript
export type User = {
  id: number;
};

export type { User };
```

This is a type-only declaration. It cannot be used as a runtime value.

# 🗁 Best Practices

* Export only declarations that other modules need.
* Use named exports for modules with multiple related declarations.
* Use default exports when a module has one clear primary declaration.
* Use `export type` for explicit type-only exports.
* Prefer selective re-exports when controlling a public API.
* Use barrel files carefully.
* Avoid exporting internal implementation details.
* Avoid circular dependencies.
* Keep module responsibilities focused.
* Use consistent naming conventions.
* Prefer ES module exports over legacy namespace patterns for modern application development.

# 🗁 Summary

Exports allow TypeScript modules to share declarations with other modules.

Key concepts:

* `export` exposes declarations.
* Named exports allow multiple declarations from one module.
* `export default` provides one primary export.
* Export lists group declarations in one statement.
* The `as` keyword renames exported declarations.
* `export type` explicitly exposes type-only declarations.
* Re-exports expose declarations from other modules.
* `export *` forwards named exports.
* Barrel files centralize module exports.
* Public module interfaces control which declarations are available externally.

Understanding exports is essential for designing reusable modules, maintaining clean application architecture, and building TypeScript libraries.
