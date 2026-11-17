# ReturnType Utility Type

The `ReturnType<Type>` utility type extracts the return type of a function type.

It allows you to derive a type directly from a function instead of manually defining the same return type in multiple places.

This is particularly useful when working with reusable functions, API responses, callbacks, and complex function signatures.

# 🗁 Syntax

```typescript
ReturnType<Type>
```

* `Type`: A function type.
* Result: The type returned by that function.

# 🗁 Basic Example

```typescript
function getUser() {
  return {
    id: 1,
    name: "John",
    email: "john@example.com",
  };
}

type User = ReturnType<typeof getUser>;

// Result:
// {
//   id: number;
//   name: string;
//   email: string;
// }
```

The `typeof` operator obtains the function's type, and `ReturnType` extracts its return type.

# 🗁 Using ReturnType with Explicit Return Types

You can also extract the return type of a function with an explicitly declared return type.

```typescript
function getAge(): number {
  return 25;
}

type Age = ReturnType<typeof getAge>;

// Result:
// number
```

The resulting type is `number`.

# 🗁 Working with Arrow Functions

`ReturnType` works with arrow functions as well.

```typescript
const getProduct = () => {
  return {
    id: 101,
    name: "Laptop",
    price: 1200,
  };
};

type Product = ReturnType<typeof getProduct>;

// Result:
// {
//   id: number;
//   name: string;
//   price: number;
// }
```

# 🗁 Working with Async Functions

For an asynchronous function, `ReturnType` extracts the complete `Promise` return type.

```typescript
async function getUser() {
  return {
    id: 1,
    name: "John",
  };
}

type UserPromise = ReturnType<typeof getUser>;

// Result:
// Promise<{
//   id: number;
//   name: string;
// }>
```

If you need the resolved value rather than the `Promise` type, combine `ReturnType` with `Awaited`.

```typescript
type User = Awaited<ReturnType<typeof getUser>>;

// Result:
// {
//   id: number;
//   name: string;
// }
```

# 🗁 Working with Function Type Aliases

You can extract the return type from a function type alias.

```typescript
type Calculate = (a: number, b: number) => number;

type Result = ReturnType<Calculate>;

// Result:
// number
```

You do not need `typeof` here because `Calculate` is already a function type.

# 🗁 Working with Interfaces

`ReturnType` can also extract return types from function interfaces.

```typescript
interface UserFactory {
  (name: string, age: number): {
    name: string;
    age: number;
  };
}

type User = ReturnType<UserFactory>;

// Result:
// {
//   name: string;
//   age: number;
// }
```

# 🗁 Working with Generic Functions

When used with a generic function, `ReturnType` generally extracts the return type using the generic parameter's constraint or default behavior rather than a particular call's inferred type.

```typescript
function identity<T>(value: T): T {
  return value;
}

type Result = ReturnType<typeof identity>;

// Result:
// unknown
```

The generic parameter `T` has no constraint, so the extracted return type is `unknown`.

If the generic parameter has a constraint, the result reflects that constraint.

```typescript
function getValue<T extends string | number>(value: T): T {
  return value;
}

type Result = ReturnType<typeof getValue>;

// Result:
// string | number
```

# 🗁 Working with Function Overloads

For overloaded functions, `ReturnType` uses the return type of the last overload signature.

```typescript
function format(value: string): string;
function format(value: number): string;
function format(value: string | number): string | number;

function format(value: string | number): string | number {
  return typeof value === "string"
    ? value.toUpperCase()
    : value.toFixed(2);
}

type FormatResult = ReturnType<typeof format>;

// Result:
// string | number
```

The implementation signature is not directly used for overload resolution. The last overload signature determines the extracted return type.

# 🗁 Using ReturnType with typeof

The `typeof` operator is commonly used with `ReturnType` to obtain the type of a function declared as a variable.

```typescript
const createUser = (name: string) => ({
  id: 1,
  name,
  active: true,
});

type User = ReturnType<typeof createUser>;
```

This pattern keeps the derived type synchronized with the function's return structure.

# 🗁 Using ReturnType with NonNullable

You can combine `ReturnType` and `NonNullable` when a function may return `null` or `undefined`.

```typescript
function findUser() {
  return Math.random() > 0.5
    ? {
        id: 1,
        name: "John",
      }
    : null;
}

type UserResult = ReturnType<typeof findUser>;

// Result:
// { id: number; name: string } | null

type User = NonNullable<UserResult>;

// Result:
// { id: number; name: string }
```

# 🗁 How ReturnType Works Internally

The built-in `ReturnType` utility is defined using a conditional type with the `infer` keyword.

Its simplified definition is:

```typescript
type ReturnType<T extends (...args: any) => any> =
  T extends (...args: any) => infer R
    ? R
    : any;
```

The `infer R` expression asks TypeScript to infer the function's return type and assign it to `R`.

Consider:

```typescript
type Calculate = (a: number, b: number) => number;

type Result = ReturnType<Calculate>;
```

TypeScript matches the function type and infers:

```typescript
type Result = number;
```

The `infer` keyword is a powerful feature for extracting types from complex structures.

# 🗁 Important Behavior

**ReturnType requires a function type.**

```typescript
type Value = ReturnType<string>;

// TypeScript error:
// string does not satisfy the function constraint.
```

**ReturnType does not execute the function.**

```typescript
function getValue(): number {
  return 100;
}

type Value = ReturnType<typeof getValue>;
```

The function is not called. TypeScript only examines its declared or inferred type.

**ReturnType does not automatically remove Promise wrappers.**

```typescript
async function getData() {
  return "Hello";
}

type Result = ReturnType<typeof getData>;

// Result:
// Promise<string>
```

Use `Awaited` when you need the resolved value:

```typescript
type ResolvedResult = Awaited<ReturnType<typeof getData>>;

// Result:
// string
```

**Generic return types may become broader.**

```typescript
function wrap<T>(value: T): { data: T } {
  return { data: value };
}

type Result = ReturnType<typeof wrap>;

// Result:
// { data: unknown }
```

The extracted type does not represent a specific invocation of the generic function.

# 🗁 Practical Example: API Response Types

Suppose an application has a function responsible for fetching user information.

Instead of defining the response type separately, derive it from the function.

```typescript
interface User {
  id: number;
  name: string;
  email: string;
}

async function fetchUser(id: number): Promise<User> {
  const response = await fetch(`/api/users/${id}`);

  if (!response.ok) {
    throw new Error("Failed to fetch user");
  }

  return response.json() as Promise<User>;
}

type FetchUserResult = ReturnType<typeof fetchUser>;

// Result:
// Promise<User>

type UserResponse = Awaited<FetchUserResult>;

// Result:
// User
```

This approach helps keep related types synchronized with their source function.

# 🗁 Best Practices

* Use `ReturnType` when you need to derive a function's return type.
* Combine it with `typeof` when referencing a function variable.
* Use `Awaited<ReturnType<typeof fn>>` for resolved asynchronous return types.
* Use `NonNullable` when nullish return values should be excluded from a derived type.
* Remember that overloaded functions use their last overload signature.
* Understand how generic functions affect extracted return types.
* Prefer deriving types from existing implementations when doing so improves maintainability.

# 🗁 Summary

* `ReturnType<T>` extracts the return type of a function type.
* It works with regular functions, arrow functions, interfaces, and function type aliases.
* It can extract `Promise` return types from asynchronous functions.
* Combine it with `Awaited` to obtain resolved asynchronous values.
* It uses the `infer` keyword internally.
* For overloaded functions, it uses the last overload signature.
* It does not execute the function or infer a specific generic invocation.
