# Type-Only Exports

Type-only exports allow TypeScript declarations to be exposed exclusively for compile-time type checking without creating JavaScript runtime exports.

They are useful when building reusable modules, libraries, packages, and application architectures that separate type definitions from runtime functionality.

TypeScript provides two main approaches:

* `export type` for exporting individual type declarations or groups of types.
* `export { type Name }` for marking individual declarations as type-only within an export list.

# 🗁 What Are Type-Only Exports?

Normally, the `export` keyword can expose runtime values and type declarations.

```typescript
export interface User {
  id: number;
  name: string;
}

export function createUser(name: string): User {
  return {
    id: 101,
    name,
  };
}
```

In this example:

* `User` is an interface used only during type checking.
* `createUser` is a runtime function.

TypeScript supports explicitly marking exports as type-only.

```typescript
export type {
  User,
};
```

Type-only exports communicate that the exported declaration is intended exclusively for the type system.

# 🗁 Exporting a Single Type

Use `export type` to export one type declaration.

```typescript
type UserId = number;

export type UserId;
```

This syntax is not valid as written because a type declaration must be exported through a declaration or an export list.

Correct approach:

```typescript
export type UserId = number;
```

Another approach is to define the type first and export it afterward:

```typescript
type UserId = number;

export type {
  UserId,
};
```

Both approaches expose `UserId` as a type-only export.

# 🗁 Exporting Multiple Types

You can export multiple type declarations using one export list.

```typescript
interface User {
  id: number;
  name: string;
}

type UserRole = "admin" | "customer";

type UserStatus = "active" | "inactive";

export type {
  User,
  UserRole,
  UserStatus,
};
```

Another module can import them:

```typescript
import type {
  User,
  UserRole,
  UserStatus,
} from "./user.types";
```

This keeps related type declarations organized.

# 🗁 Exporting Interfaces

Interfaces can be exported using `export type`.

```typescript
interface Product {
  id: number;
  name: string;
  price: number;
}

export type {
  Product,
};
```

Importing:

```typescript
import type { Product } from "./product";

const product: Product = {
  id: 101,
  name: "Laptop",
  price: 1200,
};
```

Interfaces are erased during compilation, so they do not produce JavaScript runtime values.

You can also export an interface directly:

```typescript
export interface Product {
  id: number;
  name: string;
  price: number;
}
```

Both forms are valid.

# 🗁 Exporting Type Aliases

Type aliases can also be exported explicitly as types.

```typescript
type ProductId = number;

type ProductCategory =
  | "electronics"
  | "clothing"
  | "books";

type ProductStatus =
  | "available"
  | "unavailable";

export type {
  ProductId,
  ProductCategory,
  ProductStatus,
};
```

Importing:

```typescript
import type {
  ProductId,
  ProductCategory,
  ProductStatus,
} from "./product.types";
```

Type-only exports are especially useful for sharing reusable domain models.

# 🗁 Exporting Generic Types

Generic interfaces and type aliases can be exported as type-only declarations.

```typescript
interface ApiResponse<T> {
  success: boolean;
  data: T;
  message?: string;
}

type PaginatedResponse<T> = {
  items: T[];
  total: number;
  page: number;
};

export type {
  ApiResponse,
  PaginatedResponse,
};
```

Importing:

```typescript
import type {
  ApiResponse,
  PaginatedResponse,
} from "./api.types";
```

Using the generic types:

```typescript
interface User {
  id: number;
  name: string;
}

const response: ApiResponse<User> = {
  success: true,
  data: {
    id: 101,
    name: "Alex",
  },
};

const users: PaginatedResponse<User> = {
  items: [
    { id: 101, name: "Alex" },
    { id: 102, name: "Sam" },
  ],
  total: 2,
  page: 1,
};
```

# 🗁 Exporting Type-Only Declarations with `export type`

You can use the `export type` declaration form for a group of declarations.

```typescript
interface User {
  id: number;
  name: string;
}

interface Admin extends User {
  permissions: string[];
}

type UserRole = "admin" | "customer";

export type {
  User,
  Admin,
  UserRole,
};
```

This is useful when a module contains many declarations that should be available only to TypeScript consumers.

# 🗁 Inline Type-Only Exports

You can mark individual declarations as type-only inside a regular export list.

