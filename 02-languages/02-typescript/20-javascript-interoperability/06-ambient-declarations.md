# Ambient Declarations in TypeScript

Ambient declarations describe variables, functions, classes, modules, and other APIs that exist outside the current TypeScript code.

They tell the TypeScript compiler what an external API looks like without providing its implementation.

The `declare` keyword is used to create ambient declarations.

For example:

```typescript
declare const APP_NAME: string;

declare function greet(name: string): string;
```

These declarations tell TypeScript:

* `APP_NAME` exists and has the type `string`.
* `greet` exists, accepts a string, and returns a string.

They do not create either variable or function at runtime.

# 🗁 Why Ambient Declarations Are Needed

Ambient declarations are useful when working with:

* JavaScript libraries without TypeScript definitions.
* Browser APIs and custom global variables.
* Node.js runtime APIs.
* Third-party scripts loaded through HTML.
* External modules.
* Environment variables.
* Existing JavaScript applications.

They allow TypeScript to provide type checking and editor support for APIs implemented elsewhere.

# 🗁 The declare Keyword

The `declare` keyword tells TypeScript that a value or API exists but is implemented outside the current declaration.

Example:

```typescript
declare const API_URL: string;

declare let requestCount: number;

declare function logMessage(message: string): void;
```

You can use these declarations in regular TypeScript code:

```typescript
console.log(API_URL);

requestCount++;

logMessage("Application started");
```

The runtime must provide these values and functions.

# 🗁 Ambient Variable Declarations

Use ambient variable declarations to describe globally available variables.

```typescript
// globals.d.ts

declare const APP_VERSION: string;

declare const API_BASE_URL: string;

declare let currentUser: {
  id: number;
  name: string;
} | null;
```

Usage:

```typescript
console.log(APP_VERSION);
console.log(API_BASE_URL);

if (currentUser) {
  console.log(currentUser.name);
}
```

These declarations provide compile-time information only.

# 🗁 Ambient Function Declarations

Ambient function declarations describe functions implemented elsewhere.

```typescript
// analytics.d.ts

declare function trackEvent(
  eventName: string,
  properties?: Record<string, unknown>
): void;
```

Usage:

```typescript
trackEvent("page_view");

trackEvent("purchase", {
  productId: 101,
  amount: 49.99,
});
```

The function implementation must be provided by the runtime or another script.

# 🗁 Ambient Class Declarations

You can describe externally implemented classes.

```typescript
// external-client.d.ts

declare class ExternalClient {
  constructor(apiKey: string);

  connect(): Promise<void>;

  disconnect(): void;

  getStatus(): "connected" | "disconnected";
}
```

Usage:

```typescript
const client = new ExternalClient("secret-key");

await client.connect();

console.log(client.getStatus());

client.disconnect();
```

The declaration does not generate the class implementation.

# 🗁 Ambient Module Declarations

Ambient module declarations describe modules that do not provide their own TypeScript definitions.

Use `declare module`:

```typescript
// custom-library.d.ts

declare module "custom-library" {
  export function initialize(): void;

  export function calculate(value: number): number;

  export interface Options {
    debug: boolean;
  }
}
```

Usage:

```typescript
import {
  initialize,
  calculate,
  type Options,
} from "custom-library";

const options: Options = {
  debug: true,
};

initialize();

console.log(calculate(10));
```

This declaration allows TypeScript to understand the module's public API.

The actual module must still be installed or otherwise available at runtime.

# 🗁 Ambient Module Declarations for Untyped JavaScript Libraries

Suppose a JavaScript library exists but has no TypeScript declarations.

JavaScript implementation:

```javascript
// legacy-library.js

export function formatName(name) {
  return name.trim().toUpperCase();
}
```

Create a declaration:

```typescript
// legacy-library.d.ts

declare module "legacy-library" {
  export function formatName(name: string): string;
}
```

Now TypeScript can provide type checking:

```typescript
import { formatName } from "legacy-library";

const result = formatName("alice");

console.log(result);
```

The declaration should accurately represent the JavaScript implementation.

# 🗁 Ambient Namespace Declarations

Namespaces can describe APIs organized under a shared global object.

```typescript
// legacy-api.d.ts

declare namespace LegacyAPI {
  function initialize(): void;

  function getVersion(): string;

  interface Configuration {
    debug: boolean;
  }
}
```

Usage:

```typescript
const config: LegacyAPI.Configuration = {
  debug: true,
};

LegacyAPI.initialize();

console.log(LegacyAPI.getVersion());
```

The namespace describes an API provided elsewhere. It does not automatically create a runtime object named `LegacyAPI`.

# 🗁 Ambient Declarations in Global Scope

A declaration file without top-level imports or exports is generally treated as a script declaration file.

Its declarations can contribute to the global scope.

```typescript
// globals.d.ts

declare const APP_NAME: string;

declare function showNotification(
  message: string
): void;
```

These declarations can be used throughout the TypeScript project when the file is included in the compilation.

# 🗁 Global Augmentation with declare global

When a declaration file is already a module, use `declare global` to add declarations to the global scope.

```typescript
// custom-globals.d.ts

export {};

declare global {
  interface Window {
    APP_NAME: string;
    API_URL: string;
  }

  function trackPageView(path: string): void;
}
```

