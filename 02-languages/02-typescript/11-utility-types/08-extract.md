# Extract Utility Type

The `Extract<UnionType, UnionMembers>` utility type constructs a new union type by selecting only those members of an existing union that are assignable to another type.

It is the opposite of `Exclude`:

* `Exclude` removes matching union members.
* `Extract` keeps matching union members.

It is useful when you need to derive a specific subset of an existing union without manually rewriting its members.

# 🗁 Syntax

```typescript
Extract<UnionType, UnionMembers>
```

* `UnionType`: The original union type.
* `UnionMembers`: The type or union of types to select.
* Result: A union containing only members of `UnionType` that are assignable to `UnionMembers`.

# 🗁 Basic Example

```typescript
type Status = "pending" | "approved" | "rejected";

type SuccessfulStatus = Extract<Status, "approved" | "pending">;

// Result:
// "pending" | "approved"
```

Only `"pending"` and `"approved"` are retained because they match the specified members.

# 🗁 Extracting Multiple Members

You can extract multiple members by providing a union as the second argument.

```typescript
type Permission =
  | "create"
  | "read"
  | "update"
  | "delete"
  | "manage";

type ReadWritePermissions = Extract<
  Permission,
  "read" | "update"
>;

// Result:
// "read" | "update"
```

# 🗁 Extracting Numeric Literal Types

`Extract` works with numeric literal unions as well.

```typescript
type ResponseCode = 200 | 201 | 301 | 400 | 401 | 404 | 500;

type ClientErrorCode = Extract<
  ResponseCode,
  400 | 401 | 404
>;

// Result:
// 400 | 401 | 404
```

# 🗁 Extracting Object Types

You can extract specific object variants from a union.

This is particularly useful with discriminated unions.

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

type CardPayment = Extract<
  Payment,
  { type: "card" }
>;

// Result:
// {
//   type: "card";
//   cardNumber: string;
// }
```

The resulting type contains only the card payment variant.

# 🗁 Extracting Multiple Object Variants

You can extract more than one object variant at a time.

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

type ElectronicPayment = Extract<
  Payment,
  { type: "card" } | { type: "bank" }
>;

// Result:
// Card payment | Bank payment
```

# 🗁 Using Extract with keyof

`Extract` can filter property names obtained through `keyof`.

```typescript
interface User {
  id: number;
  name: string;
  email: string;
  password: string;
}

type UserKeys = keyof User;

type PublicKeys = Extract<
  UserKeys,
  "id" | "name" | "email"
>;

// Result:
// "id" | "name" | "email"
```

You can also use it to construct another object type.

```typescript
type PublicUser = Pick<User, PublicKeys>;

// Result:
// {
//   id: number;
//   name: string;
//   email: string;
// }
```

# 🗁 Extracting Function Types

`Extract` can select function signatures from a union of types.

```typescript
type Handler =
  | string
  | number
  | (() => void)
  | ((value: string) => boolean);

type FunctionHandler = Extract<
  Handler,
  (...args: any[]) => any
>;

// Result:
// (() => void) | ((value: string) => boolean)
```

The function constraint selects only callable members.

# 🗁 Extracting Members by Structural Compatibility

TypeScript uses structural typing when determining whether a type is assignable to another type.

Consider this example:

```typescript
type Animal =
  | {
      name: string;
      legs: number;
    }
  | {
      name: string;
      wings: number;
    }
  | {
      name: string;
      fins: number;
    };

type FlyingAnimal = Extract<
  Animal,
  { wings: number }
>;

// Result:
// {
//   name: string;
//   wings: number;
// }
```

The selected member must satisfy the structure specified in the second argument.

# 🗁 How Extract Works Internally

The built-in `Extract` utility type is defined using a distributive conditional type.

Its simplified definition is:

```typescript
type Extract<T, U> = T extends U ? T : never;
```

Consider:

```typescript
type Color = "red" | "green" | "blue";

type Result = Extract<Color, "green" | "blue">;
```

TypeScript evaluates each union member separately:

```typescript
type Result =
  | ("red" extends "green" | "blue" ? "red" : never)
  | ("green" extends "green" | "blue" ? "green" : never)
  | ("blue" extends "green" | "blue" ? "blue" : never);
```

The evaluation produces:

```typescript
type Result = never | "green" | "blue";
```

Since `never` disappears from unions, the final result is:

```typescript
type Result = "green" | "blue";
```

# 🗁 Extract vs Exclude

Both utilities operate on union members, but they perform opposite operations.

| Utility Type | Behavior |
| --- | --- |
| `Extract<T, U>` | Keeps members of `T` assignable to `U` |
| `Exclude<T, U>` | Removes members of `T` assignable to `U` |

Example:

```typescript
type Role =
  | "admin"
  | "editor"
  | "author"
  | "viewer";

type AdministrativeRole = Extract<
  Role,
  "admin" | "editor"
>;

type PublicRole = Exclude<
  Role,
  "admin" | "editor"
>;

// AdministrativeRole:
// "admin" | "editor"

// PublicRole:
// "author" | "viewer"
```

# 🗁 Important Behavior

**Extract uses assignability, not arbitrary overlap.**

```typescript
type Value = string | number | boolean;

type Result = Extract<Value, string | number>;

// Result:
// string | number
```

However, extracting a string literal from the broad `string` type does not produce that literal.

```typescript
type Result = Extract<string, "admin">;

// Result:
// never
```

This happens because `string` is not assignable to the narrower literal type `"admin"`.

**Extract does not modify the original union.**

```typescript
type Original = "read" | "write" | "delete";

type ReadAction = Extract<Original, "read">;

// Original remains unchanged.
```

**Extract returns `never` when no members match.**

```typescript
type Status = "pending" | "approved" | "rejected";

type Result = Extract<Status, "completed">;

// Result:
// never
```

The resulting type has no possible values.

# 🗁 Practical Example: Filtering API Events

Suppose an application supports several event types, but a particular function should process only user-related events.

```typescript
type AppEvent =
  | {
      type: "user.created";
      userId: number;
    }
  | {
      type: "user.deleted";
      userId: number;
    }
  | {
      type: "order.created";
      orderId: number;
    }
  | {
      type: "payment.completed";
      paymentId: number;
    };

type UserEvent = Extract<
  AppEvent,
  { type: "user.created" | "user.deleted" }
>;

function handleUserEvent(event: UserEvent): void {
  console.log(event.type, event.userId);
}

handleUserEvent({
  type: "user.created",
  userId: 101,
});

// TypeScript error:
// An order or payment event is not assignable
// to the UserEvent type.
```

The `UserEvent` type contains only user-related event variants.

# 🗁 Best Practices

* Use `Extract` to select specific members from an existing union.
* Prefer deriving types instead of duplicating union members.
* Use discriminated unions to extract specific object variants.
* Combine `Extract` with `keyof` when filtering property names.
* Use `Extract` with function constraints to select callable types.
* Remember that matching is based on TypeScript assignability.
* Use descriptive names for extracted types.
* Use `Exclude` when you need the opposite behavior.

# 🗁 Summary

* `Extract<T, U>` keeps union members of `T` that are assignable to `U`.
* It works with string literals, numeric literals, object types, and function types.
* It is especially useful with discriminated unions.
* It is implemented using distributive conditional types.
* It returns `never` when no members match.
* It is the logical opposite of `Exclude<T, U>`.
