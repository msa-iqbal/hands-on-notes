# Declaration Files in TypeScript

Declaration files are TypeScript files with the `.d.ts` extension that describe the types and public APIs of JavaScript code.

They provide type information without containing the actual runtime implementation.

Declaration files tell TypeScript:

* Which functions are available.
* What parameters functions accept.
* What types functions return.
* Which classes and interfaces exist.
* What properties objects contain.
* Which modules can be imported.
* Which global variables and APIs are available.

They are especially important when using JavaScript libraries that do not include their own TypeScript definitions.

# 🗁 Why Declaration Files Matter

JavaScript does not have TypeScript's static type system.

Consider a JavaScript function:

```javascript
// calculator.js

export function add(a, b) {
  return a + b;
}
```

TypeScript may not know the intended types of `a` and `b`.

A declaration file can describe the function:

```typescript
// calculator.d.ts

export function add(a: number, b: number): number;
```

Now TypeScript understands how the JavaScript function should be used.

```typescript
import { add } from "./calculator.js";

const result = add(10, 20);

// TypeScript reports an error.
add("10", 20);
```

The declaration provides compile-time information without changing the JavaScript implementation.

# 🗁 Understanding the .d.ts Extension

The `.d.ts` extension identifies a TypeScript declaration file.

Common TypeScript file types:

| Extension | Purpose |
| --- | --- |
| `.ts` | TypeScript implementation |
| `.tsx` | TypeScript with JSX |
| `.js` | JavaScript implementation |
| `.jsx` | JavaScript with JSX |
| `.d.ts` | TypeScript declarations |

A declaration file describes types and APIs. It does not provide executable implementations.

For example:

```typescript
export function greet(name: string): string;
```

This is a declaration, not a function implementation.

# 🗁 Creating a Declaration File

Consider a JavaScript module:

```javascript
// math.js

export function add(a, b) {
  return a + b;
}

export function subtract(a, b) {
  return a - b;
}
```

Create a corresponding declaration file:

```typescript
// math.d.ts

export function add(a: number, b: number): number;

export function subtract(
  a: number,
  b: number
): number;
```

TypeScript can now understand the module's public API.

The declaration file and JavaScript implementation must remain consistent.

# 🗁 Declaring Variables

Use `declare` to describe variables that exist at runtime but are implemented elsewhere.

```typescript
// globals.d.ts

declare const APP_VERSION: string;

declare let currentUserId: number;
```

These declarations tell TypeScript that the variables exist.

They do not create the variables in JavaScript.

For example, the runtime environment must provide `APP_VERSION` and `currentUserId` separately.

# 🗁 Declaring Functions

You can declare functions without providing their implementations.

```typescript
// utilities.d.ts

export function formatDate(date: Date): string;

export function calculateTotal(
  price: number,
  quantity: number
): number;
```

Function declarations describe the expected parameter and return types.

# 🗁 Declaring Objects

Use interfaces and type aliases to describe object structures.

```typescript
// user.d.ts

export interface User {
  id: number;
  name: string;
  email?: string;
}

export type UserRole = "admin" | "user";
```

These types can be imported into other TypeScript files.

```typescript
import type { User, UserRole } from "./user.js";

const user: User = {
  id: 1,
  name: "Alice",
};

const role: UserRole = "admin";
```

# 🗁 Declaring Classes

Declaration files can describe classes implemented in JavaScript.

```typescript
// user-service.d.ts

export declare class UserService {
  constructor(apiUrl: string);

  getUser(id: number): Promise<User>;

  deleteUser(id: number): Promise<void>;
}

export interface User {
  id: number;
  name: string;
}
```

The declaration describes the class's constructor and public methods.

The actual implementation must exist in JavaScript or another runtime-compatible module.

# 🗁 Declaring Modules

Use module declarations to describe external JavaScript modules.

```typescript
// custom-library.d.ts

declare module "custom-library" {
  export function initialize(): void;

  export function getVersion(): string;
}
```

TypeScript can now understand imports from `"custom-library"`.

```typescript
import {
  initialize,
  getVersion,
} from "custom-library";

initialize();

console.log(getVersion());
```

This is useful when a JavaScript dependency does not provide TypeScript declarations.

