# keyof Operator

The `keyof` operator in TypeScript creates a union of the property keys of an object type.

It allows you to work with object keys in a type-safe way without manually writing each property name.

It is commonly used in:

* Generic functions.
* Object property access.
* Reusable utility types.
* Type-safe object manipulation.
* Form handling.
* API response processing.
* Repository and data-access layers.
* Generic libraries and frameworks.

# 🗁 Basic Syntax

```typescript
type Keys = keyof ObjectType;
```

Example:

```typescript
type User = {
  id: number;
  name: string;
  email: string;
};

type UserKeys = keyof User;
```

The resulting type is:

```typescript
type UserKeys = "id" | "name" | "email";
```

`keyof User` produces a union containing all the known property keys of `User`.

# 🗁 keyof with Object Types

Consider an object type containing several properties.

```typescript
type Product = {
  id: number;
  title: string;
  price: number;
  available: boolean;
};

type ProductKeys = keyof Product;
```

The resulting type is:

```typescript
type ProductKeys =
  | "id"
  | "title"
  | "price"
  | "available";
```

You can use this type to restrict variables to valid property names.

```typescript
let key: keyof Product;

key = "id";
key = "title";
key = "price";

// TypeScript error
// key = "description";
```

# 🗁 keyof with Interfaces

The `keyof` operator works with interfaces just as it works with type aliases.

```typescript
interface User {
  id: number;
  name: string;
  active: boolean;
}

type UserKeys = keyof User;
```

Equivalent result:

```typescript
type UserKeys = "id" | "name" | "active";
```

This makes `keyof` useful with reusable interface-based models.

# 🗁 keyof with Arrays

The `keyof` operator can also be applied to array types.

```typescript
type ArrayKeys = keyof string[];
```

The result includes array property and method names, such as:

```typescript
type ArrayKeys =
  | number
  | "length"
  | "push"
  | "pop"
  | "map"
  | "filter"
  | "find"
  | "forEach"
  | "slice"
  | "splice"
  | ...;
```

The exact set includes the keys provided by the applicable array type definitions and inherited members.

For tuples, `keyof` includes their numeric index type and array-related properties.

```typescript
type UserTuple = [string, number];

type TupleKeys = keyof UserTuple;
```

# 🗁 keyof with Index Signatures

Index signatures describe types that allow dynamic property keys.

For a string index signature:

```typescript
type Dictionary = {
  [key: string]: string;
};

type DictionaryKeys = keyof Dictionary;
```

The resulting type is:

```typescript
type DictionaryKeys = string | number;
```

The `number` is included because JavaScript object property keys can be accessed using numeric values, which are converted to strings.

For a numeric index signature:

```typescript
type NumberDictionary = {
  [key: number]: string;
};

type NumberDictionaryKeys = keyof NumberDictionary;
```

The result is:

```typescript
type NumberDictionaryKeys = number;
```

For both string and symbol index signatures:

```typescript
type MixedDictionary = {
  [key: string]: string;
  [key: symbol]: string;
};

type MixedDictionaryKeys = keyof MixedDictionary;
```

The resulting type is:

```typescript
type MixedDictionaryKeys = string | number | symbol;
```

# 🗁 keyof with Union Types

When applied to a union type, `keyof` returns keys that are safely available on every member of the union.

```typescript
type User = {
  id: number;
  name: string;
};

type Admin = {
  id: number;
  role: string;
};

type CommonKeys = keyof (User | Admin);
```

The result is:

```typescript
type CommonKeys = "id";
```

Only `id` exists on both types.

This behavior ensures that property access is safe regardless of which union member is present.

# 🗁 keyof with Intersection Types

When applied to an intersection type, `keyof` includes the keys from the combined type.

```typescript
type User = {
  id: number;
  name: string;
};

type Account = {
  email: string;
  active: boolean;
};

type UserAccount = User & Account;

type UserAccountKeys = keyof UserAccount;
```

The resulting type is:

```typescript
type UserAccountKeys =
  | "id"
  | "name"
  | "email"
  | "active";
```

Intersection types combine the properties of their constituent types.

# 🗁 keyof with Generic Constraints

One of the most important applications of `keyof` is restricting a generic parameter to valid object keys.

```typescript
function getProperty<T, K extends keyof T>(
  object: T,
  key: K,
): T[K] {
  return object[key];
}
```

Usage:

```typescript
const user = {
  id: 1,
  name: "Alice",
  active: true,
};

const id = getProperty(user, "id");
const name = getProperty(user, "name");
const active = getProperty(user, "active");
```

TypeScript infers:

```typescript
// id: number
// name: string
// active: boolean
```

Invalid property access:

