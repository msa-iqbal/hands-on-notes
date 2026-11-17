# .d.ts Files in TypeScript

A `.d.ts` file is a TypeScript declaration file that describes the types and structure of JavaScript code without providing its implementation.

The extension stands for declaration TypeScript.

These files allow TypeScript to understand JavaScript modules, external libraries, global variables, and APIs that do not contain native TypeScript type information.

For example:

```typescript
// calculator.d.ts

export function add(a: number, b: number): number;
```

This declaration tells TypeScript that:

* A function named `add` exists.
* It accepts two numbers.
* It returns a number.

The actual implementation must exist separately.

# 🗁 Why .d.ts Files Are Important

Declaration files are useful for:

* Providing types for JavaScript libraries.
* Describing external APIs.
* Defining global variables.
* Creating reusable type definitions.
* Publishing TypeScript-compatible npm packages.
* Supporting JavaScript and TypeScript interoperability.
* Improving IntelliSense and editor support.

Without declaration files, TypeScript may not know the intended structure of a JavaScript module.

# 🗁 Difference Between .ts and .d.ts

| Feature | `.ts` | `.d.ts` |
| --- | --- | --- |
| Purpose | Implementation and types | Type declarations |
| Executable code | Can contain implementation | Cannot contain runtime implementation |
| Type annotations | Supported | Supported |
| Function bodies | Supported | Not permitted for ordinary function declarations |
| Compilation | Can generate JavaScript | Generates no JavaScript implementation |
| Typical usage | Application and library source | Describing APIs |

Example TypeScript implementation:

```typescript
// math.ts

export function add(a: number, b: number): number {
  return a + b;
}
```

Equivalent declaration:

```typescript
// math.d.ts

export function add(a: number, b: number): number;
```

The declaration describes the function but does not implement it.

# 🗁 Creating a .d.ts File

Consider an existing JavaScript module:

```javascript
// user.js

export function getUser(id) {
  return {
    id,
    name: "Alice",
  };
}
```

Create a declaration file:

```typescript
// user.d.ts

export interface User {
  id: number;
  name: string;
}

export function getUser(id: number): User;
```

Now TypeScript can understand the JavaScript module's API.

Usage:

```typescript
import { getUser } from "./user.js";

const user = getUser(1);

console.log(user.name);
```

TypeScript knows that `user` has the `User` structure.

# 🗁 Declaring Functions

Function declarations describe parameters and return types.

```typescript
// utilities.d.ts

export function greet(name: string): string;

export function calculate(
  price: number,
  quantity: number
): number;

export function logMessage(message: string): void;
```

Function declarations in `.d.ts` files end with semicolons because they have no implementation bodies.

# 🗁 Declaring Variables

Use `declare` to describe variables that are provided by an external runtime.

```typescript
// environment.d.ts

declare const APP_VERSION: string;

declare const API_URL: string;

declare let currentUserId: number;
```

These declarations inform TypeScript that the variables exist.

They do not create the variables in JavaScript.

# 🗁 Declaring Interfaces and Types

Declaration files can contain reusable interfaces and type aliases.

```typescript
// models.d.ts

export interface User {
  id: number;
  name: string;
  email?: string;
}

export type UserRole = "admin" | "editor" | "viewer";

export type UserStatus = "active" | "inactive";
```

Import them into TypeScript files:

```typescript
import type {
  User,
  UserRole,
  UserStatus,
} from "./models.js";

const user: User = {
  id: 1,
  name: "Alice",
};

const role: UserRole = "admin";

const status: UserStatus = "active";
```

# 🗁 Declaring Classes

A `.d.ts` file can describe classes implemented in JavaScript.

```typescript
// user-service.d.ts

export interface User {
  id: number;
  name: string;
}

export declare class UserService {
  constructor(apiUrl: string);

  getUser(id: number): Promise<User>;

  deleteUser(id: number): Promise<void>;
}
```

The declaration describes the constructor and public methods.

The actual class implementation must exist separately.

# 🗁 Declaring Modules

Use `declare module` to describe a JavaScript module that does not provide its own TypeScript declarations.

```typescript
// custom-library.d.ts

declare module "custom-library" {
  export function initialize(): void;

  export function getVersion(): string;

  export interface Configuration {
    debug: boolean;
    environment: string;
  }
}
```

TypeScript can now understand imports from that module.

```typescript
import {
  initialize,
  getVersion,
  type Configuration,
} from "custom-library";

const config: Configuration = {
  debug: true,
  environment: "development",
};

initialize();

console.log(getVersion());
```

# 🗁 Global Declaration Files

