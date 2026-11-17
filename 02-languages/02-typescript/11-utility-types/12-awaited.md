# Awaited Utility Type

The `Awaited<Type>` utility type recursively unwraps `Promise` types and determines the type of the value that an asynchronous operation eventually resolves to.

It is useful when working with asynchronous functions, promises, API responses, and nested promise structures.

For example, if a function returns `Promise<string>`, `Awaited` extracts `string`.

# 🗁 Syntax

```typescript
Awaited<Type>
```

* `Type`: The type to unwrap.
* Result: The type obtained after recursively unwrapping promise-like values.

# 🗁 Basic Example

```typescript
type Result = Awaited<Promise<string>>;

// Result:
// string
```

The `Promise` wrapper is removed, leaving the resolved value type.

# 🗁 Working with Promise Types

`Awaited` works with different promise value types.

```typescript
type StringResult = Awaited<Promise<string>>;
type NumberResult = Awaited<Promise<number>>;
type BooleanResult = Awaited<Promise<boolean>>;

// StringResult: string
// NumberResult: number
// BooleanResult: boolean
```

# 🗁 Working with Nested Promises

`Awaited` recursively unwraps nested promises.

```typescript
type Result = Awaited<
  Promise<Promise<string>>
>;

// Result:
// string
```

You do not need to apply `Awaited` repeatedly.

```typescript
type Result = Awaited<
  Promise<Promise<Promise<number>>>
>;

// Result:
// number
```

# 🗁 Working with Async Functions

An asynchronous function always returns a `Promise`.

You can use `ReturnType` and `Awaited` together to extract the resolved return type.

```typescript
async function getUser() {
  return {
    id: 1,
    name: "John",
    email: "john@example.com",
  };
}

type UserPromise = ReturnType<typeof getUser>;

// Result:
// Promise<{
//   id: number;
//   name: string;
//   email: string;
// }>

type User = Awaited<UserPromise>;

// Result:
// {
//   id: number;
//   name: string;
//   email: string;
// }
```

This is one of the most common practical uses of `Awaited`.

# 🗁 Combining Awaited with ReturnType

You can combine both utilities directly.

```typescript
function fetchProducts() {
  return Promise.resolve([
    { id: 1, name: "Laptop" },
    { id: 2, name: "Keyboard" },
  ]);
}

type Products = Awaited<ReturnType<typeof fetchProducts>>;

// Result:
// {
//   id: number;
//   name: string;
// }[]
```

This allows you to derive the resolved return type without manually writing the array structure.

# 🗁 Working with Promise.all

`Promise.all` resolves an array of promises into a promise containing an array of their resolved values.

`Awaited` can extract the resulting value type.

```typescript
const userPromise = Promise.resolve({
  id: 1,
  name: "John",
});

const countPromise = Promise.resolve(10);

const resultPromise = Promise.all([
  userPromise,
  countPromise,
]);

type Result = Awaited<typeof resultPromise>;

// Result:
// [
//   { id: number; name: string },
//   number
// ]
```

The resulting tuple contains the resolved types of the individual promises.

# 🗁 Working with Promise.race

`Promise.race` resolves or rejects according to the first settled promise.

Its resolved value type is a union of the possible resolved values.

```typescript
const resultPromise = Promise.race([
  Promise.resolve("success"),
  Promise.resolve(100),
]);

type Result = Awaited<typeof resultPromise>;

// Result:
// string | number
```

# 🗁 Working with PromiseLike

`Awaited` is not limited to native `Promise` types.

It also supports promise-like objects that implement a compatible `then` method.

```typescript
interface CustomPromiseLike {
  then(
    onfulfilled: (value: string) => unknown
  ): unknown;
}

type Result = Awaited<CustomPromiseLike>;

// Result:
// string
```

This makes `Awaited` useful when working with custom asynchronous abstractions.

# 🗁 Working with Union Types

`Awaited` can process unions containing promises and ordinary values.

```typescript
type Value =
  | Promise<string>
  | Promise<number>
  | boolean;

type Result = Awaited<Value>;

// Result:
// string | number | boolean
```

Promise members are unwrapped, while non-promise members remain unchanged.

# 🗁 Working with Null and Undefined

`Awaited` preserves `null` and `undefined` when they are part of the original type.

```typescript
type Result = Awaited<
  Promise<string | null | undefined>
>;

// Result:
// string | null | undefined
```

If you need to remove nullish values, combine it with `NonNullable`.

```typescript
type ValidResult = NonNullable<
  Awaited<Promise<string | null | undefined>>
>;

// Result:
// string
```

# 🗁 How Awaited Works Internally