```typescript
interface User {
  id: number;
  name: string;
}

const DEFAULT_ROLE = "customer";

function createUser(name: string): User {
  return {
    id: 101,
    name,
  };
}

export {
  type User,
  DEFAULT_ROLE,
  createUser,
};
```

Here:

* `User` is exported only as a type.
* `DEFAULT_ROLE` is exported as a runtime value.
* `createUser` is exported as a runtime function.

This syntax is useful when a module exposes both types and runtime functionality.

# 🗁 Type-Only Re-Exports

You can re-export types from another module without importing them into the current module.

`user.types.ts`

```typescript
export interface User {
  id: number;
  name: string;
}

export type UserRole = "admin" | "customer";
```

`index.ts`

```typescript
export type {
  User,
  UserRole,
} from "./user.types";
```

Another module can import these types through the index file:

```typescript
import type {
  User,
  UserRole,
} from "./index";
```

This is useful for creating centralized type entry points.

# 🗁 Re-Exporting All Types

You can use `export type *` to re-export all type exports from another module.

`user.types.ts`

```typescript
export interface User {
  id: number;
  name: string;
}

export type UserRole = "admin" | "customer";

export type UserStatus = "active" | "inactive";
```

`index.ts`

```typescript
export type * from "./user.types";
```

This forwards the module's type exports without forwarding its runtime values.

TypeScript 5.0 introduced support for `export type *`.

# 🗁 Re-Exporting a Type with an Alias

You can rename a type when re-exporting it.

`user.types.ts`

```typescript
export interface User {
  id: number;
  name: string;
}
```

`index.ts`

```typescript
export type {
  User as UserModel,
} from "./user.types";
```

Importing:

```typescript
import type { UserModel } from "./index";

const user: UserModel = {
  id: 101,
  name: "Alex",
};
```

The original declaration remains named `User` in its defining module.

# 🗁 Type-Only Exports from Classes

Classes create both runtime values and instance types.

You can expose a class only as a type when consumers need its instance type but not its constructor.

`user.ts`

```typescript
export class User {
  constructor(
    public id: number,
    public name: string
  ) {}
}
```

`index.ts`

```typescript
export type { User } from "./user";
```

Importing:

```typescript
import type { User } from "./index";

function displayUser(user: User): void {
  console.log(user.name);
}
```

The consumer can use `User` as a type but cannot instantiate it through this type-only import.

If consumers need the constructor, use a regular export:

```typescript
export { User } from "./user";
```

# 🗁 Type-Only Exports from Enums

Regular enums create runtime JavaScript values.

You can expose an enum as a type-only declaration when consumers only need its type.

`user-role.ts`

```typescript
export enum UserRole {
  Admin = "ADMIN",
  Customer = "CUSTOMER",
}
```

`index.ts`

```typescript
export type { UserRole } from "./user-role";
```

A consumer can use the imported enum type:

```typescript
import type { UserRole } from "./index";

function assignRole(role: UserRole): void {
  console.log(role);
}
```

However, the consumer cannot access enum members through this type-only export.

If enum members are required at runtime, use a regular re-export:

```typescript
export { UserRole } from "./user-role";
```

# 🗁 Default Type-Only Exports

A module can default-export a type declaration.

```typescript
interface User {
  id: number;
  name: string;
}

export default User;
```

Another module can import it using `import type`:

```typescript
import type User from "./user";

const user: User = {
  id: 101,
  name: "Alex",
};
```

When re-exporting a default type declaration, use:

```typescript
export type { default } from "./user";
```

You can also expose it under a named type export:

```typescript
export type {
  default as User,
} from "./user";
```

# 🗁 Type-Only Exports and JavaScript Output

Type-only exports are removed from emitted JavaScript.

Consider:

```typescript
export interface User {
  id: number;
  name: string;
}

export type UserRole = "admin" | "customer";

export const DEFAULT_ROLE = "customer";
```

The interface and type alias do not generate JavaScript declarations.

Conceptually, the emitted JavaScript contains:

```javascript
export const DEFAULT_ROLE = "customer";
```

The exact output depends on the TypeScript compiler configuration and module target.

# 🗁 Type-Only Exports with `verbatimModuleSyntax`

The `verbatimModuleSyntax` compiler option makes type-only imports and exports explicit.

Example:

```typescript
export interface User {
  id: number;
  name: string;
}

export const DEFAULT_ROLE = "customer";
```

