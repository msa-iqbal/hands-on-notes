# Mapped Types

Mapped types allow you to create new types by iterating over the properties of an existing type. They provide a powerful way to transform object types without manually defining every property.

They are commonly used to build reusable utility types, modify property modifiers, and derive new object structures from existing types.

# 🗁 Basic Syntax

A mapped type uses a `keyof` type operator and a property iteration syntax:

```typescript
type MappedType<T> = {
  [K in keyof T]: T[K];
};
```

Where:

* `T` is the source type.
* `keyof T` produces a union of the source type's property keys.
* `K` represents each property key during iteration.
* `T[K]` represents the type of the current property.

Example:

```typescript
type User = {
  id: number;
  name: string;
  email: string;
};

type UserCopy = {
  [K in keyof User]: User[K];
};
```

The resulting type is equivalent to:

```typescript
type UserCopy = {
  id: number;
  name: string;
  email: string;
};
```

# 🗁 Mapping Over Property Keys

You can use mapped types to transform every property of an existing type.

```typescript
type User = {
  id: number;
  name: string;
  email: string;
};

type UserPermissions = {
  [K in keyof User]: boolean;
};
```

Result:

```typescript
type UserPermissions = {
  id: boolean;
  name: boolean;
  email: boolean;
};
```

Every property retains its original key but receives a new value type.

# 🗁 Making Properties Readonly

Use the `readonly` modifier to make every mapped property immutable.

```typescript
type User = {
  id: number;
  name: string;
  email: string;
};

type ReadonlyUser = {
  readonly [K in keyof User]: User[K];
};
```

Usage:

```typescript
const user: ReadonlyUser = {
  id: 1,
  name: "John",
  email: "john@example.com"
};

// Error: Cannot assign to a readonly property.
user.name = "Alex";
```

You can also use the built-in `Readonly<T>` utility type:

```typescript
type ReadonlyUser = Readonly<User>;
```

# 🗁 Making Properties Optional

Use the `?` modifier to make every mapped property optional.

```typescript
type User = {
  id: number;
  name: string;
  email: string;
};

type OptionalUser = {
  [K in keyof User]?: User[K];
};
```

Result:

```typescript
type OptionalUser = {
  id?: number;
  name?: string;
  email?: string;
};
```

You can also use the built-in `Partial<T>` utility type:

```typescript
type OptionalUser = Partial<User>;
```

# 🗁 Removing Property Modifiers

Mapped types support removing existing property modifiers.

Use `-readonly` to remove `readonly`:

```typescript
type ReadonlyUser = {
  readonly id: number;
  readonly name: string;
};

type MutableUser = {
  -readonly [K in keyof ReadonlyUser]: ReadonlyUser[K];
};
```

Result:

```typescript
type MutableUser = {
  id: number;
  name: string;
};
```

Use `-?` to remove optional modifiers:

```typescript
type OptionalUser = {
  id?: number;
  name?: string;
};

type RequiredUser = {
  [K in keyof OptionalUser]-?: OptionalUser[K];
};
```

Result:

```typescript
type RequiredUser = {
  id: number;
  name: string;
};
```

You can combine modifiers:

```typescript
type NormalizedUser<T> = {
  -readonly [K in keyof T]-?: T[K];
};
```

This removes both `readonly` and optional modifiers.

# 🗁 Adding Property Modifiers

You can explicitly add both `readonly` and optional modifiers.

```typescript
type User = {
  id: number;
  name: string;
};

type ReadonlyOptionalUser = {
  +readonly [K in keyof User]+?: User[K];
};
```

The `+` modifier is optional because adding modifiers is the default behavior when specified without a sign.

# 🗁 Key Remapping with `as`

TypeScript supports transforming property keys using the `as` clause.

```typescript
type User = {
  id: number;
  name: string;
};

type PrefixedUser = {
  [K in keyof User as `user_${K & string}`]: User[K];
};
```

Result:

```typescript
type PrefixedUser = {
  user_id: number;
  user_name: string;
};
```

The `as` clause lets you create new property names from existing keys.

# 🗁 Filtering Properties

You can use key remapping to remove properties by mapping unwanted keys to `never`.

```typescript
type User = {
  id: number;
  name: string;
  password: string;
  email: string;
};

type PublicUser = {
  [K in keyof User as K extends "password" ? never : K]: User[K];
};
```

Result:

```typescript
type PublicUser = {
  id: number;
  name: string;
  email: string;
};
```

This technique is useful for excluding sensitive or unnecessary properties from an object type.

# 🗁 Filtering Properties by Value Type

Mapped types can filter properties based on their value types.

```typescript
type User = {
  id: number;
  name: string;
  active: boolean;
  email: string;
};

type StringKeys<T> = {
  [K in keyof T as T[K] extends string ? K : never]: T[K];
};

type UserStringProperties = StringKeys<User>;
```

Result:

```typescript
type UserStringProperties = {
  name: string;
  email: string;
};
```

The condition checks whether each property's type extends `string`. Only matching keys are retained.

# 🗁 Creating Getter Methods

You can generate getter method names from an existing object's properties.

