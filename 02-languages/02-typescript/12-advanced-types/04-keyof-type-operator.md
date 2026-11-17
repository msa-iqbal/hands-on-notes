# Keyof Type Operator

The `keyof` type operator in TypeScript extracts the property keys of an object type and produces a union of string, number, or symbol literal types.

It is commonly used with generics, mapped types, and indexed access types to create reusable and type-safe abstractions.

# 🗁 Basic Syntax

The basic syntax is:

```typescript
type Keys = keyof Type;
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

Result:

```typescript
type UserKeys = "id" | "name" | "email";
```

The `keyof` operator extracts all property keys from the specified type.

# 🗁 Extracting Object Property Keys

Consider an object type with different property types:

```typescript
type Product = {
  id: number;
  title: string;
  price: number;
  available: boolean;
};

type ProductKeys = keyof Product;
```

Result:

```typescript
type ProductKeys =
  | "id"
  | "title"
  | "price"
  | "available";
```

You can use this union to restrict values to valid property names.

```typescript
let productKey: keyof Product;

productKey = "id";
productKey = "title";
productKey = "price";

// Error: "category" is not a valid property.
productKey = "category";
```

# 🗁 Using `keyof` with Variables

The `keyof` operator works with types, not directly with JavaScript values.

Use `typeof` to obtain the type of a variable before applying `keyof`.

```typescript
const user = {
  id: 1,
  name: "John",
  email: "john@example.com"
};

type User = typeof user;

type UserKeys = keyof User;
```

Result:

```typescript
type UserKeys = "id" | "name" | "email";
```

This is useful when deriving types from existing objects instead of defining them separately.

# 🗁 Using `keyof` with Generic Types

`keyof` becomes especially useful when working with generic types.

```typescript
type PropertyKey<T> = keyof T;

type User = {
  id: number;
  name: string;
  email: string;
};

type UserKeys = PropertyKey<User>;
```

Result:

```typescript
type UserKeys = "id" | "name" | "email";
```

You can create reusable utilities that work with different object structures.

# 🗁 Restricting Generic Function Parameters

A common use of `keyof` is restricting a function parameter to valid object property names.

```typescript
function getProperty<T, K extends keyof T>(
  object: T,
  key: K
): T[K] {
  return object[key];
}
```

Usage:

```typescript
const user = {
  id: 1,
  name: "John",
  active: true
};

const id = getProperty(user, "id");
const name = getProperty(user, "name");
const active = getProperty(user, "active");
```

TypeScript infers the return types:

```typescript
// number
const id = getProperty(user, "id");

// string
const name = getProperty(user, "name");

// boolean
const active = getProperty(user, "active");
```

Invalid property names are rejected:

```typescript
// Error: "age" is not a property of user.
getProperty(user, "age");
```

The constraint `K extends keyof T` ensures that `K` can only represent valid keys of `T`.

# 🗁 Using `keyof` with Indexed Access Types

You can combine `keyof` with indexed access types to retrieve all property value types.

```typescript
type User = {
  id: number;
  name: string;
  active: boolean;
};

type UserValues = User[keyof User];
```

Result:

```typescript
type UserValues = number | string | boolean;
```

Explanation:

* `keyof User` produces `"id" | "name" | "active"`.
* `User[keyof User]` accesses the types of those properties.
* TypeScript combines the resulting types into a union.

# 🗁 Using `keyof` with Mapped Types

Mapped types use `keyof` to iterate over object properties.

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

Result:

```typescript
type ReadonlyUser = {
  readonly id: number;
  readonly name: string;
  readonly email: string;
};
```

Another example makes every property optional:

```typescript
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

This pattern is the foundation of several built-in TypeScript utility types.

# 🗁 `keyof` with Index Signatures

When an object type has an index signature, `keyof` returns the corresponding index key types.

For a string index signature:

```typescript
type Dictionary = {
  [key: string]: string;
};

type DictionaryKeys = keyof Dictionary;
```

Result:

```typescript
type DictionaryKeys = string | number;
```

Why does `number` appear?

JavaScript converts numeric property access into string property keys in ordinary objects. For example, `object[42]` accesses the property `"42"`.

For a number index signature:

```typescript
type NumberDictionary = {
  [index: number]: string;
};

type NumberDictionaryKeys = keyof NumberDictionary;
```

Result:

```typescript
type NumberDictionaryKeys = number;
```

For a symbol index signature:

```typescript
declare const customKey: unique symbol;

type SymbolDictionary = {
  [customKey]: string;
};

type SymbolDictionaryKeys = keyof SymbolDictionary;
```

Result:

```typescript
type SymbolDictionaryKeys = typeof customKey;
```

# 🗁 `keyof` with Arrays

The `keyof` operator can also be applied to array types.

```typescript
type Users = string[];

type UserKeys = keyof Users;
```

The result is a union of the array's available property and method keys, including numeric indexing and members such as `"length"` and `"map"`.

For example:

```typescript
type Users = string[];

type LengthType = Users["length"];
```

Result:

```typescript
type LengthType = number;
```

If you only need the array element type, use indexed access with `number`:

```typescript
type User = Users[number];
```

Result:

```typescript
type User = string;
```

# 🗁 `keyof` with Tuples

Tuples have fixed positions, so their keys include numeric index information and array-related properties.

```typescript
type User = [number, string, boolean];

type UserKeys = keyof User;
```

