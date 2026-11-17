# Interface Merging

Interface merging is a TypeScript feature that allows multiple declarations with the same interface name to be combined into a single interface.

When TypeScript encounters multiple interface declarations with the same name in the same scope, it merges their members automatically.

This behavior is called **declaration merging**.

Interface merging helps you:

* Extend existing interfaces without modifying their original declarations.
* Organize large interfaces into multiple declarations.
* Add properties and methods to third-party library interfaces.
* Extend global and module-specific type definitions.
* Support extensible application architectures.

# 🗁 Basic Syntax

Declare multiple interfaces with the same name.

```typescript
interface User {
  id: number;
  name: string;
}

interface User {
  email: string;
  age: number;
}
```

TypeScript combines both declarations into one interface.

The resulting `User` interface is equivalent to:

```typescript
interface User {
  id: number;
  name: string;
  email: string;
  age: number;
}
```

You can create an object using all merged properties:

```typescript
const user: User = {
  id: 1,
  name: "Alice",
  email: "alice@example.com",
  age: 25,
};
```

# 🗁 Merging Interfaces with Methods

Multiple interface declarations can contribute method signatures to the same interface.

```typescript
interface Logger {
  log(message: string): void;
}

interface Logger {
  error(message: string): void;
  warn(message: string): void;
}
```

The merged interface contains all three methods.

```typescript
const logger: Logger = {
  log(message) {
    console.log(message);
  },

  error(message) {
    console.error(message);
  },

  warn(message) {
    console.warn(message);
  },
};
```

# 🗁 Merging Interfaces with Function Overloads

When multiple declarations contain methods with the same name, TypeScript combines their signatures as overloads.

```typescript
interface Formatter {
  format(value: string): string;
}

interface Formatter {
  format(value: number): string;
}
```

The resulting interface supports both signatures.

```typescript
const formatter: Formatter = {
  format(value: string | number): string {
    return String(value);
  },
};

formatter.format("Hello");
formatter.format(100);
```

The implementation must be compatible with the available overload signatures.

# 🗁 Method Overload Ordering

When interfaces are merged, later declarations generally take precedence in overload ordering.

However, specialized signatures receive special treatment.

For example:

```typescript
interface DocumentProcessor {
  process(value: string | number): string;
}

interface DocumentProcessor {
  process(value: string): string;
}
```

The more specific string signature is placed before the broader union signature in the merged overload set.

This can influence overload resolution when multiple signatures are applicable.

# 🗁 Property Merging Rules

Properties with the same name must have compatible types.

Consider:

```typescript
interface User {
  id: number;
}

interface User {
  id: number;
  name: string;
}
```

This is valid because both declarations define `id` with the same type.

However, incompatible property declarations produce an error.

```typescript
interface User {
  id: number;
}

// Error: subsequent declarations must have the same type.
interface User {
  id: string;
}
```

A property cannot be declared with conflicting types across merged interfaces.

# 🗁 Optional and Required Property Conflicts

Optional and required declarations of the same property cannot be merged as if they were identical.

```typescript
interface User {
  email?: string;
}

// Error: declarations of email are incompatible.
interface User {
  email: string;
}
```

Keep the property declarations consistent.

```typescript
interface User {
  email?: string;
}

interface User {
  email?: string;
}
```

The resulting property remains optional.

# 🗁 Interface Merging vs Interface Extension

Interface merging and interface extension both allow interfaces to gain additional members, but they work differently.

**Interface merging:**

```typescript
interface User {
  id: number;
}

interface User {
  name: string;
}
```

Both declarations share the same name and are automatically combined.

**Interface extension:**

```typescript
interface User {
  id: number;
}

interface Admin extends User {
  permissions: string[];
}
```

The second interface creates a new interface that inherits from `User`.

| Feature | Interface Merging | Interface Extension |
| --- | --- | --- |
| Keyword | None | `extends` |
| Interface names | Same name | Different names |
| Result | One combined interface | Separate related interfaces |
| Original interface | Expanded | Remains unchanged |
| Common use | Augmenting existing declarations | Creating specialized contracts |

# 🗁 Interface Merging vs Type Alias

Interfaces support declaration merging, but type aliases do not.

Valid interface merging:

```typescript
interface Product {
  id: number;
}

interface Product {
  name: string;
}
```

Invalid duplicate type alias declarations:

