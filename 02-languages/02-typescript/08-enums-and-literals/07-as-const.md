# As Const

`as const` is a TypeScript const assertion that preserves the most specific literal types of an expression and makes its properties readonly.

It prevents TypeScript from widening literal values into broader types such as `string`, `number`, and `boolean`.

It also makes object properties readonly and converts array literals into readonly tuples.

`as const` is useful for:

* Preserving exact string and numeric values.
* Creating readonly configuration objects.
* Defining fixed collections of values.
* Generating union types from arrays.
* Creating strongly typed application constants.
* Defining action types in state management.
* Modeling API configuration.
* Building type-safe lookup tables.

Example:

```typescript
const status = "active" as const;
```

TypeScript infers:

```typescript
// type: "active"
```

Instead of:

```typescript
// type: string
```

# 🗁 Basic Syntax

Use `as const` after a literal expression.

```typescript
const value = expression as const;
```

Examples:

```typescript
const username = "alice" as const;
const age = 25 as const;
const isActive = true as const;
```

Inferred types:

```typescript
// username: "alice"
// age: 25
// isActive: true
```

These variables retain their exact literal types.

# 🗁 Literal Type Preservation

Normally, TypeScript may widen literal values depending on how they are declared.

Without `as const`:

```typescript
let status = "active";
```

Inferred type:

```typescript
// status: string
```

With `as const`:

```typescript
let status = "active" as const;
```

Inferred type:

```typescript
// status: "active"
```

The variable can be reassigned, but only to a value compatible with its literal type.

```typescript
let status = "active" as const;

// Valid
status = "active";

// TypeScript error
// status = "inactive";
```

# 🗁 Readonly Object Properties

When `as const` is applied to an object literal, its properties become readonly.

Without `as const`:

```typescript
const user = {
  name: "Alice",
  age: 25,
};

user.name = "Bob";
user.age = 30;
```

These assignments are allowed because the properties are mutable.

With `as const`:

```typescript
const user = {
  name: "Alice",
  age: 25,
} as const;
```

Inferred type:

```typescript
{
  readonly name: "Alice";
  readonly age: 25;
}
```

Attempting to modify a property produces a TypeScript error:

```typescript
// TypeScript error
// user.name = "Bob";

// TypeScript error
// user.age = 30;
```

# 🗁 Readonly Arrays

Applying `as const` to an array literal makes it a readonly tuple.

Without `as const`:

```typescript
const colors = ["red", "green", "blue"];
```

Inferred type:

```typescript
string[]
```

With `as const`:

```typescript
const colors = ["red", "green", "blue"] as const;
```

Inferred type:

```typescript
readonly ["red", "green", "blue"]
```

The array retains its exact length, order, and element types.

Mutation methods are unavailable:

```typescript
// TypeScript error
// colors.push("yellow");

// TypeScript error
// colors.pop();

// TypeScript error
// colors[0] = "yellow";
```

# 🗁 Readonly Tuples

`as const` automatically creates a readonly tuple from an array literal.

```typescript
const coordinates = [10, 20] as const;
```

Inferred type:

```typescript
readonly [10, 20]
```

Each position has an exact literal type.

```typescript
const x = coordinates[0];
const y = coordinates[1];

// x: 10
// y: 20
```

The tuple cannot be modified.

# 🗁 Nested Objects and Arrays

Const assertions also apply recursively to nested literal structures.

```typescript
const application = {
  name: "My Application",
  version: "1.0.0",
  settings: {
    theme: "dark",
    notifications: true,
  },
  features: ["authentication", "dashboard"],
} as const;
```

Inferred type:

```typescript
{
  readonly name: "My Application";
  readonly version: "1.0.0";
  readonly settings: {
    readonly theme: "dark";
    readonly notifications: true;
  };
  readonly features: readonly [
    "authentication",
    "dashboard"
  ];
}
```

Nested properties and array elements are readonly in the inferred type.

# 🗁 Creating Union Types from Arrays

One of the most useful applications of `as const` is generating union types from arrays.

Example:

```typescript
const roles = [
  "admin",
  "editor",
  "viewer",
] as const;
```

Create a union type:

```typescript
type Role = (typeof roles)[number];
```

The resulting type is:

```typescript
type Role = "admin" | "editor" | "viewer";
```

Usage:

```typescript
let role: Role = "admin";

role = "editor";
role = "viewer";
```

Invalid assignment:

```typescript
// TypeScript error
// role = "superadmin";
```

This pattern keeps the runtime values and compile-time type synchronized.

# 🗁 Understanding typeof and Indexed Access

The following expression combines two TypeScript operators:

```typescript
type Role = (typeof roles)[number];
```

Break it down:

**typeof**

Gets the type of the `roles` variable.

```typescript
type RolesArray = typeof roles;
```