```typescript
type User = {
  id: number;
  name: string;
  active: boolean;
};

type Getters<T> = {
  [K in keyof T as `get${Capitalize<K & string>}`]: () => T[K];
};
```

Result:

```typescript
type UserGetters = Getters<User>;

/*
{
  getId: () => number;
  getName: () => string;
  getActive: () => boolean;
}
*/
```

TypeScript provides intrinsic string manipulation types such as:

* `Uppercase<T>`
* `Lowercase<T>`
* `Capitalize<T>`
* `Uncapitalize<T>`

These can be combined with key remapping to generate structured property names.

# 🗁 Mapping Over Unions

Mapped types can also iterate over unions of property keys.

```typescript
type Status = "pending" | "success" | "failed";

type StatusMessages = {
  [K in Status]: string;
};
```

Result:

```typescript
type StatusMessages = {
  pending: string;
  success: string;
  failed: string;
};
```

This is useful when creating objects whose keys must correspond to a specific union of string literals.

# 🗁 Generic Mapped Types

Mapped types become especially useful when combined with generics.

```typescript
type Nullable<T> = {
  [K in keyof T]: T[K] | null;
};
```

Usage:

```typescript
type User = {
  id: number;
  name: string;
  email: string;
};

type NullableUser = Nullable<User>;
```

Result:

```typescript
type NullableUser = {
  id: number | null;
  name: string | null;
  email: string | null;
};
```

The mapped type works with different object structures:

```typescript
type Product = {
  id: number;
  title: string;
  price: number;
};

type NullableProduct = Nullable<Product>;
```

No additional implementation is required for each new type.

# 🗁 Built-in Utility Types Based on Mapped Types

Several built-in TypeScript utility types use mapped type patterns.

| Utility type | Purpose |
| --- | --- |
| `Partial<T>` | Makes all properties optional |
| `Required<T>` | Makes all properties required |
| `Readonly<T>` | Makes all properties readonly |
| `Pick<T, K>` | Selects specified properties |
| `Record<K, T>` | Creates an object type with specified keys and value types |

Example:

```typescript
type User = {
  id: number;
  name: string;
  email?: string;
};

type PartialUser = Partial<User>;
type RequiredUser = Required<User>;
type ReadonlyUser = Readonly<User>;
type BasicUser = Pick<User, "id" | "name">;
```

Understanding mapped types makes these utility types easier to understand and implement yourself.

# 🗁 Practical Example: Form State

Mapped types can generate form state from a data model.

```typescript
type RegistrationForm = {
  username: string;
  email: string;
  password: string;
};

type FormErrors<T> = {
  [K in keyof T]?: string;
};

type FormValues<T> = {
  [K in keyof T]: T[K] | "";
};

type RegistrationErrors = FormErrors<RegistrationForm>;

type RegistrationValues = FormValues<RegistrationForm>;
```

Result:

```typescript
type RegistrationErrors = {
  username?: string;
  email?: string;
  password?: string;
};

type RegistrationValues = {
  username: string;
  email: string;
  password: string;
};
```

This approach keeps form-related types synchronized with the original data model.

# 🗁 Practical Example: API Update Payload

An update operation often allows clients to modify only selected properties.

```typescript
type User = {
  id: number;
  name: string;
  email: string;
  role: "admin" | "user";
};

type UpdatePayload<T> = {
  [K in keyof T]?: T[K];
};

type UserUpdatePayload = UpdatePayload<User>;
```

Usage:

```typescript
const updateUser: UserUpdatePayload = {
  name: "Alex",
  email: "alex@example.com"
};
```

Every property is optional, but its original value type is preserved.

# 🗁 Homomorphic Mapped Types

A homomorphic mapped type transforms properties directly from an existing type.

Example:

```typescript
type Copy<T> = {
  [K in keyof T]: T[K];
};
```

When mapping directly over `keyof T`, TypeScript preserves property characteristics such as optional and readonly modifiers unless you explicitly change them.

```typescript
type User = {
  readonly id: number;
  name?: string;
};

type UserCopy = Copy<User>;
```

Result:

```typescript
type UserCopy = {
  readonly id: number;
  name?: string;
};
```

This behavior is important when building generic utility types because it allows transformations to preserve the original structure.

# 🗁 Best Practices

* Use mapped types when transforming multiple properties of an existing object type.
* Prefer built-in utility types when they already solve the problem.
* Use descriptive generic names such as `T`, `K`, and `Value` consistently.
* Use key remapping when generating or filtering property names.
* Use `never` to remove unwanted properties from mapped results.
* Avoid deeply nested mapped types when a simpler type definition is easier to understand.
* Keep complex transformations reusable by extracting them into named utility types.
* Remember that mapped types operate at compile time and do not transform JavaScript objects at runtime.

# 🗁 Summary

Mapped types allow you to programmatically transform object types by iterating over their property keys.

Key concepts:

* `[K in keyof T]` iterates over property keys.
* `T[K]` accesses the corresponding property type.
* `readonly` adds immutability.
* `?` makes properties optional.
* `-readonly` and `-?` remove modifiers.
* `as` remaps property keys.
* `never` removes properties from mapped results.
* Generic mapped types make reusable type transformations possible.
