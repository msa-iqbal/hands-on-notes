# Declaration Merging

Declaration merging is a TypeScript feature that allows multiple declarations with the same name to be combined into a single definition.

Instead of producing a duplicate declaration error, TypeScript can merge compatible declarations and make their combined members available throughout the program.

Declaration merging is particularly useful when working with:

* Interfaces.
* Namespaces.
* Classes and namespaces.
* Functions and namespaces.
* Enums and namespaces.
* Third-party library extensions.
* Global type definitions.

# 🗁 What Is Declaration Merging?

Normally, declaring the same identifier multiple times can cause a naming conflict.

However, TypeScript allows certain declaration types to share the same name.

For example:

```typescript
interface User {
  id: number;
}

interface User {
  name: string;
}
```

TypeScript merges these interfaces into one interface.

The resulting type is equivalent to:

```typescript
interface User {
  id: number;
  name: string;
}
```

You can use both properties:

```typescript
const user: User = {
  id: 101,
  name: "Alex",
};
```

This behavior is called declaration merging.

# 🗁 Declaration Merging with Interfaces

Interfaces are the most common example of declaration merging.

Multiple interfaces with the same name in the same scope are merged.

```typescript
interface Product {
  id: number;
}

interface Product {
  name: string;
}

interface Product {
  price: number;
}
```

The resulting interface contains all three properties.

```typescript
const product: Product = {
  id: 101,
  name: "Laptop",
  price: 1200,
};
```

Each declaration contributes additional members to the same interface.

# 🗁 Merging Interface Properties

When interfaces merge, their properties are combined.

```typescript
interface User {
  id: number;
  name: string;
}

interface User {
  email: string;
  active: boolean;
}
```

The merged interface is equivalent to:

```typescript
interface User {
  id: number;
  name: string;
  email: string;
  active: boolean;
}
```

All required properties must be provided when creating an object of this type.

```typescript
const user: User = {
  id: 101,
  name: "Alex",
  email: "alex@example.com",
  active: true,
};
```

# 🗁 Duplicate Property Compatibility

When two interfaces declare the same property, their declarations must be compatible.

For non-function members, the property types must be identical.

Valid:

```typescript
interface User {
  id: number;
}

interface User {
  id: number;
}
```

Invalid:

```typescript
interface User {
  id: number;
}

interface User {
  id: string;
}
```

The second declaration causes a type error because the same property cannot have conflicting types.

# 🗁 Optional Properties in Declaration Merging

Optional properties can also be merged when their declarations are compatible.

```typescript
interface User {
  id: number;
}

interface User {
  email?: string;
}
```

The resulting interface contains both properties.

```typescript
const user: User = {
  id: 101,
};
```

The `email` property is optional, so it does not need to be provided.

# 🗁 Merging Function Members

Interfaces can contain multiple function signatures with the same name.

These signatures form an overload set.

```typescript
interface Formatter {
  format(value: string): string;
}

interface Formatter {
  format(value: number): string;
}
```

The merged interface supports both signatures.

```typescript
const formatter: Formatter = {
  format(value: string | number): string {
    return String(value);
  },
};

formatter.format("Hello");

formatter.format(100);
```

The implementation must be compatible with all declared overloads.

# 🗁 Interface Merging and Method Overloads

When interfaces merge, overload signatures are combined.

```typescript
interface SearchService {
  search(query: string): string[];
}

interface SearchService {
  search(query: string, limit: number): string[];
}

interface SearchService {
  search(query: string, limit: number, offset: number): string[];
}
```

The merged interface supports three call signatures.

```typescript
const service: SearchService = {
  search(
    query: string,
    limit?: number,
    offset?: number
  ): string[] {
    return [];
  },
};

service.search("typescript");

service.search("typescript", 10);

service.search("typescript", 10, 20);
```

Declaration merging is useful for extending APIs with additional method signatures.

# 🗁 Merging Generic Interfaces

Generic interfaces can be merged when their type parameters and declarations are compatible.

```typescript
interface Repository<T> {
  findById(id: number): T | undefined;
}

interface Repository<T> {
  findAll(): T[];
}
```

The merged interface contains both methods.

```typescript
interface User {
  id: number;
  name: string;
}

const repository: Repository<User> = {
  findById(id) {
    return {
      id,
      name: "Alex",
    };
  },

  findAll() {
    return [
      {
        id: 101,
        name: "Alex",
      },
    ];
  },
};
```

The same generic parameter is used by both declarations.

# 🗁 Declaration Merging with Namespaces

Namespaces can also participate in declaration merging.

Multiple namespace declarations with the same name are combined.

```typescript
namespace MathUtils {
  export function add(a: number, b: number): number {
    return a + b;
  }
}

namespace MathUtils {
  export function subtract(a: number, b: number): number {
    return a - b;
  }
}
```

Both functions are available through the same namespace.

```typescript
console.log(MathUtils.add(10, 5));

console.log(MathUtils.subtract(10, 5));
```

Namespace merging allows functionality to be organized across multiple declarations.

