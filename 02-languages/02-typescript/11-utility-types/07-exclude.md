# Exclude Utility Type

The `Exclude<UnionType, ExcludedMembers>` utility type constructs a new union type by removing members from an existing union that are assignable to another type.

It is useful when you want to derive a more specific union from an existing one without manually rewriting every remaining member.

# 🗁 Syntax

```typescript
Exclude<UnionType, ExcludedMembers>
```

* `UnionType`: The original union type.
* `ExcludedMembers`: The type or union of types to remove.
* Result: A union containing only members that are not assignable to `ExcludedMembers`.

# 🗁 Basic Example

```typescript
type Status = "pending" | "approved" | "rejected";

type ActiveStatus = Exclude<Status, "rejected">;

// Result:
// "pending" | "approved"
```

The `"rejected"` member is removed from the original union.

# 🗁 Excluding Multiple Members

You can provide a union of members to exclude.

```typescript
type Status =
  | "pending"
  | "approved"
  | "rejected"
  | "cancelled";

type FinalStatus = Exclude<
  Status,
  "rejected" | "cancelled"
>;

// Result:
// "pending" | "approved"
```

Both `"rejected"` and `"cancelled"` are removed.

# 🗁 Working with String Literal Unions

`Exclude` is particularly useful for creating subsets of string literal unions.

```typescript
type Role =
  | "admin"
  | "editor"
  | "author"
  | "viewer";

type PublicRole = Exclude<Role, "admin">;

// Result:
// "editor" | "author" | "viewer"
```

This allows you to reuse an existing type while defining a more restricted set of allowed values.

# 🗁 Working with Numeric Unions

`Exclude` also works with numeric literal types.

```typescript
type ResponseCode = 200 | 201 | 400 | 401 | 404 | 500;

type SuccessCode = Exclude<
  ResponseCode,
  400 | 401 | 404 | 500
>;

// Result:
// 200 | 201
```

# 🗁 Excluding Types from Object Unions

You can use `Exclude` to remove specific object variants from a discriminated union.

```typescript
type Payment =
  | {
      type: "card";
      cardNumber: string;
    }
  | {
      type: "cash";
      amount: number;
    }
  | {
      type: "bank";
      accountNumber: string;
    };

type NonCashPayment = Exclude<
  Payment,
  { type: "cash" }
>;

// Result:
// Card payment | Bank payment
```

The resulting type contains only the card and bank payment variants.

# 🗁 Using Exclude with keyof

`Exclude` can filter property names obtained through `keyof`.

```typescript
interface User {
  id: number;
  name: string;
  email: string;
  password: string;
}

type PublicUserKeys = Exclude<
  keyof User,
  "password"
>;

// Result:
// "id" | "name" | "email"
```

You can use the resulting keys to construct other types.

```typescript
type PublicUser = Pick<User, PublicUserKeys>;

// Result:
// {
//   id: number;
//   name: string;
//   email: string;
// }
```

# 🗁 Creating Custom Utility Types

You can build custom utility types by combining `Exclude` with other TypeScript features.

For example, create a type that removes a specified property from an object:

```typescript
type RemoveProperty<T, K extends keyof T> = Omit<T, K>;

interface Product {
  id: number;
  name: string;
  price: number;
  internalCode: string;
}

type PublicProduct = RemoveProperty<Product, "internalCode">;
```

Although `Omit` already provides this functionality, understanding how utility types combine helps you design more flexible types.

# 🗁 How Exclude Works Internally

The built-in `Exclude` utility type is defined using a distributive conditional type.

Its simplified definition is:

```typescript
type Exclude<T, U> = T extends U ? never : T;
```

Consider this example:

```typescript
type Color = "red" | "green" | "blue";

type Result = Exclude<Color, "green">;
```

TypeScript evaluates each union member separately:

```typescript
type Result =
  | ("red" extends "green" ? never : "red")
  | ("green" extends "green" ? never : "green")
  | ("blue" extends "green" ? never : "blue");
```

The evaluation produces:

```typescript
type Result = "red" | never | "blue";
```

Since `never` represents an impossible value and disappears from unions, the final result is:

```typescript
type Result = "red" | "blue";
```

# 🗁 Important Behavior

**Exclude removes assignable members, not arbitrary overlapping types.**

```typescript
type Value = string | number | boolean;

type Result = Exclude<Value, string>;

// Result:
// number | boolean
```

However, excluding a string literal from the broad `string` type does not create a string type that excludes that literal.

```typescript
type Result = Exclude<string, "admin">;

// Result:
// string
```

This happens because `string` is not assignable to the narrower literal type `"admin"`.

**Exclude does not modify the original type.**

```typescript
type Original = "read" | "write" | "delete";

type ReadOnly = Exclude<Original, "write" | "delete">;

// Original remains unchanged.
```

# 🗁 Exclude vs Pick vs Omit

| Utility Type | Purpose |
| --- | --- |
| `Exclude<T, U>` | Removes union members assignable to `U` |
| `Pick<T, K>` | Selects specified object properties |
| `Omit<T, K>` | Removes specified object properties |

Example:

```typescript
type Actions = "create" | "read" | "update" | "delete";

type ReadActions = Exclude<Actions, "create" | "update" | "delete">;

interface User {
  id: number;
  name: string;
  password: string;
}

type UserWithoutPassword = Omit<User, "password">;

type UserIdentity = Pick<User, "id" | "name">;
```

# 🗁 Practical Example: API Permissions

Suppose an application defines several permissions, but some permissions should not be available to guest users.

```typescript
type Permission =
  | "view"
  | "create"
  | "update"
  | "delete"
  | "manage-users";

type GuestPermission = Exclude<
  Permission,
  "create" | "update" | "delete" | "manage-users"
>;

function checkGuestPermission(
  permission: GuestPermission
): boolean {
  return permission === "view";
}

checkGuestPermission("view");

// TypeScript error:
// Argument of type '"delete"' is not assignable
// to parameter of type '"view"'.
```

The `GuestPermission` type allows only the `"view"` permission.

# 🗁 Best Practices

* Use `Exclude` when deriving a union with fewer possible members.
* Reuse existing union types instead of duplicating their members.
* Combine `Exclude` with `keyof` to filter property names.
* Use `Exclude` with discriminated unions to remove specific variants.
* Prefer built-in utility types when they already solve the problem.
* Remember that `Exclude` operates on assignability, not on partial overlap.
* Use descriptive names for derived types so their restrictions are clear.

# 🗁 Summary

* `Exclude<T, U>` removes members from `T` that are assignable to `U`.
* It works with string literals, numeric literals, primitive unions, and object unions.
* It can filter keys obtained through `keyof`.
* It is implemented using distributive conditional types.
* It does not remove a literal value from a broad type such as `string`.
* It is useful for deriving restricted types without rewriting existing unions.