The built-in `Awaited` utility uses conditional types and the `infer` keyword to recursively inspect promise-like objects.

Its simplified conceptual implementation is:

```typescript
type Awaited<T> =
  T extends null | undefined
    ? T
    : T extends object & {
        then(
          onfulfilled: infer F,
          ...args: any[]
        ): any;
      }
      ? F extends (
          value: infer V,
          ...args: any[]
        ) => any
        ? Awaited<V>
        : never
      : T;
```

The implementation:

* Preserves `null` and `undefined`.
* Checks whether the type is an object with a callable `then` method.
* Infers the fulfillment callback.
* Extracts the callback's value type.
* Recursively unwraps that value.
* Returns ordinary non-promise values unchanged.

The actual TypeScript library definition includes additional details for handling edge cases.

# 🗁 Awaited vs ReturnType

These utilities serve different purposes.

| Utility Type | Purpose |
| --- | --- |
| `ReturnType<T>` | Extracts a function's declared or inferred return type |
| `Awaited<T>` | Recursively unwraps promise-like types |

Example:

```typescript
async function getData(): Promise<string> {
  return "Hello";
}

type ResultA = ReturnType<typeof getData>;
type ResultB = Awaited<ResultA>;

// ResultA:
// Promise<string>

// ResultB:
// string
```

# 🗁 Awaited vs Promise

`Promise<T>` represents an asynchronous operation that will fulfill with a value of type `T`.

`Awaited<T>` extracts the value type that the promise resolves to.

```typescript
type PromiseValue = Promise<number>;

type ResolvedValue = Awaited<PromiseValue>;

// PromiseValue:
// Promise<number>

// ResolvedValue:
// number
```

Use `Promise<T>` when describing an asynchronous return type and `Awaited<T>` when deriving its resolved value type.

# 🗁 Practical Example: API Response Types

Suppose an application has an asynchronous function that fetches user information.

You can derive the resolved response type directly from the function.

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

type UserResponse = Awaited<
  ReturnType<typeof fetchUser>
>;

// Result:
// User
```

You can now reuse `UserResponse` throughout the application.

```typescript
function displayUser(user: UserResponse): void {
  console.log(user.name);
  console.log(user.email);
}
```

This keeps the derived type synchronized with the function's return type.

# 🗁 Practical Example: Reusable Async Function Types

You can create a reusable type for extracting the resolved result of any asynchronous function.

```typescript
type AsyncResult<T extends (...args: any[]) => any> =
  Awaited<ReturnType<T>>;

async function getProduct() {
  return {
    id: 101,
    name: "Laptop",
    price: 1200,
  };
}

type Product = AsyncResult<typeof getProduct>;

// Result:
// {
//   id: number;
//   name: string;
//   price: number;
// }
```

This pattern is useful when building service layers, API clients, and reusable data-fetching abstractions.

# 🗁 Important Behavior

**Awaited does not execute a promise.**

```typescript
type Result = Awaited<Promise<number>>;
```

This is compile-time type extraction. It does not create, execute, or resolve a promise.

**Awaited preserves ordinary types.**

```typescript
type StringResult = Awaited<string>;
type NumberResult = Awaited<number>;

// StringResult: string
// NumberResult: number
```

**Awaited recursively unwraps nested promises.**

```typescript
type Result = Awaited<
  Promise<Promise<Promise<boolean>>>
>;

// Result:
// boolean
```

**Awaited supports promise-like objects.**

It follows the behavior of JavaScript's `await` operation by examining compatible `then` methods, not just native `Promise` instances.

**Awaited does not remove rejected promise types.**

A promise's rejection reason is not represented in its generic type parameter.

```typescript
type Result = Awaited<Promise<string>>;

// Result:
// string
```

The type does not describe possible rejection reasons. Handle errors separately using appropriate error-handling logic.

# 🗁 Best Practices

* Use `Awaited` to extract resolved promise value types.
* Combine it with `ReturnType` for asynchronous function return types.
* Use it with `Promise.all` and `Promise.race` when deriving resolved results.
* Use `NonNullable` when nullish resolved values must be excluded.
* Prefer deriving response types from existing functions when practical.
* Remember that `Awaited` supports promise-like objects.
* Handle asynchronous errors separately because `Awaited` does not model rejection types.

# 🗁 Summary

* `Awaited<T>` extracts the resolved value type of a promise-like type.
* It recursively unwraps nested promises.
* It supports native promises and compatible promise-like objects.
* It preserves ordinary non-promise values.
* It works with union types and nullish values.
* It is commonly combined with `ReturnType`.
* It is useful for deriving API response and asynchronous function result types.
* It performs compile-time type extraction, not runtime promise resolution.
