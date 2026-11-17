# Module Augmentation

Module augmentation is a TypeScript feature that allows you to extend the existing declarations of an external module without modifying its original source code.

It is commonly used when:

* Extending third-party library interfaces.
* Adding custom properties to existing framework types.
* Adding methods to existing classes through prototype extensions.
* Integrating application-specific properties into library-provided objects.
* Extending types from installed packages.

Module augmentation uses TypeScript's declaration merging mechanism to add new members to existing declarations.

# 🗁 Basic Module Augmentation

Suppose a module exports a `User` interface:

```typescript
// user.ts

export interface User {
  id: number;
  name: string;
}
```

You can extend this interface from another file:

```typescript
// user-augmentation.ts

import "./user";

declare module "./user" {
  interface User {
    email: string;
    role: string;
  }
}
```

The original `User` interface now includes the additional properties within the TypeScript program that includes this augmentation.

```typescript
// app.ts

import type { User } from "./user";
import "./user-augmentation";

const user: User = {
  id: 1,
  name: "Alice",
  email: "alice@example.com",
  role: "admin",
};
```

The original module remains unchanged.

# 🗁 Understanding declare module

The `declare module` syntax identifies the module whose existing declarations should be extended.

```typescript
declare module "module-name" {
  // Additional declarations
}
```

The module name must resolve to the module being augmented.

For example:

```typescript
declare module "some-library" {
  interface Options {
    debug: boolean;
  }
}
```

This tells TypeScript to merge the new `Options` declaration with the existing declaration exported by `some-library`, assuming that the target declaration is compatible and accessible for augmentation.

# 🗁 Module Augmentation with Third-Party Libraries

A common use case is extending interfaces provided by installed libraries.

For example, suppose a library exports an interface named `User`:

```typescript
// some-library declarations

export interface User {
  id: number;
  name: string;
}
```

Your application may need an additional `permissions` property.

```typescript
// types/some-library.d.ts

import "some-library";

declare module "some-library" {
  interface User {
    permissions: string[];
  }
}
```

Now TypeScript recognizes the additional property:

```typescript
import type { User } from "some-library";

const user: User = {
  id: 1,
  name: "Alice",
  permissions: ["read", "write"],
};
```

This technique is frequently used when customizing framework and library types.

# 🗁 Extending Express Request

Express applications often use module augmentation to add custom properties to the request object.

For example, an authentication middleware may attach the authenticated user to `Request`.

```typescript
// types/express.d.ts

import "express";

declare module "express-serve-static-core" {
  interface Request {
    user?: {
      id: string;
      email: string;
    };
  }
}
```

Now application code can access the custom property:

```typescript
import type { Request, Response } from "express";

function getProfile(req: Request, res: Response): void {
  if (!req.user) {
    res.status(401).json({
      message: "Unauthorized",
    });

    return;
  }

  res.json({
    id: req.user.id,
    email: req.user.email,
  });
}
```

The augmentation changes TypeScript's understanding of the request type.

It does not automatically add `user` to actual request objects. Middleware must still assign the property at runtime.

# 🗁 Extending Existing Class Types

Module augmentation can add methods to an existing class declaration.

Consider a library that exports a `Calculator` class:

```typescript
// calculator.ts

export class Calculator {
  add(a: number, b: number): number {
    return a + b;
  }
}
```

You can declare an additional method:

```typescript
// calculator-augmentation.ts

import "./calculator";

declare module "./calculator" {
  interface Calculator {
    multiply(a: number, b: number): number;
  }
}
```

The class instance type now includes `multiply`.

However, the declaration does not create the method at runtime.

You must implement it separately:

```typescript
// calculator-extension.ts

import { Calculator } from "./calculator";

Calculator.prototype.multiply = function (
  a: number,
  b: number,
): number {
  return a * b;
};
```

Usage:

```typescript
import { Calculator } from "./calculator";
import "./calculator-augmentation";
import "./calculator-extension";

const calculator = new Calculator();

console.log(calculator.add(2, 3)); // 5
console.log(calculator.multiply(2, 3)); // 6
```

The declaration provides type information, while the prototype assignment provides runtime behavior.

