# Generic Basics

Generics are a TypeScript feature that allows you to create reusable, flexible, and type-safe components, functions, interfaces, and classes.

A generic lets you define a type parameter that represents a type supplied later when the generic is used.

Instead of hardcoding a specific type, you write code that works with multiple types while preserving type information.

Generics are useful for:

* Creating reusable functions.
* Building flexible interfaces and classes.
* Working with arrays and collections.
* Defining reusable API response types.
* Building type-safe data structures.
* Implementing utility functions.
* Reducing duplicated code.
* Preserving type information across operations.

# 🗁 The Problem Without Generics

Consider a function that returns the value passed to it.

```typescript
function getString(value: string): string {
  return value;
}

function getNumber(value: number): number {
  return value;
}

function getBoolean(value: boolean): boolean {
  return value;
}
```

Each function performs the same operation, but requires a different implementation for each type.

You could use `any` to create one reusable function:

```typescript
function getValue(value: any): any {
  return value;
}
```

Usage:

```typescript
const name = getValue("Alice");
const age = getValue(25);
```

However, `any` removes useful type information.

```typescript
const result = getValue("Alice");

result.toFixed(); // No TypeScript error, but fails at runtime
```

TypeScript cannot determine the actual type of `result` because the function returns `any`.

Generics solve this problem by preserving the relationship between the input and output types.

# 🗁 Basic Generic Syntax

A generic type parameter is declared inside angle brackets.

```typescript
<T>
```

Here, `T` is a type parameter. It acts as a placeholder for a type that will be supplied when the generic is used.

Example:

```typescript
function getValue<T>(value: T): T {
  return value;
}
```

The function accepts a value of type `T` and returns a value of the same type.

Usage:

```typescript
const name = getValue<string>("Alice");
const age = getValue<number>(25);
const isActive = getValue<boolean>(true);
```

TypeScript infers the return types:

```typescript
// name: string
// age: number
// isActive: boolean
```

The same function works with different types without losing type safety.

# 🗁 Understanding Type Parameters

A type parameter is a variable that represents a type.

In this example:

```typescript
function identity<T>(value: T): T {
  return value;
}
```

The generic components are:

| Component | Meaning |
| --- | --- |
| `T` | Type parameter |
| `<T>` | Generic parameter declaration |
| `value: T` | Parameter uses type `T` |
| `: T` | Return type is `T` |

When the function is called, TypeScript determines what `T` represents.

```typescript
identity<string>("Hello");
```

In this call:

```typescript
T = string
```

In another call:

```typescript
identity<number>(100);
```

Here:

```typescript
T = number
```

The type parameter is resolved separately for each invocation.

# 🗁 Generic Type Inference

TypeScript can often determine generic type arguments automatically.

Explicit type argument:

```typescript
const result = identity<string>("Hello");
```

Inferred type argument:

```typescript
const result = identity("Hello");
```

Both produce:

```typescript
// result: string
```

TypeScript infers `T` from the argument.

Inference also works with other types:

```typescript
const name = identity("Alice");
const age = identity(30);
const active = identity(true);
const values = identity([10, 20, 30]);
```

Inferred types:

```typescript
// name: string
// age: number
// active: boolean
// values: number[]
```

Prefer inference when TypeScript can determine the intended type correctly.

# 🗁 Multiple Type Parameters

A generic can have multiple type parameters.

```typescript
function pair<T, U>(first: T, second: U): [T, U] {
  return [first, second];
}
```

Usage:

```typescript
const user = pair("Alice", 25);
const result = pair(true, "Success");
```

Inferred types:

```typescript
// user: [string, number]
// result: [boolean, string]
```

You can also provide explicit type arguments:

```typescript
const user = pair<string, number>("Alice", 25);
```

Multiple type parameters are useful when a function works with values that have different types.

# 🗁 Generic Functions with Arrays

Generics are commonly used to create reusable array utilities.

Example:

```typescript
function getFirst<T>(items: T[]): T | undefined {
  return items[0];
}
```

Usage:

