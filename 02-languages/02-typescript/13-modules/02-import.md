# Import

Imports are a fundamental part of TypeScript modules. They allow one module to access declarations exported by another module.

Instead of duplicating code, you can import reusable functions, variables, classes, interfaces, types, and other declarations.

TypeScript supports several import patterns, including:

* Named imports.
* Default imports.
* Namespace imports.
* Renamed imports.
* Type-only imports.
* Side-effect imports.
* Re-exported imports.
* Dynamic imports.

# 🗁 Basic Import Syntax

The `import` keyword loads exported declarations from another module.

```typescript
import { add } from "./math";
```

Here:

* `import` declares an import.
* `{ add }` specifies the named declaration.
* `"./math"` identifies the module.

Example:

`math.ts`

```typescript
export function add(a: number, b: number): number {
  return a + b;
}
```

`app.ts`

```typescript
import { add } from "./math";

const result = add(10, 20);

console.log(result);
```

Output:

```text
30
```

# 🗁 Named Imports

Named imports allow you to import specific declarations using their exported names.

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

You can import multiple declarations from the same module using one import statement.

# 🗁 Importing Selected Declarations

You do not need to import every declaration exported by a module.

```typescript
import { add } from "./math";
```

Only `add` is imported into the current module.

Other exported declarations remain unused.

This keeps dependencies explicit and makes code easier to understand.

# 🗁 Renaming Imports with `as`

You can rename an imported declaration using the `as` keyword.

```typescript
import { add as sum } from "./math";

const result = sum(10, 20);
```

Here, the exported function is named `add`, but it is referenced locally as `sum`.

You can also rename multiple imports:

```typescript
import {
  add as sum,
  subtract as difference,
} from "./math";

console.log(sum(10, 5));

console.log(difference(10, 5));
```

Renaming is useful when:

* Two modules export declarations with the same name.
* An imported name conflicts with a local declaration.
* You want a more meaningful local name.
* You need to distinguish similar functions.

# 🗁 Default Imports

A default import loads the default export of a module.

`user.ts`

```typescript
export default class User {
  constructor(
    public id: number,
    public name: string
  ) {}
}
```

`app.ts`

```typescript
import User from "./user";

const user = new User(101, "Alex");

console.log(user.name);
```

Default imports do not require curly braces.

You can also choose a different local name:

```typescript
import ApplicationUser from "./user";

const user = new ApplicationUser(101, "Alex");
```

The local name does not change the original export.

# 🗁 Importing Default and Named Exports Together

A module can provide one default export and multiple named exports.

`math.ts`

```typescript
export const PI = 3.14159;

export function add(a: number, b: number): number {
  return a + b;
}

export default function square(value: number): number {
  return value * value;
}
```

You can import both types of exports in one statement:

```typescript
import square, { PI, add } from "./math";

console.log(square(5));

console.log(add(10, 20));

console.log(PI);
```

The default import appears before the named imports.

# 🗁 Namespace Imports

Namespace imports allow you to import all named exports from a module under a single namespace.

Syntax:

```typescript
import * as MathUtils from "./math";
```

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
```

`app.ts`

```typescript
import * as MathUtils from "./math";

console.log(MathUtils.PI);

console.log(MathUtils.add(10, 5));

console.log(MathUtils.subtract(10, 5));
```

Instead of importing individual declarations, you access them through the namespace object.

Namespace imports are useful when a module exports many related declarations.

# 🗁 Importing Types

TypeScript allows you to import interfaces, type aliases, and other type declarations.

`user.ts`

```typescript
export interface User {
  id: number;
  name: string;
}

export type UserRole = "admin" | "customer";
```

`app.ts`

```typescript
import { User, UserRole } from "./user";

const user: User = {
  id: 101,
  name: "Alex",
};

const role: UserRole = "admin";
```

These declarations are used by TypeScript during type checking.

They do not create JavaScript runtime values.

# 🗁 Importing Types with `import type`

The `import type` syntax explicitly imports declarations for type usage only.

```typescript
import type { User, UserRole } from "./user";
```

Example:

`user.ts`

```typescript
export interface User {
  id: number;
  name: string;
}