# 🗁 Namespace Members and Visibility

Only exported namespace members are accessible outside the namespace.

```typescript
namespace App {
  const internalValue = 100;

  export const publicValue = 200;
}
```

Accessing the exported member:

```typescript
console.log(App.publicValue);
```

Accessing the private member:

```typescript
// Error: internalValue is not exported.
console.log(App.internalValue);
```

This visibility rule also applies when namespaces are merged.

# 🗁 Merging Interfaces and Namespaces

An interface and a namespace can share the same name.

The namespace can provide additional functionality associated with the interface.

```typescript
interface User {
  id: number;
  name: string;
}

namespace User {
  export function create(
    id: number,
    name: string
  ): User {
    return {
      id,
      name,
    };
  }
}
```

Using the merged declarations:

```typescript
const user = User.create(101, "Alex");

console.log(user.name);
```

The interface provides the instance type, while the namespace provides the `create` function.

This pattern is useful when attaching related utilities to a type.

# 🗁 Merging Classes and Namespaces

A class can be merged with a namespace declared after it.

```typescript
class User {
  constructor(public name: string) {}
}

namespace User {
  export const DEFAULT_ROLE = "customer";

  export function create(name: string): User {
    return new User(name);
  }
}
```

Using the class and namespace:

```typescript
const user = User.create("Alex");

console.log(user.name);

console.log(User.DEFAULT_ROLE);
```

The class provides the constructor, while the namespace adds static-like functionality.

The namespace declaration must follow the class declaration.

# 🗁 Merging Functions and Namespaces

A function can also be merged with a namespace.

```typescript
function greet(name: string): string {
  return `Hello, ${name}`;
}

namespace greet {
  export const language = "English";

  export function formal(name: string): string {
    return `Good day, ${name}`;
  }
}
```

Using the merged declarations:

```typescript
console.log(greet("Alex"));

console.log(greet.language);

console.log(greet.formal("Alex"));
```

This pattern allows a function to expose additional properties and related utilities.

The namespace must be declared after the function.

# 🗁 Merging Enums and Namespaces

An enum can be merged with a namespace declared after it.

```typescript
enum UserRole {
  Admin = "ADMIN",
  Customer = "CUSTOMER",
}

namespace UserRole {
  export function isAdmin(role: UserRole): boolean {
    return role === UserRole.Admin;
  }
}
```

Using the merged declaration:

```typescript
console.log(UserRole.Admin);

console.log(UserRole.isAdmin(UserRole.Admin));

console.log(UserRole.isAdmin(UserRole.Customer));
```

This is useful when attaching helper functions to enum types.

# 🗁 Declaration Merging Rules

Not every declaration can merge with every other declaration.

TypeScript supports specific combinations.

| Declaration Combination | Supported |
| --- | --- |
| Interface + Interface | Yes |
| Namespace + Namespace | Yes |
| Class + Namespace | Yes |
| Function + Namespace | Yes |
| Enum + Namespace | Yes |
| Interface + Namespace | Yes |
| Class + Class | No |
| Function + Function | No |
| Enum + Enum | No |
| Type Alias + Type Alias | No |
| Class + Interface | No |

Some combinations require specific ordering and compatible declarations.

For example, class, function, and enum declarations must generally appear before their merging namespace declarations.

# 🗁 Declaration Merging vs Interface Extension

Declaration merging and interface extension are different mechanisms.

**Declaration merging**

Two interfaces with the same name are automatically combined.

```typescript
interface User {
  id: number;
}

interface User {
  name: string;
}
```

**Interface extension**

A new interface explicitly extends another interface.

```typescript
interface User {
  id: number;
}

interface Admin extends User {
  permissions: string[];
}
```

The difference:

* Declaration merging combines declarations with the same name.
* Interface extension creates a new interface based on another interface.

Both mechanisms help build reusable type definitions.

# 🗁 Declaration Merging vs Intersection Types

Intersection types combine multiple types using the `&` operator.

```typescript
type Identifiable = {
  id: number;
};

type Named = {
  name: string;
};

type User = Identifiable & Named;
```

The resulting type contains both properties.

Declaration merging achieves a similar result through repeated compatible interface declarations.

```typescript
interface User {
  id: number;
}

interface User {
  name: string;
}
```

However, they are not identical.

For example, conflicting property types in interface merging produce a declaration error, while intersections can produce an impossible property type.

# 🗁 Declaration Merging with Third-Party Libraries

Declaration merging is particularly useful when extending types from external libraries.

For example, a library might expose an interface:

```typescript
interface Request {
  url: string;
}
```

Your application may need to attach an authenticated user.

You can extend the interface through declaration merging:

```typescript
declare global {
  interface Request {
    user?: {
      id: number;
      email: string;
    };
  }
}
```

After merging, TypeScript recognizes the additional `user` property.

In real projects, the exact interface must be the one used by the library, and the declaration file must be included in the TypeScript compilation.

# 🗁 Declaration Merging with Express

Express applications commonly use declaration merging to add custom properties to request objects.

Example declaration file:

