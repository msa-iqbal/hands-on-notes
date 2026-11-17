# Type-Only Imports

Type-only imports allow you to import declarations exclusively for TypeScript's type system without treating them as JavaScript runtime dependencies.

They are useful when working with interfaces, type aliases, type-only declarations, and other compile-time information.

TypeScript provides two main syntaxes:

* `import type` for importing only types.
* `import { type Name }` for marking individual named imports as type-only.

# 🗁 What Are Type-Only Imports?

Normally, an import statement can import runtime values and types.

```typescript
import { User, createUser } from "./user";
```

Suppose the module contains:

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

Here:

* `User` is an interface used only during type checking.
* `createUser` is a runtime function.

You can explicitly import only the type declarations using `import type`.

```typescript
import type { User } from "./user";

const user: User = {
  id: 101,
  name: "Alex",
};
```

The type-only import is erased from the emitted JavaScript.

# 🗁 Importing a Single Type

Use `import type` to import one type declaration.

```typescript
import type { User } from "./user";

const user: User = {
  id: 101,
  name: "Alex",
};
```

This makes the intention clear: the imported declaration is needed only for type checking.

# 🗁 Importing Multiple Types

You can import multiple type declarations using one statement.

```typescript
import type {
  User,
  UserRole,
  UserStatus,
} from "./user.types";
```

Example source module:

```typescript
export interface User {
  id: number;
  name: string;
}

export type UserRole = "admin" | "customer";

export type UserStatus = "active" | "inactive";
```

Using the imported types:

```typescript
const user: User = {
  id: 101,
  name: "Alex",
};

const role: UserRole = "admin";

const status: UserStatus = "active";
```

# 🗁 Importing Type Aliases

Type aliases can be imported using `import type`.

`user.types.ts`

```typescript
export type UserId = number;

export type UserName = string;

export type UserRole = "admin" | "customer";
```

`app.ts`

```typescript
import type {
  UserId,
  UserName,
  UserRole,
} from "./user.types";

const id: UserId = 101;

const name: UserName = "Alex";

const role: UserRole = "admin";
```

Type aliases do not create runtime JavaScript values.

# 🗁 Importing Interfaces

Interfaces are also imported using type-only imports.

`user.ts`

```typescript
export interface User {
  id: number;
  name: string;
}

export interface Admin extends User {
  permissions: string[];
}
```

`app.ts`

```typescript
import type {
  User,
  Admin,
} from "./user";

const user: User = {
  id: 101,
  name: "Alex",
};

const admin: Admin = {
  id: 102,
  name: "Sam",
  permissions: ["read", "write"],
};
```

# 🗁 Importing Types from Classes

A class creates a runtime value and also provides an instance type.

You can import only its type when you do not need to instantiate or otherwise access the class at runtime.

`user.ts`

```typescript
export class User {
  constructor(
    public id: number,
    public name: string
  ) {}
}
```

`user-service.ts`

```typescript
import type { User } from "./user";

export function getUser(): User {
  return {
    id: 101,
    name: "Alex",
  };
}
```

The function uses the class-derived instance type, but it does not need the `User` constructor at runtime.

# 🗁 Importing a Class as a Type

A class can be used as a type without importing its runtime value.

```typescript
import type { User } from "./user";

function displayUser(user: User): void {
  console.log(user.name);
}
```

However, you cannot instantiate the class through a type-only import.

```typescript
import type { User } from "./user";

// Error: User cannot be used as a runtime value.
const user = new User(101, "Alex");
```

Use a normal import when the class constructor is required.

# 🗁 Importing Enums as Types

Enums are runtime values in TypeScript when declared as regular enums.

If you only need an enum's type, you can use a type-only import.

`user-role.ts`

```typescript
export enum UserRole {
  Admin = "ADMIN",
  Customer = "CUSTOMER",
  Guest = "GUEST",
}
```

`app.ts`

```typescript
import type { UserRole } from "./user-role";

function assignRole(role: UserRole): void {
  console.log(role);
}
```

Because the import is type-only, you cannot access enum members through it at runtime.

Incorrect:

```typescript
import type { UserRole } from "./user-role";

const role = UserRole.Admin;
```

Correct when accessing enum members:

```typescript
import { UserRole } from "./user-role";

const role = UserRole.Admin;
```

# 🗁 Inline Type-Only Imports

TypeScript also allows individual imports to be marked as type-only using the `type` modifier.

```typescript
import {
  type User,
  type UserRole,
  createUser,
} from "./user";
```

Here:

* `User` is imported only as a type.
* `UserRole` is imported only as a type.
* `createUser` is imported as a runtime value.

This syntax is useful when a module provides both types and runtime functionality.

# 🗁 Combining Default and Type-Only Imports

You can combine a default import with named type-only imports.

```typescript
import UserService, {
  type User,
  type UserRole,
} from "./user-service";
```

The default import is a runtime value, while `User` and `UserRole` are type-only imports.

Example:

```typescript
import UserService, {
  type User,
} from "./user-service";

const service = new UserService();

const user: User = service.getUser();
```