export type UserRole = "admin" | "customer";
```

`app.ts`

```typescript
import type { User, UserRole } from "./user";

const user: User = {
  id: 101,
  name: "Alex",
};

const role: UserRole = "admin";
```

Type-only imports are erased from the emitted JavaScript.

They are particularly useful for:

* Separating type dependencies from runtime dependencies.
* Improving code clarity.
* Avoiding unnecessary runtime imports.
* Working with strict module syntax settings.

# 🗁 Mixed Value and Type Imports

A module can export both runtime values and type declarations.

You can import both in one statement.

```typescript
import { createUser, type User } from "./user";
```

Example:

`user.ts`

```typescript
export interface User {
  id: number;
  name: string;
}

export function createUser(id: number, name: string): User {
  return { id, name };
}
```

`app.ts`

```typescript
import { createUser, type User } from "./user";

const user: User = createUser(101, "Alex");
```

Here:

* `createUser` is a runtime value.
* `User` is a type-only declaration.

The `type` modifier makes the distinction explicit.

# 🗁 Side-Effect Imports

A side-effect import loads a module without importing any specific declaration.

```typescript
import "./initialize";
```

This executes the module's top-level code.

Example:

`initialize.ts`

```typescript
console.log("Application initialized");

export const initialized = true;
```

`app.ts`

```typescript
import "./initialize";
```

When the module is loaded, its top-level code executes.

Side-effect imports are commonly used for:

* Global CSS files.
* Polyfills.
* Application initialization.
* Global configuration.
* Registering plugins.

For example, in a frontend application:

```typescript
import "./styles.css";
```

Whether a particular file extension can be imported depends on the build tool and TypeScript configuration.

# 🗁 Importing from Different Paths

You can import modules using relative paths.

```typescript
import { User } from "./user";
```

This references a module relative to the current file.

Parent directory:

```typescript
import { User } from "../models/user";
```

Nested directory:

```typescript
import { createUser } from "./services/user.service";
```

Absolute or package-based paths may also be supported depending on the project's configuration.

# 🗁 Importing from Packages

TypeScript supports importing declarations from installed packages.

For example, after installing a package:

```bash
npm install lodash
```

You can import its functions:

```typescript
import { debounce } from "lodash";
```

The module specifier does not begin with `./` or `../`, so TypeScript resolves it as a package dependency.

Package resolution depends on the package's exports, type declarations, and the configured module resolution strategy.

# 🗁 Importing from a Barrel File

A barrel file is a module that re-exports declarations from multiple modules.

It provides a centralized import location.

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
export * from "./user.model";
export * from "./user.service";
export * from "./user.utils";
```

Now other modules can import from the barrel:

```typescript
import {
  getUser,
  getDisplayName,
  type User,
} from "./users";
```

Barrel files simplify imports when working with large feature directories.

However, excessive barrel usage can introduce circular dependencies or cause unintended modules to be loaded.

# 🗁 Dynamic Imports

Dynamic imports allow you to load a module when it is needed instead of importing it at the top level.

Syntax:

```typescript
const module = await import("./math");
```

Example:

`math.ts`

```typescript
export function add(a: number, b: number): number {
  return a + b;
}
```

`app.ts`

```typescript
async function calculate(): Promise<void> {
  const math = await import("./math");

  const result = math.add(10, 20);

  console.log(result);
}

calculate();
```

Dynamic imports return a promise that resolves to a module namespace object.

They are useful for:

* Lazy loading.
* Code splitting.
* Loading optional features.
* Reducing initial application loading costs.
* Conditionally loading dependencies.

The actual loading behavior depends on the runtime or build tool.

# 🗁 Dynamic Import with Default Exports

When dynamically importing a module with a default export, access it through the `default` property.

`calculator.ts`

```typescript
export default function multiply(a: number, b: number): number {
  return a * b;
}
```

`app.ts`

```typescript
async function calculate(): Promise<void> {
  const module = await import("./calculator");

  const result = module.default(10, 5);

  console.log(result);
}
```

The imported module namespace exposes its default export through `module.default`.