The resulting type includes tuple indices such as `"0"`, `"1"`, and `"2"`, along with array members and numeric indexing.

You can retrieve individual element types using indexed access:

```typescript
type UserId = User[0];
type UserName = User[1];
type UserStatus = User[2];
```

Result:

```typescript
type UserId = number;
type UserName = string;
type UserStatus = boolean;
```

# 🗁 `keyof` with Union Types

When applied to a union of object types, `keyof` returns the keys that are safely available on every member of the union.

```typescript
type Admin = {
  id: number;
  name: string;
  permissions: string[];
};

type Customer = {
  id: number;
  name: string;
  loyaltyPoints: number;
};

type Person = Admin | Customer;

type PersonKeys = keyof Person;
```

Result:

```typescript
type PersonKeys = "id" | "name";
```

Although `Admin` and `Customer` have different properties, only `id` and `name` are common to both types.

To obtain all possible keys from the union members, use a distributive conditional type:

```typescript
type AllKeys<T> = T extends unknown ? keyof T : never;

type PersonKeys = AllKeys<Admin | Customer>;
```

Result:

```typescript
type PersonKeys =
  | "id"
  | "name"
  | "permissions"
  | "loyaltyPoints";
```

This technique is useful when working with discriminated unions and complex object models.

# 🗁 `keyof` with Intersection Types

For intersection types, `keyof` includes the keys from both intersected types.

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

Result:

```typescript
type UserAccountKeys =
  | "id"
  | "name"
  | "email"
  | "active";
```

An intersection combines the properties of both types.

# 🗁 Practical Example: Type-Safe Property Validation

You can use `keyof` to create a function that checks whether an object has a particular property.

```typescript
function hasProperty<T extends object>(
  object: T,
  key: PropertyKey
): key is keyof T {
  return key in object;
}
```

Usage:

```typescript
const user = {
  id: 1,
  name: "John"
};

const key: PropertyKey = "name";

if (hasProperty(user, key)) {
  const value = user[key];
}
```

The type predicate narrows `key` to `keyof T` when the property exists.

# 🗁 Practical Example: Updating Object Properties

You can create a type-safe update function using `keyof`.

```typescript
function updateProperty<T, K extends keyof T>(
  object: T,
  key: K,
  value: T[K]
): void {
  object[key] = value;
}
```

Usage:

```typescript
const user = {
  id: 1,
  name: "John",
  active: true
};

updateProperty(user, "name", "Alex");
updateProperty(user, "active", false);
updateProperty(user, "id", 100);

// Error: The value must be a number.
updateProperty(user, "id", "100");
```

The relationship between the key and its value is preserved through `T[K]`.

# 🗁 Practical Example: Selecting Object Properties

You can use `keyof` to build a reusable property-selection function.

```typescript
function selectProperties<
  T,
  K extends keyof T
>(
  object: T,
  keys: readonly K[]
): Pick<T, K> {
  const result = {} as Pick<T, K>;

  for (const key of keys) {
    result[key] = object[key];
  }

  return result;
}
```

Usage:

```typescript
const user = {
  id: 1,
  name: "John",
  email: "john@example.com",
  active: true
};

const publicUser = selectProperties(user, [
  "id",
  "name"
]);
```

Result:

```typescript
{
  id: 1,
  name: "John"
}
```

The return type is automatically inferred as:

```typescript
type PublicUser = {
  id: number;
  name: string;
};
```

# 🗁 Common Mistakes

**Using `keyof` directly on a value**

```typescript
const user = {
  id: 1,
  name: "John"
};

// Incorrect: keyof expects a type.
type UserKeys = keyof user;
```

Use `typeof` first:

```typescript
type UserKeys = keyof typeof user;
```

**Assuming union types include every possible key**

```typescript
type Admin = {
  id: number;
  permissions: string[];
};

type Customer = {
  id: number;
  loyaltyPoints: number;
};

type PersonKeys = keyof (Admin | Customer);
```

The result is only:

```typescript
type PersonKeys = "id";
```

Use a distributive conditional type when you need all keys from all union members.

**Confusing property keys with property value types**

```typescript
type User = {
  id: number;
  name: string;
};

type UserKeys = keyof User;
// "id" | "name"

type UserValues = User[keyof User];
// number | string
```

`keyof` extracts property names, while indexed access retrieves their value types.

# 🗁 Best Practices

* Use `keyof` to restrict property names to valid keys.
* Combine `keyof` with `T[K]` for strongly typed property access.
* Use `typeof` when deriving keys from existing JavaScript values.
* Use mapped types when transforming all properties of an object.
* Remember that `keyof` on a union returns keys common to every union member.
* Use distributive conditional types when you need keys from every union member.
* Avoid unnecessarily complex type expressions when a simple type alias is sufficient.

# 🗁 Summary

The `keyof` type operator extracts property keys from an object type and produces a union of key types.

Key concepts:

* `keyof T` extracts property keys.
* `keyof typeof value` extracts keys from an existing variable.
* `T[keyof T]` retrieves all property value types.
* `K extends keyof T` restricts generic keys to valid properties.
* `keyof` works with objects, arrays, tuples, index signatures, unions, and intersections.
* On union types, `keyof` returns only keys shared by all members.
* Mapped types use `keyof` to iterate over object properties.

The `keyof` operator is a fundamental building block for advanced TypeScript generics, mapped types, and type-safe object operations.