```typescript
type Product = {
  id: number;
};

// Error: duplicate identifier.
type Product = {
  name: string;
};
```

If you need to combine type aliases, use intersection types.

```typescript
type ProductIdentity = {
  id: number;
};

type ProductDetails = {
  name: string;
};

type Product = ProductIdentity & ProductDetails;
```

# 🗁 Extending Third-Party Library Interfaces

Interface merging is especially useful when a library exposes an interface that needs additional application-specific properties.

Suppose a library provides:

```typescript
interface User {
  id: number;
  name: string;
}
```

You can declare another interface with the same name in the appropriate scope to add properties.

```typescript
interface User {
  role: "admin" | "customer";
}
```

The resulting interface includes:

```typescript
interface User {
  id: number;
  name: string;
  role: "admin" | "customer";
}
```

In real applications, the declaration must be placed in the correct module or global scope for the library's interface to be augmented.

# 🗁 Module Augmentation

Module augmentation is a specialized form of declaration merging that allows you to extend declarations exported by an existing module.

Suppose a module exports a `User` interface:

```typescript
// user-library.ts

export interface User {
  id: number;
  name: string;
}
```

You can augment the module in another declaration file:

```typescript
// user-library-augmentation.d.ts

import "./user-library";

declare module "./user-library" {
  interface User {
    role: "admin" | "customer";
  }
}
```

Now the module's `User` interface includes the additional `role` property.

Usage:

```typescript
import type { User } from "./user-library";

const user: User = {
  id: 1,
  name: "Alice",
  role: "admin",
};
```

The augmentation changes TypeScript's type information. It does not automatically add runtime properties to objects.

# 🗁 Global Interface Merging

TypeScript also allows interfaces in the global scope to be extended.

For example, you can add a custom property to the global `Window` interface.

```typescript
export {};

declare global {
  interface Window {
    appVersion: string;
  }
}
```

You can then use the property in browser code:

```typescript
window.appVersion = "1.0.0";

console.log(window.appVersion);
```

The declaration tells TypeScript that the property exists. Your application must still initialize it at runtime.

# 🗁 Practical Example: Extending Application Configuration

Interface merging can be useful when multiple modules contribute configuration properties.

```typescript
interface AppConfig {
  appName: string;
}

interface AppConfig {
  version: string;
}

interface AppConfig {
  debug: boolean;
}
```

The resulting interface contains all three properties.

```typescript
const config: AppConfig = {
  appName: "My Application",
  version: "1.0.0",
  debug: true,
};
```

This pattern can be useful when configuration types are intentionally extended across modules.

However, use module augmentation or explicit composition when declarations belong to separate modules.

# 🗁 Important Limitations

Interface merging has several rules:

* Multiple declarations must use the same interface name and compatible declaration scope.
* Non-function members with the same name must have identical types and compatible optionality.
* Function members with the same name are combined as overloads.
* Type aliases cannot participate in declaration merging.
* Merging does not create runtime objects or properties.
* Module augmentation cannot introduce entirely new top-level declarations into an existing module.
* Default exports cannot be augmented by targeting the default export directly.
* Module augmentation must target the correct module and declaration.

# 🗁 Best Practices

* Use interface merging when an interface is intentionally designed to be extensible.
* Prefer interface extension when creating specialized application models.
* Use module augmentation to extend third-party module declarations.
* Keep global augmentations limited and well documented.
* Avoid duplicate declarations that make types difficult to understand.
* Ensure repeated property declarations have identical types and optionality.
* Remember that declaration merging affects compile-time types, not runtime behavior.
* Prefer explicit composition when merging would make the resulting interface difficult to track.

# 🗁 Summary

* Interface merging combines multiple declarations with the same interface name.
* TypeScript automatically creates one combined interface.
* Properties and methods from all declarations become available.
* Duplicate properties must have compatible declarations.
* Duplicate methods are combined into overloads.
* Interface merging differs from interface extension.
* Interfaces support declaration merging, but type aliases do not.
* Module augmentation extends declarations exported by existing modules.
* Global augmentation extends declarations available in the global scope.
* Declaration merging changes the type system, not runtime behavior.

# 🗁 Related Topics

* Object Types
* Type Aliases
* Interfaces
* Interface Extension
* Optional Properties
* Readonly Properties
* Function Overloads
* Declaration Merging
* Module Augmentation
* Global Augmentation