Result:

```typescript
readonly ["admin", "editor", "viewer"]
```

**Indexed access with number**

Accesses the type of the array's elements.

```typescript
type Role = RolesArray[number];
```

Result:

```typescript
"admin" | "editor" | "viewer"
```

This technique works with readonly tuples and ordinary arrays.

# 🗁 Creating Union Types from Objects

You can use `as const` with objects and derive union types from their values.

```typescript
const Status = {
  Pending: "pending",
  Approved: "approved",
  Rejected: "rejected",
} as const;
```

Create a union type:

```typescript
type Status = (typeof Status)[keyof typeof Status];
```

Result:

```typescript
type Status =
  | "pending"
  | "approved"
  | "rejected";
```

Usage:

```typescript
const currentStatus: Status = "approved";
```

This pattern provides a runtime object and a compile-time union type without using an enum.

# 🗁 Using as Const for Application Configuration

`as const` is useful for application configuration objects.

```typescript
const config = {
  environment: "production",
  apiUrl: "https://api.example.com",
  port: 3000,
  logging: true,
} as const;
```

The inferred type preserves the exact values:

```typescript
{
  readonly environment: "production";
  readonly apiUrl: "https://api.example.com";
  readonly port: 3000;
  readonly logging: true;
}
```

Access the configuration:

```typescript
console.log(config.environment);
console.log(config.apiUrl);
console.log(config.port);
```

The configuration cannot be reassigned through its readonly properties.

# 🗁 Using as Const for HTTP Methods

You can define HTTP methods using a const object.

```typescript
const HttpMethod = {
  Get: "GET",
  Post: "POST",
  Put: "PUT",
  Patch: "PATCH",
  Delete: "DELETE",
} as const;
```

Create a union type:

```typescript
type HttpMethod =
  (typeof HttpMethod)[keyof typeof HttpMethod];
```

Use it in a function:

```typescript
function request(method: HttpMethod, url: string): void {
  console.log(`${method} ${url}`);
}

request(HttpMethod.Get, "/users");
request(HttpMethod.Post, "/users");
```

Invalid usage:

```typescript
// TypeScript error
// request("OPTIONS", "/users");
```

# 🗁 Using as Const for State Management

Const assertions are useful for defining action objects in state management.

```typescript
const increment = {
  type: "counter/increment",
} as const;

const decrement = {
  type: "counter/decrement",
} as const;
```

Create an action union:

```typescript
type CounterAction =
  | typeof increment
  | typeof decrement;
```

Handle actions:

```typescript
function reducer(
  state: number,
  action: CounterAction,
): number {
  switch (action.type) {
    case "counter/increment":
      return state + 1;

    case "counter/decrement":
      return state - 1;
  }
}
```

The exact `type` values allow TypeScript to narrow the action automatically.

# 🗁 Using as Const with Discriminated Unions

Const assertions preserve discriminant values in object literals.

```typescript
const successResponse = {
  status: "success",
  data: "User loaded",
} as const;

const errorResponse = {
  status: "error",
  message: "User not found",
} as const;
```

Create a union:

```typescript
type ApiResponse =
  | typeof successResponse
  | typeof errorResponse;
```

Handle each response:

```typescript
function handleResponse(response: ApiResponse): void {
  if (response.status === "success") {
    console.log(response.data);
  } else {
    console.log(response.message);
  }
}
```

The literal `status` values make the union discriminated.

# 🗁 Using as Const with Functions

Const assertions can preserve literal information when working with function arguments and return values.

Example:

```typescript
function getStatus() {
  return {
    status: "success",
  } as const;
}
```

The inferred return type is:

```typescript
{
  readonly status: "success";
}
```

This is useful when a function returns a fixed object shape with specific literal values.

# 🗁 Const Assertions on Imported Values

You can use const assertions with values imported from another module, provided the expression is eligible for a const assertion.

Example:

```typescript
import { config } from "./config.js";

const localConfig = {
  ...config,
  mode: "production",
} as const;
```

The newly created object literal receives readonly properties and preserved literal types where inference permits.

However, `as const` does not retroactively make the original imported object deeply readonly.

# 🗁 Const Assertions and Runtime Immutability

An important distinction is that `as const` affects TypeScript's type system, not JavaScript runtime behavior.

Example:

```typescript
const user = {
  name: "Alice",
} as const;
```

TypeScript prevents this assignment:

```typescript
// TypeScript error
// user.name = "Bob";
```

But the emitted JavaScript does not automatically call `Object.freeze()`.

If the code is executed without TypeScript's type checking, the object is not inherently frozen.

For runtime immutability, use `Object.freeze()`:

```typescript
const user = Object.freeze({
  name: "Alice",
  role: "admin",
});
```

`Object.freeze()` is shallow: nested objects are not automatically frozen.