`src/types/express/index.d.ts`

```typescript
import "express";

declare global {
  namespace Express {
    interface Request {
      user?: {
        id: number;
        email: string;
      };
    }
  }
}

export {};
```

Now an Express handler can access the additional property:

```typescript
import type { Request, Response } from "express";

function getProfile(
  req: Request,
  res: Response
): void {
  if (!req.user) {
    res.status(401).send("Unauthorized");

    return;
  }

  res.json({
    id: req.user.id,
    email: req.user.email,
  });
}
```

The declaration adds compile-time information. It does not automatically attach a user object to the request at runtime.

Authentication middleware must still populate `req.user`.

# 🗁 Declaration Merging with Global Interfaces

You can extend global interfaces using a global augmentation.

For example, extending the built-in `Window` interface:

```typescript
export {};

declare global {
  interface Window {
    appVersion: string;
  }
}
```

Now TypeScript recognizes the additional property:

```typescript
window.appVersion = "1.0.0";

console.log(window.appVersion);
```

The declaration changes TypeScript's understanding of `Window`, but it does not create the actual property in the browser.

Your application must initialize it at runtime.

# 🗁 Declaration Merging and Module Scope

Declaration merging depends on scope.

Two declarations with the same name can merge when they belong to the same declaration scope and their declaration kinds support merging.

Consider:

```typescript
interface User {
  id: number;
}

export {};
```

This file is a module because it contains an export declaration.

Its `User` interface is module-scoped rather than global.

Another file declaring an interface named `User` does not automatically merge with it.

To share types across modules, use imports and exports.

# 🗁 Module Augmentation

Module augmentation allows you to extend declarations from an existing module.

For example, suppose a library exports:

`user-library.ts`

```typescript
export interface User {
  id: number;
  name: string;
}
```

Another declaration file can augment the module:

```typescript
import "./user-library";

declare module "./user-library" {
  interface User {
    email?: string;
  }
}
```

Now consumers of the module can use the additional property:

```typescript
import type { User } from "./user-library";

const user: User = {
  id: 101,
  name: "Alex",
  email: "alex@example.com",
};
```

Module augmentation modifies TypeScript's type information, not the actual runtime implementation.

It is commonly used to extend third-party library interfaces.

# 🗁 Limitations of Declaration Merging

Declaration merging has several restrictions.

* Classes cannot merge with other classes.
* Type aliases cannot be reopened.
* Duplicate interface properties must have compatible declarations.
* Namespace merging requires compatible declarations.
* Namespace declarations must follow certain declaration-order rules when merging with classes, functions, and enums.
* Module augmentation cannot introduce arbitrary new top-level declarations into an existing module.
* Global and module-scoped declarations do not merge automatically.
* Type declarations do not implement missing runtime functionality.

Understanding these limitations helps avoid confusing compiler errors.

# 🗁 Common Declaration Merging Errors

**Duplicate property with incompatible types**

```typescript
interface User {
  id: number;
}

interface User {
  id: string;
}
```

The declarations conflict because `id` has different types.

Use a consistent property definition.

**Trying to merge type aliases**

```typescript
type User = {
  id: number;
};

type User = {
  name: string;
};
```

Type aliases cannot be reopened through declaration merging.

Use an intersection type instead:

```typescript
type User = {
  id: number;
} & {
  name: string;
};
```

**Trying to merge two classes**

```typescript
class User {}

class User {}
```

Classes cannot merge with other classes.

Use inheritance, composition, or another suitable design.

**Declaring a namespace before a class**

The namespace must follow the class in a class-namespace merge.

```typescript
class User {
  constructor(public name: string) {}
}

namespace User {
  export const DEFAULT_ROLE = "customer";
}
```

**Expecting type augmentation to change runtime behavior**

Adding a property to an interface does not automatically create that property on an object.

The runtime implementation must still provide the property.

# 🗁 Best Practices

* Use interface merging when intentionally extending an existing interface.
* Prefer explicit interface extension when creating a distinct type.
* Use module augmentation when extending third-party declarations.
* Use global augmentation only when a genuine global extension is necessary.
* Avoid unnecessary global declarations.
* Keep declaration files organized and included in the TypeScript project.
* Ensure runtime implementations match augmented types.
* Avoid relying on declaration merging to hide poor module boundaries.
* Use namespaces selectively in modern applications.
* Document important declaration augmentations for other developers.

# 🗁 Summary

Declaration merging allows compatible TypeScript declarations with the same name to be combined.

Key concepts:

* Interfaces can merge with other interfaces.
* Namespaces can merge with other namespaces.
* Classes, functions, and enums can merge with namespaces under specific ordering rules.
* Merged interfaces combine properties and method signatures.
* Duplicate properties must have compatible types.
* Module augmentation extends declarations from existing modules.
* Global augmentation extends global declarations.
* Type aliases and classes cannot be reopened through declaration merging.
* Declaration merging changes type information, not runtime behavior.

Understanding declaration merging helps you extend existing APIs, customize third-party types, and build flexible TypeScript type systems.