```typescript
// TypeScript error
// getProperty(user, "email");
```

The constraint `K extends keyof T` ensures that `K` must be a valid key of `T`.

The indexed access type `T[K]` retrieves the corresponding property type.

# 🗁 keyof with Indexed Access Types

`keyof` is frequently combined with indexed access types.

```typescript
type User = {
  id: number;
  name: string;
  active: boolean;
};

type UserValue = User[keyof User];
```

The resulting type is:

```typescript
type UserValue = number | string | boolean;
```

Explanation:

* `keyof User` produces `"id" | "name" | "active"`.
* `User[keyof User]` retrieves the type of each corresponding property.
* The resulting type is a union of all property value types.

You can also retrieve an individual property type.

```typescript
type UserName = User["name"];
type UserId = User["id"];
```

Results:

```typescript
// UserName: string
// UserId: number
```

# 🗁 keyof with Pick

The `Pick` utility type uses the same key constraint pattern.

```typescript
type User = {
  id: number;
  name: string;
  email: string;
  active: boolean;
};

type PublicUser = Pick<User, "id" | "name">;
```

Result:

```typescript
type PublicUser = {
  id: number;
  name: string;
};
```

The second type parameter of `Pick` is constrained to `keyof User`.

You can build a similar utility type yourself:

```typescript
type MyPick<T, K extends keyof T> = {
  [P in K]: T[P];
};
```

Usage:

```typescript
type UserPreview = MyPick<User, "id" | "name">;
```

This demonstrates how `keyof`, indexed access types, and mapped types work together.

# 🗁 keyof with Omit

The `Omit` utility type uses keys to remove selected properties.

```typescript
type User = {
  id: number;
  name: string;
  email: string;
  password: string;
};

type SafeUser = Omit<User, "password">;
```

Result:

```typescript
type SafeUser = {
  id: number;
  name: string;
  email: string;
};
```

You can implement a similar utility type using `keyof` and mapped types.

```typescript
type MyOmit<T, K extends keyof any> = {
  [P in keyof T as P extends K ? never : P]: T[P];
};
```

This implementation uses key remapping to exclude selected properties.

# 🗁 keyof with typeof

The `typeof` operator can obtain the type of a JavaScript value, while `keyof` extracts its keys.

Together, they are useful for deriving types directly from existing objects.

```typescript
const user = {
  id: 1,
  name: "Alice",
  active: true,
};

type User = typeof user;

type UserKeys = keyof typeof user;
```

The resulting types are:

```typescript
type User = {
  id: number;
  name: string;
  active: boolean;
};

type UserKeys = "id" | "name" | "active";
```

This avoids duplicating object structures in type declarations.

# 🗁 keyof with as const

The `as const` assertion preserves literal values and makes object properties readonly.

```typescript
const roles = {
  ADMIN: "admin",
  EDITOR: "editor",
  USER: "user",
} as const;
```

Extract the property names:

```typescript
type RoleKey = keyof typeof roles;
```

Result:

```typescript
type RoleKey = "ADMIN" | "EDITOR" | "USER";
```

Extract the property values:

```typescript
type RoleValue = (typeof roles)[keyof typeof roles];
```

Result:

```typescript
type RoleValue = "admin" | "editor" | "user";
```

This pattern is particularly useful for application constants, configuration objects, and role definitions.

# 🗁 keyof with Symbol Keys

Object property keys can be strings, numbers, or symbols.

```typescript
const idSymbol = Symbol("id");

type User = {
  name: string;
  age: number;
  [idSymbol]: string;
};

type UserKeys = keyof User;
```

The resulting type includes:

```typescript
type UserKeys =
  | "name"
  | "age"
  | typeof idSymbol;
```

The symbol key remains a unique symbol type rather than becoming a string.

# 🗁 keyof with Optional and Readonly Properties

Optional and readonly modifiers do not prevent properties from appearing in `keyof`.

```typescript
type User = {
  readonly id: number;
  name: string;
  email?: string;
};

type UserKeys = keyof User;
```

Result:

```typescript
type UserKeys = "id" | "name" | "email";
```

All declared properties are included regardless of their modifiers.

# 🗁 Practical Example: Type-Safe Configuration

Consider an application configuration object.

```typescript
const config = {
  appName: "My Application",
  version: "1.0.0",
  debug: true,
  port: 3000,
};

type ConfigKey = keyof typeof config;
```

Create a reusable getter:

```typescript
function getConfig<K extends ConfigKey>(
  key: K,
): (typeof config)[K] {
  return config[key];
}
```

Usage:

```typescript
const appName = getConfig("appName");
const debug = getConfig("debug");
const port = getConfig("port");
```