Usage:

```typescript
console.log(window.APP_NAME);

trackPageView("/dashboard");
```

The `export {}` makes the file a module.

The `declare global` block adds declarations to the global namespace.

# 🗁 Ambient Declarations for Third-Party Scripts

Suppose an external JavaScript script exposes a global object:

```html
<script src="https://example.com/payment-sdk.js"></script>
```

The script provides a global `PaymentSDK` object.

You can describe its API:

```typescript
// payment-sdk.d.ts

interface PaymentResult {
  transactionId: string;
  status: "success" | "failed";
}

interface PaymentOptions {
  amount: number;
  currency: string;
}

declare const PaymentSDK: {
  initialize(apiKey: string): void;

  processPayment(
    options: PaymentOptions
  ): Promise<PaymentResult>;
};
```

Usage:

```typescript
PaymentSDK.initialize("public-api-key");

const result = await PaymentSDK.processPayment({
  amount: 100,
  currency: "USD",
});

console.log(result.transactionId);
```

The external script must load successfully before the code uses the global object.

# 🗁 Ambient Declarations for Environment Variables

Environment variables are commonly accessed through runtime-specific APIs.

For example, a Node.js application may use:

```typescript
process.env.DATABASE_URL;
```

Node.js type definitions provide the relevant declarations.

For custom environment variables, you can augment the existing `ProcessEnv` interface:

```typescript
// environment.d.ts

declare namespace NodeJS {
  interface ProcessEnv {
    DATABASE_URL: string;
    NODE_ENV: "development" | "production" | "test";
  }
}
```

Usage:

```typescript
const databaseUrl = process.env.DATABASE_URL;

const environment = process.env.NODE_ENV;
```

These declarations improve static typing, but they do not guarantee that the environment variables actually exist at runtime.

# 🗁 Ambient Declarations vs Regular Declarations

| Feature | Regular declaration | Ambient declaration |
| --- | --- | --- |
| Purpose | Defines TypeScript code | Describes externally implemented APIs |
| Implementation | May provide implementation | Does not provide runtime implementation |
| `declare` keyword | Not always needed | Commonly used |
| Runtime output | May generate JavaScript | Declaration itself generates no implementation |
| Typical location | `.ts` files | `.d.ts` files |

Example of a regular declaration:

```typescript
function add(a: number, b: number): number {
  return a + b;
}
```

Example of an ambient declaration:

```typescript
declare function add(a: number, b: number): number;
```

The first implements the function. The second only describes it.

# 🗁 Ambient Declarations vs Module Augmentation

These concepts are related but serve different purposes.

**Ambient module declaration**

Describes a module that TypeScript does not already understand.

```typescript
declare module "legacy-library" {
  export function initialize(): void;
}
```

**Module augmentation**

Extends an existing module's declarations.

```typescript
import "some-library";

declare module "some-library" {
  interface User {
    role: string;
  }
}
```

Module augmentation is used to extend existing type definitions rather than replace or independently describe the entire module.

# 🗁 Organizing Ambient Declaration Files

A practical project structure:

```text
my-project/
├── src/
│   ├── app.ts
│   └── services/
│       └── payment.ts
├── types/
│   ├── globals.d.ts
│   ├── environment.d.ts
│   ├── payment-sdk.d.ts
│   └── legacy-library.d.ts
├── package.json
└── tsconfig.json
```

Example `tsconfig.json`:

```json
{
  "compilerOptions": {
    "strict": true,
    "noEmit": true
  },
  "include": [
    "src/**/*.ts",
    "types/**/*.d.ts"
  ]
}
```

The `include` setting ensures that the custom declaration files are part of the TypeScript project.

# 🗁 Best Practices

* Use ambient declarations only for APIs implemented elsewhere.
* Keep declarations consistent with actual runtime behavior.
* Prefer module declarations over unnecessary global declarations.
* Use `declare global` when extending global APIs from a module.
* Organize custom declaration files in a dedicated directory.
* Avoid declaring values that do not exist at runtime.
* Prefer official type definitions when available.
* Use precise types instead of `any`.
* Validate external data at runtime even when declarations describe its expected structure.

# 🗁 Common Mistakes

**Assuming declare creates a variable**

```typescript
declare const APP_NAME: string;
```

This does not create `APP_NAME` in JavaScript.

**Declaring incorrect external APIs**

If the declaration does not match the actual implementation, TypeScript may accept code that fails at runtime.

**Forgetting to include declaration files**

A declaration file outside the TypeScript project's configured scope may not be loaded.

**Confusing ambient declarations with module augmentation**

Use ambient module declarations to describe an otherwise untyped module. Use module augmentation to extend an existing module's types.

**Creating unnecessary global declarations**

Global declarations can introduce naming conflicts and make dependencies less explicit.

# 🗁 Key Takeaways

* Ambient declarations describe APIs implemented outside the current TypeScript code.
* The `declare` keyword provides compile-time type information without creating runtime implementations.
* Ambient declarations can describe variables, functions, classes, namespaces, and modules.
* `.d.ts` files are the most common place for custom ambient declarations.
* `declare module` describes external modules.
* `declare global` extends the global scope from a module.
* Ambient declarations are essential when integrating TypeScript with existing JavaScript libraries and external runtime APIs.
