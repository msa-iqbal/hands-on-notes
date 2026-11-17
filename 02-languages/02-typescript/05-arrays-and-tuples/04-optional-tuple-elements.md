# Optional Tuple Elements

Optional tuple elements are tuple positions that may or may not be provided when creating a tuple.

TypeScript uses the `?` modifier to declare optional tuple elements.

Optional tuple elements are useful when:

* Some values are not always available.
* A function accepts a variable number of positional arguments.
* A data structure has required and optional fields.
* Different tuple configurations share the same basic structure.

# 🗁 Basic Syntax

Use `?` after an element's type to make that position optional.

```typescript
type User = [
  id: number,
  name: string,
  age?: number
];
```

In this example:

* `id` is required.
* `name` is required.
* `age` is optional.

Valid assignments:

```typescript
const user1: User = [1, "Alice"];

const user2: User = [2, "Bob", 25];
```

Both tuples are valid because the third element is optional.

# 🗁 Declaring Optional Tuple Elements

You can declare multiple optional elements.

```typescript
type User = [
  id: number,
  name: string,
  age?: number,
  email?: string
];
```

Examples:

```typescript
const user1: User = [1, "Alice"];

const user2: User = [2, "Bob", 25];

const user3: User = [
  3,
  "Charlie",
  30,
  "charlie@example.com",
];
```

All required elements must be present, while optional elements may be omitted from the end.

# 🗁 Optional Elements Must Follow Required Elements

Required tuple elements cannot normally follow optional elements.

Incorrect:

```typescript
type User = [
  id: number,
  age?: number,
  name: string
];
```

TypeScript reports an error because a required element follows an optional element.

Correct:

```typescript
type User = [
  id: number,
  name: string,
  age?: number
];
```

This restriction keeps tuple positions predictable.

# 🗁 Accessing Optional Tuple Elements

An optional tuple element may be absent, so accessing it produces a type that includes `undefined`.

```typescript
type User = [
  id: number,
  name: string,
  age?: number
];

const user: User = [1, "Alice"];

const age = user[2];

// Inferred type: number | undefined
```

Handle the possibility of `undefined` before using the value.

```typescript
if (age !== undefined) {
  console.log(age.toFixed(0));
}
```

You can also use optional chaining and nullish coalescing where appropriate.

```typescript
console.log(age?.toFixed(0) ?? "Age not provided");
```

# 🗁 Optional Tuple Elements vs Union with Undefined

These two declarations are related but have different meanings.

**Optional tuple element**

```typescript
type User = [
  id: number,
  name: string,
  age?: number
];

const user1: User = [1, "Alice"];
const user2: User = [2, "Bob", 25];
```

The third position may be omitted.

**Required position that allows `undefined`**

```typescript
type User = [
  id: number,
  name: string,
  age: number | undefined
];

const user: User = [1, "Alice", undefined];
```

The third position must exist, but its value can be `undefined`.

| Feature | `age?: number` | `age: number \| undefined` |
| --- | --- | --- |
| Position may be omitted | Yes | No |
| Position may contain `undefined` | Yes | Yes |
| Position is required | No | Yes |

# 🗁 Optional Tuple Elements and Tuple Length

Optional tuple elements affect the possible length of a tuple.

```typescript
type User = [
  id: number,
  name: string,
  age?: number,
  email?: string
];

const user1: User = [1, "Alice"];
const user2: User = [2, "Bob", 25];
const user3: User = [3, "Charlie", 30, "charlie@example.com"];

console.log(user1.length); // 2
console.log(user2.length); // 3
console.log(user3.length); // 4
```

TypeScript represents the tuple's length as a union of possible numeric literal values.

For the `User` type above, the possible lengths are `2 | 3 | 4`.

# 🗁 Optional Tuple Elements in Function Parameters

Optional tuple elements are useful when defining functions that accept different numbers of positional arguments.

```typescript
type SearchOptions = [
  query: string,
  limit?: number,
  offset?: number
];

function search(...options: SearchOptions): void {
  const [query, limit, offset] = options;

  console.log({
    query,
    limit,
    offset,
  });
}

search("TypeScript");

search("TypeScript", 10);

search("TypeScript", 10, 20);
```

The function accepts one required argument and up to two optional arguments.

# 🗁 Optional Tuple Elements with Rest Parameters

A tuple type can be used to describe the parameters of a function.

```typescript
type LogArguments = [
  message: string,
  level?: "info" | "warning" | "error"
];

function log(...args: LogArguments): void {
  const [message, level = "info"] = args;

  console.log(`[${level.toUpperCase()}] ${message}`);
}

log("Application started");

log("Low memory", "warning");

log("Connection failed", "error");
```