A `.d.ts` file can describe global APIs available throughout an application.

```typescript
// globals.d.ts

declare const APP_NAME: string;

declare function trackEvent(
  eventName: string
): void;

interface ApplicationConfig {
  environment: "development" | "production";
}
```

When the declaration file is included in the TypeScript project, these declarations become available globally.

Use global declarations carefully because they can introduce naming conflicts.

# 🗁 Global Augmentation

You can extend existing global interfaces using declaration merging.

For example, extending the browser `Window` interface:

```typescript
// window.d.ts

export {};

declare global {
  interface Window {
    APP_VERSION: string;
    isProduction: boolean;
  }
}
```

Usage:

```typescript
console.log(window.APP_VERSION);

if (window.isProduction) {
  console.log("Production environment");
}
```

The `export {}` makes the declaration file a module, allowing the `declare global` block to augment the global scope.

The actual properties must still be initialized by the application at runtime.

# 🗁 Organizing .d.ts Files

A project can maintain custom declaration files in a dedicated directory.

```text
my-project/
├── src/
│   ├── app.ts
│   └── services/
│       └── user-service.ts
├── types/
│   ├── globals.d.ts
│   ├── environment.d.ts
│   ├── custom-library.d.ts
│   └── window.d.ts
├── package.json
└── tsconfig.json
```

Example configuration:

```json
{
  "compilerOptions": {
    "strict": true,
    "noEmit": true
  },
  "include": [
    "src/**/*",
    "types/**/*.d.ts"
  ]
}
```

TypeScript loads declaration files included in the project.

# 🗁 Generating .d.ts Files

TypeScript can automatically generate declaration files from TypeScript source code.

Configure `tsconfig.json`:

```json
{
  "compilerOptions": {
    "declaration": true,
    "declarationMap": true,
    "outDir": "./dist"
  },
  "include": [
    "src/**/*.ts"
  ]
}
```

Compile the project:

```bash
npx tsc
```

Example output:

```text
dist/
├── index.js
├── index.d.ts
├── index.js.map
└── index.d.ts.map
```

Important compiler options:

| Option | Description |
| --- | --- |
| `declaration` | Generates `.d.ts` files |
| `declarationMap` | Generates source maps for declaration files |
| `emitDeclarationOnly` | Generates declarations without JavaScript output |
| `outDir` | Specifies the output directory |
| `declarationDir` | Specifies the output directory for declaration files |

# 🗁 Declaration Files for npm Packages

TypeScript libraries can publish declaration files so consumers receive type information automatically.

Example package structure:

```text
my-library/
├── src/
│   └── index.ts
├── dist/
│   ├── index.js
│   └── index.d.ts
└── package.json
```

Example `package.json`:

```json
{
  "name": "my-library",
  "version": "1.0.0",
  "main": "./dist/index.js",
  "types": "./dist/index.d.ts"
}
```

The `types` field identifies the package's main declaration entry point.

Modern packages can also use the `exports` field to define entry points and their corresponding type declarations.

# 🗁 Best Practices

* Keep declaration files consistent with actual implementations.
* Use meaningful names for declaration files.
* Organize custom declarations in a dedicated directory.
* Avoid unnecessary global declarations.
* Prefer module-based declarations for reusable libraries.
* Generate declarations automatically when publishing TypeScript packages.
* Use `declarationMap` when source navigation is useful.
* Test public declarations before releasing a package.
* Avoid duplicating types already provided by dependencies.

# 🗁 Common Mistakes

**Adding implementation code to declaration files**

A `.d.ts` file describes APIs and should not contain ordinary runtime implementations.

**Assuming declarations create runtime variables**

A `declare const` statement provides type information only. The runtime must supply the actual variable.

**Mismatching declarations and implementations**

If a declaration says a function returns a string but the implementation returns a number, TypeScript may accept incorrect usage and runtime behavior can break.

**Forgetting to include custom declaration files**

A declaration file outside the configured project scope may not be loaded.

**Confusing module declarations with global declarations**

`declare module` describes a module, while global declarations describe APIs available without importing a module.

# 🗁 Key Takeaways

* `.d.ts` files describe TypeScript types and APIs.
* They do not contain runtime implementations.
* They allow TypeScript to understand JavaScript modules and external libraries.
* `declare` describes variables and APIs provided elsewhere.
* `declare module` describes external modules.
* Global declarations describe application-wide APIs.
* TypeScript can generate declaration files automatically.
* The `types` field in `package.json` identifies a package's declaration entry point.
* Accurate declaration files are essential for reliable JavaScript interoperability.
