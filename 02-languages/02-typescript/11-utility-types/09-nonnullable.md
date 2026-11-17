# NonNullable Utility Type

The `NonNullable<Type>` utility type constructs a new type by removing `null` and `undefined` from an existing type.

It is useful when a value may be absent or uninitialized, but a particular operation requires a value that is definitely present.

# 🗁 Syntax

```typescript
NonNullable<Type>
```

* `Type`: The original type.
* Result: A type containing all original possibilities except `null` and `undefined`.

# 🗁 Basic Example

```typescript
type Value = string | number | null | undefined;

type DefinedValue = NonNullable<Value>;

// Result:
// string | number
```

Both `null` and `undefined` are removed from the resulting type.

# 🗁 Working with String Types

```typescript
type UserName = string | null | undefined;

type ValidUserName = NonNullable<UserName>;

// Result:
// string
```

A variable using `ValidUserName` must contain a string.

```typescript
let userName: ValidUserName = "Muhammad";

// Valid
userName = "Iqbal";

// TypeScript errors:
// userName = null;
// userName = undefined;
```

# 🗁 Working with Number Types

```typescript
type Age = number | null | undefined;

type ValidAge = NonNullable<Age>;

// Result:
// number
```

This is useful when numeric values are optional in an application but must be validated before calculations.

# 🗁 Working with Object Types

`NonNullable` can also remove nullish values from object unions.

```typescript
interface User {
  id: number;
  name: string;
}

type UserValue = User | null | undefined;

type ValidUser = NonNullable<UserValue>;

// Result:
// User
```

The resulting type contains only the `User` object.

# 🗁 Working with Function Return Types

You can combine `NonNullable` with `ReturnType` to remove nullish possibilities from a function's return type.

```typescript
function getUser() {
  return Math.random() > 0.5
    ? { id: 1, name: "John" }
    : null;
}

type UserResult = ReturnType<typeof getUser>;

// Result:
// { id: number; name: string } | null

type ValidUserResult = NonNullable<UserResult>;

// Result:
// { id: number; name: string }
```

This is useful when a function may return `null`, but another part of the application needs the non-null object type.

# 🗁 Working with Array Elements

You can use `NonNullable` to derive an array element type without `null` or `undefined`.

```typescript
type Values = (string | number | null | undefined)[];

type ValidValue = NonNullable<Values[number]>;

// Result:
// string | number

type ValidValues = ValidValue[];
```

The resulting array type allows only strings and numbers.

# 🗁 Using NonNullable with Generics

You can use `NonNullable` inside generic functions and type aliases.

```typescript
type RequiredValue<T> = NonNullable<T>;

type Name = RequiredValue<string | null>;
type Count = RequiredValue<number | undefined>;

// Name: string
// Count: number
```

This allows reusable types that remove nullish possibilities from different input types.

# 🗁 How NonNullable Works Internally

In modern TypeScript, the built-in `NonNullable` utility type is conceptually implemented using an intersection with an empty object type.

```typescript
type NonNullable<T> = T & {};
```

Under `strictNullChecks`, `{}` excludes `null` and `undefined` while accepting other non-nullish values.

Consider:

```typescript
type Value = string | number | null | undefined;

type Result = NonNullable<Value>;
```

The resulting type is:

```typescript
type Result = string | number;
```

Historically, `NonNullable` was commonly expressed using a conditional type:

```typescript
type NonNullable<T> = T extends null | undefined
  ? never
  : T;
```

Both approaches express the goal of excluding nullish values, but the current TypeScript library definition uses the intersection approach.

# 🗁 NonNullable vs Exclude

Both utilities can remove `null` and `undefined`, but their purposes differ.

| Utility Type | Purpose |
| --- | --- |
| `NonNullable<T>` | Specifically removes `null` and `undefined` |
| `Exclude<T, U>` | Removes any union members assignable to `U` |

Example:

```typescript
type Value = string | number | null | undefined;

type ResultA = NonNullable<Value>;

type ResultB = Exclude<Value, null | undefined>;

// Both:
// string | number
```

Use `NonNullable` when your intention is specifically to remove nullish values.

# 🗁 Important Behavior

**NonNullable does not validate runtime values.**

```typescript
function getName(): string | null {
  return Math.random() > 0.5 ? "John" : null;
}

type Name = NonNullable<ReturnType<typeof getName>>;
```

`Name` is a compile-time type. It does not change what `getName()` returns at runtime.

You must still check the actual value before using it.

```typescript
const name = getName();

if (name !== null) {
  console.log(name.toUpperCase());
}
```

**NonNullable depends on strict null checking.**

With `strictNullChecks: true`, TypeScript treats `null` and `undefined` as distinct types that can be explicitly removed.

Without strict null checking, their behavior is less precise because they are broadly assignable to other types.

For reliable null safety, enable strict null checking:

```json
{
  "compilerOptions": {
    "strict": true
  }
}
```

**NonNullable does not remove optional properties.**

```typescript
interface User {
  id: number;
  name?: string | null;
}

type ValidUser = NonNullable<User>;
```

`ValidUser` is still the same object type. Its optional `name` property remains optional and can still contain `null`.

To remove nullish values from a particular property, apply `NonNullable` to that property's type:

```typescript
type ValidName = NonNullable<User["name"]>;

// Result:
// string
```

# 🗁 Practical Example: Handling API Responses

An API may return a user object, `null`, or `undefined`.

You can define a reusable type for the valid user object.

```typescript
interface User {
  id: number;
  name: string;
  email: string;
}

type ApiUser = User | null | undefined;

type ValidApiUser = NonNullable<ApiUser>;

function displayUser(user: ValidApiUser): void {
  console.log(`Name: ${user.name}`);
  console.log(`Email: ${user.email}`);
}
```

Before calling `displayUser`, validate the API response:

```typescript
const apiResponse: ApiUser = {
  id: 1,
  name: "John",
  email: "john@example.com",
};

if (apiResponse != null) {
  displayUser(apiResponse);
}
```

The `!= null` check excludes both `null` and `undefined`.

# 🗁 Best Practices

* Use `NonNullable` when a type should exclude `null` and `undefined`.
* Combine it with `ReturnType` to derive non-nullish function return types.
* Use it with indexed access types to extract non-nullish property types.
* Enable `strictNullChecks` for reliable null safety.
* Perform runtime checks before using values that may be absent.
* Remember that removing nullish values from a type does not modify runtime data.
* Do not confuse removing nullish values from an object type with making its properties required.

# 🗁 Summary

* `NonNullable<T>` removes `null` and `undefined` from a type.
* It works with primitive, object, function, and union types.
* It is useful for handling optional API responses and nullable values.
* It can be combined with `ReturnType`, generics, and indexed access types.
* It does not perform runtime validation.
* It does not automatically make optional object properties required.