Inferred types:

```typescript
// appName: string
// debug: boolean
// port: number
```

Invalid key:

```typescript
// TypeScript error
// getConfig("database");
```

This approach provides type-safe access to configuration properties.

# 🗁 Practical Example: Type-Safe Property Updates

You can use `keyof` to build a generic function that updates object properties.

```typescript
function updateProperty<T, K extends keyof T>(
  object: T,
  key: K,
  value: T[K],
): void {
  object[key] = value;
}
```

Usage:

```typescript
const user = {
  id: 1,
  name: "Alice",
  active: true,
};

updateProperty(user, "name", "Bob");
updateProperty(user, "active", false);
updateProperty(user, "id", 2);
```

Invalid value:

```typescript
// TypeScript error
// updateProperty(user, "id", "two");
```

The value must match the type of the selected property.

# 🗁 Practical Example: Filtering Object Keys

You can create a generic helper that returns an object's keys with a type-safe result.

```typescript
function getObjectKeys<T extends object>(
  object: T,
): (keyof T)[] {
  return Object.keys(object) as (keyof T)[];
}
```

Usage:

```typescript
const user = {
  id: 1,
  name: "Alice",
  active: true,
};

const keys = getObjectKeys(user);
```

The resulting static type is:

```typescript
(keyof typeof user)[]
```

The assertion is necessary because JavaScript's `Object.keys()` returns `string[]`, while TypeScript cannot guarantee that every runtime key corresponds exactly to the statically declared keys.

# 🗁 keyof vs typeof

These operators serve different purposes.

| Operator | Purpose | Example |
| --- | --- | --- |
| `keyof` | Extracts property keys from a type | `keyof User` |
| `typeof` | Obtains the type of a value | `typeof user` |
| `keyof typeof` | Extracts keys from a value's type | `keyof typeof user` |

Example:

```typescript
const product = {
  id: 1,
  title: "Keyboard",
  price: 75,
};

type Product = typeof product;

type ProductKeys = keyof Product;

type ProductValues = Product[keyof Product];
```

Results:

```typescript
// Product: object type inferred from product
// ProductKeys: "id" | "title" | "price"
// ProductValues: number | string
```

# 🗁 Common Mistakes

**Using keyof on a value directly**

```typescript
const user = {
  id: 1,
  name: "Alice",
};

// TypeScript error
// type Keys = keyof user;
```

Correct:

```typescript
type Keys = keyof typeof user;
```

`keyof` operates on types, not directly on runtime values.

**Assuming keyof returns property values**

```typescript
type User = {
  id: number;
  name: string;
};

type Keys = keyof User;
```

The result is `"id" | "name"`, not `number | string`.

To obtain property value types:

```typescript
type Values = User[keyof User];
```

**Assuming keyof a union returns every possible key**

```typescript
type A = {
  id: number;
  name: string;
};

type B = {
  id: number;
  email: string;
};

type Keys = keyof (A | B);
```

The result is only `"id"` because it is the key safely available on both members.

**Confusing keyof with Object.keys**

`keyof` is a compile-time type operator. `Object.keys()` is a runtime JavaScript function.

They operate at different stages and have different behavior.

**Ignoring numeric and symbol keys**

`keyof` is not limited to string keys. It can also represent numeric and symbol property keys.

# 🗁 Best Practices

* Use `keyof` to derive property-key unions instead of manually duplicating them.
* Combine `keyof` with generic constraints for type-safe object access.
* Use `typeof` when deriving types from existing JavaScript values.
* Combine `keyof` with indexed access types to retrieve property value types.
* Use `keyof` with mapped types to build reusable utilities.
* Remember that `keyof` on a union returns keys common to all union members.
* Remember that `keyof` on an intersection combines available keys.
* Account for string, number, and symbol keys.
* Do not confuse compile-time key types with runtime object enumeration.
* Avoid unnecessary type assertions when a generic constraint can provide the required safety.

# 🗁 Summary

* `keyof` extracts the property keys of an object type.
* Its result is a union of property-key types.
* It works with interfaces, type aliases, arrays, tuples, and index signatures.
* It can be combined with generic constraints.
* `T[K]` retrieves the property type associated with a key.
* `keyof` works with union and intersection types.
* `keyof typeof value` derives keys from an existing JavaScript value.
* It is a fundamental building block for mapped types and utility types.
* It helps create reusable and type-safe APIs.

# 🗁 Related Topics

* Generic Basics
* Generic Functions
* Generic Constraints
* typeof Operator
* Indexed Access Types
* Mapped Types
* Conditional Types
* Pick
* Omit
* Record
* Type Inference
* Structural Typing