# 🗁 as Const vs const Declaration

These concepts are related but different.

**Const declaration**

Prevents reassignment of a variable binding.

```typescript
const user = {
  name: "Alice",
};

user.name = "Bob"; // Allowed
```

The variable cannot point to a different object, but its properties remain mutable.

**Const assertion**

Preserves literal types and makes literal object properties readonly.

```typescript
const user = {
  name: "Alice",
} as const;

// TypeScript error
// user.name = "Bob";
```

Comparison:

| Feature | `const` Declaration | `as const` Assertion |
| --- | --- | --- |
| Prevents variable reassignment | Yes | No |
| Preserves literal types | Generally for primitive bindings | Yes |
| Makes object properties readonly | No | Yes |
| Creates readonly tuples | No | Yes |
| Runtime freezing | No | No |

# 🗁 as Const vs Readonly

Both can make properties readonly, but they are used differently.

Using `as const`:

```typescript
const user = {
  name: "Alice",
  age: 25,
} as const;
```

Using `Readonly`:

```typescript
type User = {
  name: string;
  age: number;
};

type ReadonlyUser = Readonly<User>;
```

Comparison:

| Feature | `as const` | `Readonly<T>` |
| --- | --- | --- |
| Purpose | Preserve literal values and readonly structure | Make existing properties readonly |
| Preserves literal types | Yes | No, not by itself |
| Supports object literals | Yes | Applied to a type |
| Creates readonly tuples | Yes | Requires an appropriate tuple type |
| Runtime immutability | No | No |

Example:

```typescript
const first = {
  status: "active",
} as const;

type Second = Readonly<{
  status: string;
}>;
```

The first object's property has the exact type `"active"`, while the second type's property remains `string`.

# 🗁 as Const vs Enums

Both const objects and enums can represent fixed sets of named values.

Using an enum:

```typescript
enum Status {
  Pending = "pending",
  Approved = "approved",
  Rejected = "rejected",
}
```

Using a const object:

```typescript
const Status = {
  Pending: "pending",
  Approved: "approved",
  Rejected: "rejected",
} as const;

type Status =
  (typeof Status)[keyof typeof Status];
```

Comparison:

| Feature | Enum | Const Object |
| --- | --- | --- |
| Runtime object | Yes | Yes |
| Named members | Yes | Yes |
| Union from values | Enum type | `typeof` and `keyof` |
| Readonly properties | Not the same as `as const` | Yes |
| Reverse mapping | Numeric enums only | Must be implemented |
| JavaScript compatibility | Requires enum compilation | Ordinary JavaScript object |

Const objects are often a flexible alternative when you need runtime values and a union type.

# 🗁 Common Mistakes

**Assuming const variables automatically make objects readonly**

```typescript
const settings = {
  theme: "dark",
};

settings.theme = "light"; // Allowed
```

Use `as const` if the properties should be readonly.

**Expecting runtime freezing**

`as const` does not call `Object.freeze()`.

**Confusing readonly with deep runtime immutability**

Const assertions provide readonly types for literal structures, but they do not guarantee runtime immutability.

**Using as Const on arbitrary expressions**

Const assertions are intended for literal expressions and certain eligible expressions. They cannot be applied indiscriminately to every computed expression.

**Forgetting indexed access when creating unions**

```typescript
const colors = ["red", "green", "blue"] as const;

type Color = (typeof colors)[number];
```

Using `typeof colors` alone produces the readonly tuple type, not the union of its element values.

# 🗁 Best Practices

* Use `as const` for fixed application constants.
* Use it when exact literal types must be preserved.
* Use readonly tuples for fixed collections.
* Generate union types from const arrays and objects.
* Use const objects when both runtime values and compile-time types are needed.
* Use `satisfies` with `as const` when validating configuration shapes while preserving literal information.
* Use `Object.freeze()` when runtime freezing is required.
* Avoid unnecessary const assertions when ordinary type inference is sufficient.
* Keep runtime immutability and compile-time readonly behavior conceptually separate.

# 🗁 Summary

* `as const` is a TypeScript const assertion.
* It prevents literal type widening.
* Object properties become readonly.
* Array literals become readonly tuples.
* Nested literal structures retain their specific values and readonly types.
* It can generate union types from arrays and objects.
* It is useful for configuration, HTTP methods, state management, and discriminated unions.
* It does not provide runtime immutability.
* `Object.freeze()` can provide shallow runtime freezing.
* Const objects with `as const` are a flexible alternative to enums.

# 🗁 Related Topics

* Literal Types
* Type Inference
* Literal Type Widening
* Readonly Properties
* Readonly Arrays
* Tuples
* Typeof Operator
* Keyof Operator
* Indexed Access Types
* Satisfies Operator
* Enums
* Union Types
* Discriminated Unions
* Utility Types