```typescript
const firstName = getFirst(["Alice", "Bob"]);
const firstNumber = getFirst([10, 20, 30]);
const firstBoolean = getFirst([true, false]);
```

Inferred types:

```typescript
// firstName: string | undefined
// firstNumber: number | undefined
// firstBoolean: boolean | undefined
```

The function preserves the element type of each array.

Another example:

```typescript
function reverse<T>(items: T[]): T[] {
  return [...items].reverse();
}
```

Usage:

```typescript
const numbers = reverse([1, 2, 3]);
const names = reverse(["Alice", "Bob"]);
```

The return type corresponds to the input array's element type.

# 🗁 Generic Functions with Objects

Generics also work with object types.

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
  name: "Alice",
  age: 25,
  active: true,
};

const name = getProperty(user, "name");
const age = getProperty(user, "age");
const active = getProperty(user, "active");
```

Inferred types:

```typescript
// name: string
// age: number
// active: boolean
```

This example introduces generic constraints and indexed access types, which are covered in later topics.

# 🗁 Generic Type Aliases

Type aliases can also use generic parameters.

```typescript
type Container<T> = {
  value: T;
};
```

Usage:

```typescript
type StringContainer = Container<string>;
type NumberContainer = Container<number>;
```

Equivalent expanded types:

```typescript
type StringContainer = {
  value: string;
};

type NumberContainer = {
  value: number;
};
```

You can also use the generic type directly:

```typescript
const username: Container<string> = {
  value: "Alice",
};

const userAge: Container<number> = {
  value: 25,
};
```

# 🗁 Generic Interfaces

Interfaces can define generic type parameters.

```typescript
interface Box<T> {
  value: T;
}
```

Usage:

```typescript
const stringBox: Box<string> = {
  value: "Hello",
};

const numberBox: Box<number> = {
  value: 100,
};
```

The same interface supports different value types.

Generic interfaces are useful for reusable data structures, API responses, repositories, and service contracts.

# 🗁 Generic Classes

Classes can also accept generic type parameters.

```typescript
class Storage<T> {
  private value: T;

  constructor(value: T) {
    this.value = value;
  }

  getValue(): T {
    return this.value;
  }
}
```

Usage:

```typescript
const nameStorage = new Storage<string>("Alice");
const ageStorage = new Storage<number>(25);

console.log(nameStorage.getValue());
console.log(ageStorage.getValue());
```

The class maintains the selected type throughout its instance.

# 🗁 Generic API Response Types

Generics are especially useful when building applications that consume APIs.

Different endpoints may return different data structures, but share a common response format.

Define a reusable response type:

```typescript
interface ApiResponse<T> {
  success: boolean;
  data: T;
  message: string;
}
```

Define application data:

```typescript
interface User {
  id: number;
  name: string;
  email: string;
}

interface Product {
  id: number;
  title: string;
  price: number;
}
```

Create typed responses:

```typescript
type UserResponse = ApiResponse<User>;
type ProductResponse = ApiResponse<Product>;
```

Example values:

```typescript
const userResponse: UserResponse = {
  success: true,
  data: {
    id: 1,
    name: "Alice",
    email: "alice@example.com",
  },
  message: "User loaded successfully",
};

const productResponse: ProductResponse = {
  success: true,
  data: {
    id: 101,
    title: "Keyboard",
    price: 75,
  },
  message: "Product loaded successfully",
};
```

The shared response structure is reusable while the `data` property remains strongly typed.

# 🗁 Generic Constraints Overview

Sometimes a generic function needs to work only with certain kinds of types.

For example, consider a function that reads an object's `length` property.

```typescript
function getLength<T>(value: T): number {
  return value.length;
}
```

This produces a TypeScript error because not every possible type has a `length` property.

You can use a generic constraint:

```typescript
function getLength<T extends { length: number }>(
  value: T,
): number {
  return value.length;
}
```

Usage:

```typescript
getLength("Hello");
getLength([10, 20, 30]);
getLength({ length: 5 });
```

Invalid usage:

```typescript
// TypeScript error
// getLength(100);
```

`extends` restricts the allowed types to those that satisfy the required structure.

Generic constraints are explored in detail in the Generics section.

# 🗁 Generic Type Parameter Naming

TypeScript does not require a specific name for generic parameters.

Common conventions include:

| Parameter | Common Meaning |
| --- | --- |
| `T` | Type |
| `U` | Second type parameter |
| `V` | Third type parameter |
| `K` | Key |
| `V` | Value |
| `E` | Error type |
| `R` | Result or return type |

Examples:

```typescript
function identity<T>(value: T): T {
  return value;
}