# 🗁 Augmenting Static Members

Module augmentation cannot directly add new top-level exports to an existing module.

It also cannot add arbitrary static members to an existing class through an interface augmentation.

For example, this does not extend the static side of the class:

```typescript
declare module "./calculator" {
  interface Calculator {
    static create(): Calculator;
  }
}
```

An interface named after a class describes its instance side, not its static side.

If you need additional static behavior, consider:

* Exporting a separate helper function.
* Creating a wrapper or subclass.
* Extending the module's exports through an appropriate module design.
* Using an existing library extension mechanism.

# 🗁 Module Augmentation vs Declaration Merging

Module augmentation and declaration merging are closely related, but they serve different purposes.

| Feature | Declaration merging | Module augmentation |
| --- | --- | --- |
| Purpose | Combines compatible declarations | Extends declarations exported by an existing module |
| Scope | Relevant declaration scope | Specific module |
| Typical use | Combining interface declarations | Extending library or framework types |
| Mechanism | Multiple declarations with the same name | `declare module` plus declaration merging |
| Runtime changes | No automatic runtime changes | No automatic runtime changes |

Example of declaration merging:

```typescript
interface User {
  id: number;
}

interface User {
  name: string;
}
```

Example of module augmentation:

```typescript
import "some-library";

declare module "some-library" {
  interface User {
    role: string;
  }
}
```

Module augmentation uses declaration merging to extend an existing module declaration.

# 🗁 Module Augmentation vs Module Declaration

These two concepts use similar syntax but have different purposes.

**Module augmentation:**

Extends an existing module.

```typescript
import "some-library";

declare module "some-library" {
  interface User {
    role: string;
  }
}
```

**Ambient module declaration:**

Describes a module when TypeScript does not already have its declarations.

```typescript
// custom-library.d.ts

declare module "custom-library" {
  export function initialize(): void;
}
```

The second example declares a module rather than extending an existing one.

The distinction matters because a top-level import or export makes a declaration file a module, changing how its `declare module` blocks are interpreted.

# 🗁 File Placement and TypeScript Configuration

Module augmentation declarations are often placed in dedicated `.d.ts` files.

Example project structure:

```text
src/
├── app.ts
├── types/
│   └── express.d.ts
└── middleware/
    └── auth.ts
```

Ensure TypeScript includes the declaration file.

For example:

```json
{
  "compilerOptions": {
    "strict": true
  },
  "include": [
    "src/**/*.ts",
    "src/**/*.d.ts"
  ]
}
```

If the declaration file is outside the configured `include` paths, TypeScript may not recognize the augmentation.

Also ensure the module specifier resolves to the intended package or module.

# 🗁 Runtime Considerations

Module augmentation only changes TypeScript's type information.

It does not:

* Modify the original library source code.
* Automatically add properties to runtime objects.
* Automatically implement newly declared methods.
* Guarantee that a third-party object contains an augmented property.

For example:

```typescript
declare module "some-library" {
  interface User {
    role: string;
  }
}
```

This makes `role` available to the type checker, but the application must still provide that property at runtime.

For runtime extensions, use actual implementation code, middleware, plugins, wrappers, or prototype modifications as appropriate.

# 🗁 Best Practices

* Use module augmentation when extending an existing module's exported declarations.
* Keep augmentations in dedicated declaration files when they are shared across the application.
* Verify that the target module is correctly resolved.
* Ensure declaration files are included in the TypeScript compilation context.
* Avoid changing third-party library types unnecessarily.
* Keep runtime implementations separate from type declarations.
* Prefer official extension points when libraries provide them.
* Avoid augmenting private or internal implementation details of dependencies.
* Document custom augmentations so other developers understand why they exist.

# 🗁 Key Takeaways

* Module augmentation extends declarations exported by an existing module.
* It uses `declare module` and declaration merging.
* It is commonly used to customize third-party library and framework types.
* Interface augmentation can extend existing object and class instance types.
* Class prototype extensions require separate runtime implementations.
* Module augmentation cannot freely add new top-level exports or static class members.
* Declaration files must be included in the TypeScript compilation context.
* Type augmentation does not automatically change runtime behavior.