# 🗁 Importing JSON Files

TypeScript can support importing JSON modules when the appropriate compiler option is enabled.

Example configuration:

`tsconfig.json`

```json
{
  "compilerOptions": {
    "resolveJsonModule": true
  }
}
```

Example JSON file:

`config.json`

```json
{
  "appName": "My Application",
  "version": "1.0.0",
  "debug": true
}
```

Importing:

```typescript
import config from "./config.json";

console.log(config.appName);

console.log(config.version);
```

The runtime or bundler must also support the corresponding JSON module loading behavior.

# 🗁 Import Aliases

Import aliases allow you to use alternative paths instead of long relative paths.

For example:

```typescript
import { User } from "../../../models/user";
```

This can become difficult to maintain in deeply nested applications.

You can configure a path alias in `tsconfig.json`:

```json
{
  "compilerOptions": {
    "baseUrl": ".",
    "paths": {
      "@/*": ["src/*"]
    }
  }
}
```

Now you can write:

```typescript
import { User } from "@/models/user";
```

Important: TypeScript's `paths` option primarily informs module resolution during development and type checking. It does not automatically rewrite import paths in emitted JavaScript.

Your runtime or bundler must also understand the alias.

# 🗁 Importing the Same Module Multiple Times

Multiple imports from the same module can be consolidated.

Instead of:

```typescript
import { add } from "./math";

import { subtract } from "./math";

import { PI } from "./math";
```

Use:

```typescript
import {
  add,
  subtract,
  PI,
} from "./math";
```

This improves readability and keeps dependencies organized.

# 🗁 Import Ordering

Consistent import ordering makes large projects easier to maintain.

A common convention is:

```typescript
// External packages
import React from "react";
import { z } from "zod";

// Internal application modules
import { UserService } from "@/services/user.service";
import { User } from "@/models/user";

// Relative imports
import { formatDate } from "./date.utils";

// Type-only imports
import type { UserRole } from "@/models/user.types";
```

The exact ordering convention depends on the project's style guide and linting configuration.

# 🗁 Common Import Errors

**Module not found**

```typescript
import { User } from "./models/user";
```

This error can occur when the path is incorrect or the module cannot be resolved.

Check:

* File location.
* Filename spelling.
* File extension requirements.
* Case sensitivity.
* `moduleResolution` configuration.

**Declaration is not exported**

```typescript
import { User } from "./user";
```

This fails if the `user.ts` module does not export `User`.

**Incorrect import style**

```typescript
import { User } from "./user";
```

If `User` is a default export, use:

```typescript
import User from "./user";
```

**Using a type as a runtime value**

```typescript
import type { User } from "./user";

console.log(User);
```

This is invalid because `User` is imported only for type checking.

**Runtime alias resolution failure**

An import such as:

```typescript
import { User } from "@/models/user";
```

may type-check successfully but fail at runtime if the bundler or runtime does not support the configured alias.

# 🗁 Best Practices

* Import only the declarations you need.
* Use named imports for clarity.
* Use `import type` for type-only dependencies.
* Use namespace imports when grouping many related declarations improves readability.
* Keep import paths consistent.
* Use aliases for deeply nested application structures when supported by the runtime or bundler.
* Avoid unnecessary side-effect imports.
* Avoid excessive barrel files.
* Use dynamic imports for appropriate lazy-loading scenarios.
* Keep external package imports separate from internal application imports.
* Remove unused imports.
* Follow the project's module resolution strategy.

# 🗁 Summary

TypeScript provides flexible import mechanisms for sharing declarations between modules.

Key concepts:

* Named imports load specific exported declarations.
* Default imports load a module's default export.
* Namespace imports group named exports under one name.
* The `as` keyword renames imported declarations.
* `import type` loads declarations for type checking only.
* Side-effect imports execute a module without importing declarations.
* Dynamic imports load modules asynchronously.
* Barrel files centralize exports from multiple modules.
* Path aliases simplify long import paths.
* Module resolution determines how imported modules are located.

Understanding imports is essential for building modular, reusable, and maintainable TypeScript applications.