Output:

```text
[INFO] Application started
[WARNING] Low memory
[ERROR] Connection failed
```

The default value is applied during destructuring when the optional element is omitted or `undefined`.

# 🗁 Optional Tuple Elements with Destructuring

You can destructure optional tuple elements just like regular tuple elements.

```typescript
type Product = [
  id: number,
  name: string,
  price?: number
];

const product: Product = [1, "Keyboard"];

const [id, name, price] = product;

console.log(id);
console.log(name);
console.log(price);
```

The `price` variable has the type `number | undefined`.

You can provide a default value:

```typescript
const [id, name, price = 0] = product;

console.log(price);
```

Now `price` is treated as a number after destructuring.

# 🗁 Optional Tuple Elements with Readonly Tuples

Optional elements can also be used in readonly tuples.

```typescript
type User = readonly [
  id: number,
  name: string,
  age?: number
];

const user1: User = [1, "Alice"];

const user2: User = [2, "Bob", 25];

// Type error:
user1[0] = 10;
```

Readonly prevents modifications through the tuple reference, while optional elements retain their optional behavior.

# 🗁 Optional Tuple Elements with Union Types

An optional tuple element can itself have a union type.

```typescript
type User = [
  id: number,
  name: string,
  age?: number | string
];

const user1: User = [1, "Alice"];

const user2: User = [2, "Bob", 25];

const user3: User = [3, "Charlie", "30"];
```

The third position may be omitted or contain either a number or a string.

When accessed, its type includes `undefined`.

```typescript
const age = user2[2];

// number | string | undefined
```

# 🗁 Practical Example: API Configuration

An API request may accept a required URL followed by optional configuration values.

```typescript
type RequestOptions = [
  url: string,
  method?: "GET" | "POST" | "PUT" | "DELETE",
  timeout?: number
];

function request(...options: RequestOptions): void {
  const [
    url,
    method = "GET",
    timeout = 5000,
  ] = options;

  console.log({
    url,
    method,
    timeout,
  });
}

request("/api/users");

request("/api/users", "POST");

request("/api/users", "GET", 10000);
```

This example demonstrates how optional tuple elements can model positional configuration.

For more complex configuration objects, an interface or object type is often easier to maintain.

# 🗁 Common Mistakes

**Placing required elements after optional elements**

Incorrect:

```typescript
type User = [
  id: number,
  age?: number,
  name: string
];
```

Correct:

```typescript
type User = [
  id: number,
  name: string,
  age?: number
];
```

**Assuming optional means the value is always available**

```typescript
type User = [number, string, number?];

const user: User = [1, "Alice"];

const age = user[2];

// age may be undefined.
```

Check the value before using it.

**Confusing optional elements with `undefined`**

```typescript
type A = [number, string?];

type B = [number, string | undefined];
```

`A` allows the second position to be omitted. `B` requires the second position to exist.

**Skipping an optional position without a placeholder**

If you want to provide a later optional element, earlier optional positions must still be represented.

```typescript
type User = [
  id: number,
  name: string,
  age?: number,
  email?: string
];

// Type error: email is in the fourth position.
const user: User = [1, "Alice", "alice@example.com"];
```

Provide `undefined` for the omitted age:

```typescript
const user: User = [
  1,
  "Alice",
  undefined,
  "alice@example.com",
];
```

# 🗁 Best Practices

* Use optional tuple elements when some trailing positional values are genuinely optional.
* Place required elements before optional elements.
* Handle `undefined` when accessing optional positions.
* Use default values during destructuring when appropriate.
* Prefer named tuple elements for readability.
* Use readonly tuples when the structure should not be modified.
* Prefer object types for complex configurations with many optional properties.
* Avoid long tuples that are difficult to understand from their positions alone.

# 🗁 Summary

* Optional tuple elements are declared using the `?` modifier.
* Optional elements can be omitted from the end of a tuple.
* Required tuple elements must appear before optional elements.
* Accessing an optional element produces a type that includes `undefined`.
* Optional tuple elements affect the possible tuple lengths.
* They work with rest parameters, destructuring, union types, and readonly tuples.
* A required element whose type includes `undefined` is different from an optional element.

# 🗁 Related Topics

* Arrays
* Readonly Arrays
* Tuples
* Rest Elements
* Named Tuples
* Optional Parameters
* Rest Parameters
* Union Types
* Destructuring