# 🗁 Declaring Third-Party JavaScript Libraries

Some JavaScript libraries do not include their own TypeScript declarations.

For popular libraries, declarations are often available through DefinitelyTyped.

Install the corresponding package:

```bash
npm install -D @types/lodash
```

The `@types/lodash` package provides declaration files for Lodash.

These declarations allow TypeScript to understand the library's API.

Not every JavaScript package needs a separate `@types` package. Many packages already include their own declarations.

# 🗁 Declaration Files in Project Structure

A project may organize declaration files in a dedicated directory.

```text
my-project/
├── src/
│   ├── app.ts
│   └── services/
│       └── user-service.ts
├── types/
│   ├── global.d.ts
│   └── custom-library.d.ts
├── package.json
└── tsconfig.json
```

You can include declaration files through the TypeScript configuration.

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

TypeScript automatically loads included declaration files during type checking.

# 🗁 Global Declaration Files

Declaration files can describe global variables, functions, and interfaces.

```typescript
// global.d.ts

declare const API_URL: string;

declare function trackEvent(
  eventName: string
): void;

interface ApplicationConfig {
  environment: "development" | "production";
}
```

These declarations are available throughout the TypeScript project when the file is included.

Global declarations should be used carefully to avoid naming conflicts.

# 🗁 Declaration Files and JavaScript Implementations

A declaration file describes an implementation; it does not replace it.

Example:

```text
src/
├── calculator.js
└── calculator.d.ts
```

JavaScript implementation:

```javascript
// calculator.js

export function multiply(a, b) {
  return a * b;
}
```

Declaration:

```typescript
// calculator.d.ts

export function multiply(
  a: number,
  b: number
): number;
```

The declaration describes the expected API of the JavaScript module.

TypeScript uses this information when checking imports.

# 🗁 Generating Declaration Files Automatically

TypeScript can generate declaration files from TypeScript source code.

Enable the `declaration` option:

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

Important options:

| Option | Description |
| --- | --- |
| `declaration` | Generates `.d.ts` files |
| `declarationMap` | Generates source maps for declaration files |
| `emitDeclarationOnly` | Generates declarations without JavaScript output |
| `outDir` | Specifies the output directory |

# 🗁 Publishing Declaration Files

When publishing a TypeScript library, declaration files allow consumers to use the package with type checking and editor support.

Example `package.json`:

```json
{
  "name": "my-library",
  "version": "1.0.0",
  "main": "./dist/index.js",
  "types": "./dist/index.d.ts"
}
```

The `types` field identifies the package's main TypeScript declaration entry point.

The `typings` field is also recognized as an alias.

For modern packages, the `exports` field can provide more precise entry-point and type mappings.

# 🗁 Best Practices

* Keep declaration files consistent with their implementations.
* Expose only the intended public API.
* Use interfaces and type aliases to describe complex structures.
* Prefer package-provided declarations when available.
* Organize custom declarations in a dedicated directory when appropriate.
* Avoid unnecessary global declarations.
* Generate declaration files automatically for TypeScript libraries.
* Validate declarations before publishing packages.
* Use declaration maps when source navigation is important.

# 🗁 Common Mistakes

**Assuming declaration files contain implementations**

A `.d.ts` file describes types. It does not provide runtime JavaScript behavior.

**Creating declarations that do not match implementations**

Incorrect declarations can allow invalid code to pass type checking and cause runtime errors.

**Using unnecessary `declare` statements**

In a declaration file, top-level declarations already describe ambient APIs. For example, `export function add(...): number;` is sufficient; an additional `declare` modifier is unnecessary.

**Forgetting to include declaration files**

Custom declaration files outside the configured project scope may not be loaded.

**Confusing declarations with runtime exports**

Declaring a function does not create or export that function at runtime.

# 🗁 Key Takeaways

* Declaration files use the `.d.ts` extension.
* They describe the types and APIs of JavaScript implementations.
* Declaration files do not contain executable implementations.
* `declare` describes variables and APIs provided elsewhere.
* Module declarations describe external JavaScript packages.
* `@types` packages provide declarations for many third-party libraries.
* TypeScript can generate declarations using the `declaration` compiler option.
* Declaration files are essential for distributing strongly typed JavaScript libraries.