You can explicitly export the type and value:

```typescript
export {
  type User,
  DEFAULT_ROLE,
};
```

Or use separate declarations:

```typescript
export type { User };

export { DEFAULT_ROLE };
```

Example configuration:

```json
{
  "compilerOptions": {
    "verbatimModuleSyntax": true,
    "module": "ESNext",
    "target": "ES2022"
  }
}
```

When this option is enabled, TypeScript requires developers to express type-only intent explicitly where necessary.

# 🗁 Type-Only Exports in Barrel Files

Barrel files provide a centralized entry point for declarations from multiple modules.

Example structure:

```text
src/
└── users/
    ├── user.model.ts
    ├── user.types.ts
    ├── user.service.ts
    └── index.ts
```

`user.model.ts`

```typescript
export class UserModel {
  constructor(
    public id: number,
    public name: string
  ) {}
}
```

`user.types.ts`

```typescript
export interface UserDTO {
  id: number;
  name: string;
}

export type UserRole = "admin" | "customer";
```

`user.service.ts`

```typescript
import type { UserDTO } from "./user.types";

export function getUsers(): UserDTO[] {
  return [
    {
      id: 101,
      name: "Alex",
    },
  ];
}
```

`index.ts`

```typescript
export { UserModel } from "./user.model";

export type {
  UserDTO,
  UserRole,
} from "./user.types";

export { getUsers } from "./user.service";
```

Consumers can import everything through one module:

```typescript
import {
  UserModel,
  getUsers,
  type UserDTO,
  type UserRole,
} from "./users";
```

This creates a clear public interface while distinguishing runtime functionality from type declarations.

# 🗁 Type-Only Exports vs Regular Exports

| Feature | Type-Only Export | Regular Export |
| --- | --- | --- |
| Syntax | `export type` | `export` |
| Purpose | Expose type information | Expose declarations |
| JavaScript runtime export | No | Depends on declaration |
| Interfaces | Supported | Supported as type declarations |
| Type aliases | Supported | Supported as type declarations |
| Functions | Not available as runtime values | Supported |
| Classes | Can expose only their type | Exposes constructor and type |
| Enum members | Not available at runtime | Available for regular enums |
| Re-exporting | `export type { ... }` | `export { ... }` |

# 🗁 Common Type-Only Export Errors

**Trying to use a type-only export as a runtime value**

```typescript
export class User {}
```

```typescript
export type { User } from "./user";
```

This makes the class available only as a type through the second module.

Trying to instantiate it through that module will fail:

```typescript
import { User } from "./index";

const user = new User();
```

Use a regular re-export when the constructor is needed:

```typescript
export { User } from "./user";
```

**Using `export type` with a runtime function**

Incorrect:

```typescript
export type {
  createUser,
} from "./user";
```

This does not expose `createUser` as a runtime function.

Correct:

```typescript
export { createUser } from "./user";
```

**Confusing type-only exports with regular exports**

A regular export list can contain both types and values, but explicit type-only modifiers make the distinction clear.

```typescript
export {
  type User,
  createUser,
  DEFAULT_ROLE,
} from "./user";
```

# 🗁 Best Practices

* Use `export type` for declarations intended exclusively for type checking.
* Use inline `type` modifiers when exporting both types and runtime values.
* Prefer explicit type-only re-exports in barrel files.
* Use `export type *` when forwarding all type exports from a module.
* Use regular exports when consumers need runtime functionality.
* Avoid exposing internal type declarations unnecessarily.
* Keep public type definitions organized by domain or feature.
* Use `verbatimModuleSyntax` when explicit module syntax is desired.
* Maintain consistent export conventions throughout the project.

# 🗁 Summary

Type-only exports expose declarations exclusively for TypeScript's type system.

Key concepts:

* `export type` creates type-only exports.
* Interfaces and type aliases can be exported as types.
* Multiple type declarations can be exported together.
* Inline `type` modifiers allow mixed type and value exports.
* Type-only re-exports use `export type { ... }`.
* `export type *` forwards all type exports from another module.
* Type declarations can be renamed during re-exporting.
* Classes and enums can be exposed only as types.
* Default type declarations can also be re-exported.
* Type-only exports do not create JavaScript runtime exports.

Understanding type-only exports helps build clear module boundaries, reduce unnecessary runtime dependencies, and design maintainable TypeScript libraries.