function mapObject<K, V>(
  key: K,
  value: V,
): [K, V] {
  return [key, value];
}
```

For simple generics, short names such as `T` and `U` are conventional.

For more complex APIs, descriptive names can improve readability.

```typescript
function createResponse<ResponseData>(
  data: ResponseData,
): ResponseData {
  return data;
}
```

# 🗁 Generic Type Parameters vs any

Generics and `any` may appear similar because both can accept different types. However, they behave very differently.

Using `any`:

```typescript
function getValue(value: any): any {
  return value;
}

const result = getValue("Hello");

result.toFixed(); // Allowed by TypeScript
```

Using generics:

```typescript
function getValue<T>(value: T): T {
  return value;
}

const result = getValue("Hello");

// TypeScript error
// result.toFixed();
```

Comparison:

| Feature | `any` | Generics |
| --- | --- | --- |
| Accepts multiple types | Yes | Yes |
| Preserves input type | No | Yes |
| Provides type safety | No | Yes |
| Supports reusable functions | Yes | Yes |
| Maintains relationships between types | No | Yes |

Use generics when you want reusable code that preserves type information.

# 🗁 Common Mistakes

**Using any instead of generics**

```typescript
function identity(value: any): any {
  return value;
}
```

This removes the relationship between the input and output types.

Prefer:

```typescript
function identity<T>(value: T): T {
  return value;
}
```

**Using a generic when it is unnecessary**

```typescript
function printValue<T>(value: T): void {
  console.log(value);
}
```

If the function does not need to preserve or relate the type, a generic may provide no meaningful benefit.

A simpler alternative could be:

```typescript
function printValue(value: unknown): void {
  console.log(value);
}
```

Choose the type based on the function's actual requirements.

**Declaring unused type parameters**

```typescript
function logValue<T>(value: string): void {
  console.log(value);
}
```

Here, `T` does not participate in the function's input, output, or behavior.

Remove unnecessary type parameters.

**Assuming generics automatically restrict types**

```typescript
function process<T>(value: T): T {
  return value;
}
```

This function accepts any type. Generics preserve types but do not automatically impose restrictions.

Use `extends` when a constraint is required.

# 🗁 Best Practices

* Use generics to preserve relationships between input and output types.
* Prefer type inference when it produces the intended result.
* Use explicit type arguments when inference is insufficient or unclear.
* Choose descriptive type parameter names for complex generic APIs.
* Avoid unnecessary use of `any`.
* Avoid generic parameters that do not affect the resulting type.
* Use constraints when a generic operation requires specific properties.
* Reuse generic types for common application structures such as API responses and repositories.
* Keep generic implementations simple and readable.
* Do not introduce generics when a normal concrete type is sufficient.

# 🗁 Summary

* Generics allow reusable and type-safe code.
* A generic type parameter represents a type supplied later.
* Generic parameters are declared using angle brackets.
* TypeScript can infer generic arguments automatically.
* A generic can have multiple type parameters.
* Functions, type aliases, interfaces, and classes can all use generics.
* Generics preserve relationships between input and output types.
* Generic constraints restrict the types that can be used.
* Generics provide stronger type safety than `any`.

# 🗁 Related Topics

* Generic Functions
* Generic Interfaces
* Generic Classes
* Generic Constraints
* Generic Defaults
* Type Inference
* Type Parameters
* keyof Operator
* typeof Operator
* Indexed Access Types
* Conditional Types
* Utility Types