# 🗁 Default Type-Only Imports

A module can default-export a type declaration.

`user.ts`

```typescript
interface User {
  id: number;
  name: string;
}

export default User;
```

Import it using `import type`.

```typescript
import type User from "./user";

const user: User = {
  id: 101,
  name: "Alex",
};
```

This import does not create a runtime dependency.

# 🗁 Type-Only Imports with Aliases

You can rename imported types using `as`.

```typescript
import type {
  User as UserModel,
  UserRole as Role,
} from "./user.types";
```

Using the aliases:

```typescript
const user: UserModel = {
  id: 101,
  name: "Alex",
};

const role: Role = "admin";
```

This is useful when multiple modules export declarations with the same name.

# 🗁 Type-Only Imports and JavaScript Output

Type-only imports are removed during compilation.

For example:

```typescript
import type { User } from "./user";

const user: User = {
  id: 101,
  name: "Alex",
};

console.log(user);
```

The emitted JavaScript is conceptually:

```javascript
const user = {
    id: 101,
    name: "Alex"
};

console.log(user);
```

There is no JavaScript import for `User` because the interface is needed only during type checking.

The exact emitted output depends on the TypeScript compiler configuration and target.

# 🗁 Type-Only Imports vs Regular Imports

| Feature | Type-Only Import | Regular Import |
| --- | --- | --- |
| Syntax | `import type` | `import` |
| Purpose | Type information | Runtime values and types |
| JavaScript output | Import is erased | Runtime import may remain |
| Functions | Cannot import for runtime use | Can import and execute |
| Classes | Can use as types | Can use as types and values |
| Interfaces | Supported | Supported as type references |
| Type aliases | Supported | Supported as type references |
| Enum members | Cannot access at runtime | Can access at runtime |

# 🗁 Type-Only Imports and Module Dependencies

Type-only imports help distinguish dependencies needed for compilation from dependencies needed during execution.

Consider:

```typescript
import type { User } from "./user";
import { fetchUsers } from "./api";

export function displayUsers(): void {
  const users: User[] = fetchUsers();

  console.log(users);
}
```

The module has two different dependency requirements:

* `User` is needed only for type checking.
* `fetchUsers` is needed at runtime.

This distinction can make module dependencies easier to understand and maintain.

# 🗁 Circular Dependencies and Type-Only Imports

Type-only imports can help reduce runtime circular dependencies when modules depend on each other's types but do not need each other's runtime values.

Example:

`user.ts`

```typescript
import type { Order } from "./order";

export interface User {
  id: number;
  orders: Order[];
}
```

`order.ts`

```typescript
import type { User } from "./user";

export interface Order {
  id: number;
  customer: User;
}
```

Both modules reference each other's types.

Since these imports are type-only, they do not create corresponding runtime imports.

However, type-only imports do not automatically solve every circular dependency problem. Runtime dependencies and architectural coupling still need to be considered.

# 🗁 The `verbatimModuleSyntax` Compiler Option

The `verbatimModuleSyntax` compiler option makes import and export intent explicit.

When enabled:

* Imports marked with `type` are erased.
* Imports without `type` are preserved according to the module emission rules.
* Type-only declarations must be marked appropriately when they should not appear in JavaScript output.

Example:

```typescript
import type { User } from "./user";

import { createUser } from "./user";
```

This clearly separates type-only dependencies from runtime dependencies.

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

This option is useful in modern TypeScript projects that want predictable module syntax.

# 🗁 The `importsNotUsedAsValues` Option

Older TypeScript configurations used the `importsNotUsedAsValues` compiler option to influence how imports used only as types were handled.

This option was deprecated and removed in TypeScript 5.5.

Modern projects should use explicit type-only imports and consider `verbatimModuleSyntax` instead.

# 🗁 Best Practices

* Use `import type` when importing declarations exclusively for type checking.
* Use inline `type` modifiers when mixing runtime and type imports.
* Keep runtime dependencies separate from type-only dependencies.
* Use meaningful aliases when imported type names conflict.
* Use normal imports when accessing runtime values such as class constructors and enum members.
* Prefer explicit type-only imports in projects using `verbatimModuleSyntax`.
* Avoid assuming type-only imports solve all circular dependency problems.
* Keep module boundaries clear and dependencies intentional.

# 🗁 Summary

Type-only imports allow TypeScript declarations to be imported exclusively for compile-time type checking.

Key concepts:

* `import type` imports only types.
* Multiple types can be imported in one statement.
* Interfaces and type aliases can be imported as types.
* Classes can be imported as types when their runtime constructors are unnecessary.
* Regular enum members require runtime imports.
* Inline `type` modifiers allow mixed imports.
* Default-exported types can also be imported using `import type`.
* Type-only imports are erased from emitted JavaScript.
* `verbatimModuleSyntax` helps enforce explicit import and export intent.

Understanding type-only imports helps control runtime dependencies, improve module organization, and maintain clean TypeScript architecture.
