# Literal Types

Literal types allow you to specify exact values that a variable, parameter, property, or return value can contain.

Unlike general types such as `string`, `number`, and `boolean`, literal types restrict values to specific constants.

For example:

```typescript
let username: string = "alice";
```

The variable can hold any string.

With a string literal type:

```typescript
let username: "alice" = "alice";
```

The variable can only contain the exact string `"alice"`.

Literal types are useful for:

* Defining fixed configuration values.
* Restricting function parameters.
* Representing application states.
* Defining HTTP methods.
* Creating status types.
* Building discriminated unions.
* Improving type safety.
* Modeling finite sets of possible values.

# 🗁 String Literal Types

A string literal type represents one exact string value.

```typescript
let direction: "north" = "north";
```

Valid assignment:

```typescript
direction = "north";
```

Invalid assignment:

```typescript
// TypeScript error
// direction = "south";
```

The variable can only contain `"north"`.

# 🗁 Multiple String Literal Types

Use a union (`|`) to allow multiple specific string values.

```typescript
type Direction = "north" | "south" | "east" | "west";

let direction: Direction = "north";

direction = "south";
direction = "east";
```

Invalid assignment:

```typescript
// TypeScript error
// direction = "up";
```

This approach creates a type containing a fixed set of allowed strings.

# 🗁 Number Literal Types

Number literal types restrict a value to an exact numeric value.

```typescript
let statusCode: 200 = 200;
```

Only the value `200` is accepted.

```typescript
statusCode = 200;

// TypeScript error
// statusCode = 404;
```

You can also define multiple allowed numeric values:

```typescript
type HttpStatusCode = 200 | 201 | 400 | 401 | 404 | 500;

let responseCode: HttpStatusCode = 200;

responseCode = 404;
responseCode = 500;
```

Invalid assignment:

```typescript
// TypeScript error
// responseCode = 301;
```

# 🗁 Boolean Literal Types

Boolean literal types restrict a value to exactly `true` or `false`.

```typescript
let isActive: true = true;
```

This variable cannot contain `false`.

```typescript
// TypeScript error
// isActive = false;
```

You can define a type containing either boolean value:

```typescript
type ToggleState = true | false;

let enabled: ToggleState = true;

enabled = false;
```

In practice, `boolean` is equivalent to the union `true | false`.

Boolean literal types become particularly useful when combined with other types.

# 🗁 BigInt Literal Types

BigInt literal types represent exact bigint values.

```typescript
const identifier: 100n = 100n;
```

Another bigint value is not assignable:

```typescript
// TypeScript error
// identifier = 200n;
```

You can also combine bigint literal types:

```typescript
type BigIntValue = 100n | 200n | 300n;

let value: BigIntValue = 100n;

value = 200n;
```

BigInt literal types are less common than string and number literal types.

# 🗁 Literal Types with Type Aliases

Type aliases make literal types easier to reuse.

```typescript
type UserRole = "admin" | "editor" | "viewer";

let role: UserRole = "admin";
```

Use the same type in multiple places:

```typescript
function assignRole(role: UserRole): void {
  console.log(`Assigned role: ${role}`);
}

assignRole("editor");
assignRole("viewer");
```

Invalid usage:

```typescript
// TypeScript error
// assignRole("superadmin");
```

# 🗁 Literal Types with Function Parameters

Literal types can restrict function arguments.

```typescript
function setTheme(theme: "light" | "dark"): void {
  console.log(`Theme changed to ${theme}`);
}

setTheme("light");
setTheme("dark");
```

Invalid argument:

```typescript
// TypeScript error
// setTheme("blue");
```

This is useful when a function supports only a fixed set of options.

# 🗁 Literal Types with Return Types

Functions can return specific literal types.

```typescript
function getStatus(): "success" {
  return "success";
}
```

The return type is exactly `"success"`, not any string.

You can also use a union return type:

```typescript
function getResult(): "success" | "failure" {
  return Math.random() > 0.5 ? "success" : "failure";
}
```

# 🗁 Literal Types with Objects

Literal types can restrict object property values.

```typescript
type User = {
  name: string;
  role: "admin" | "editor" | "viewer";
  status: "active" | "inactive";
};

const user: User = {
  name: "Alice",
  role: "admin",
  status: "active",
};
```

Invalid property values are rejected:

```typescript
const anotherUser: User = {
  name: "Bob",

  // TypeScript error
  role: "superadmin",

  status: "active",
};
```

# 🗁 Literal Types with Interfaces

Interfaces can also define literal property types.

```typescript
interface Button {
  label: string;
  variant: "primary" | "secondary" | "danger";
  disabled: boolean;
}
```

Usage:

```typescript
const submitButton: Button = {
  label: "Submit",
  variant: "primary",
  disabled: false,
};
```

# 🗁 Literal Types with Arrays

Literal types can restrict array elements.

```typescript
type Direction = "north" | "south" | "east" | "west";

const directions: Direction[] = [
  "north",
  "south",
  "east",
];
```

Invalid element:

```typescript
const invalidDirections: Direction[] = [
  "north",
  "south",

  // TypeScript error
  "up",
];
```

You can also define a tuple containing specific literal values:

```typescript
type Coordinates = [number, number];

type FixedPosition = ["top", "left"];

const position: FixedPosition = ["top", "left"];
```

The tuple must contain the specified values in the correct order.

# 🗁 Literal Types and Type Inference

TypeScript automatically infers types from assigned values.

However, inference behavior depends on whether a value is declared with `const` or `let`.

With `const`:

```typescript
const status = "active";
```

TypeScript infers:

```typescript
// status: "active"
```

A `const` variable cannot be reassigned, so its inferred type can remain the exact literal.

With `let`:

```typescript
let status = "active";
```

TypeScript generally infers:

```typescript
// status: string
```

A `let` variable can be reassigned, so TypeScript widens the inferred literal type to its general primitive type.

# 🗁 Literal Type Widening

Literal type widening is the process of converting a specific literal type into a broader primitive type.

Example:

```typescript
const name = "Alice";

let username = name;
```

TypeScript infers:

```typescript
// name: "Alice"
// username: string
```

The `const` variable retains its literal type, while the `let` variable receives the broader `string` type.

Other examples:

```typescript
const count = 10;
let total = count;

// count: 10
// total: number

const enabled = true;
let isEnabled = enabled;

// enabled: true
// isEnabled: boolean
```

Understanding widening is important when working with literal types and function arguments.

# 🗁 Literal Types and Const Assertions

The `as const` assertion prevents literal values from being widened and makes object properties and array elements readonly.

Example:

```typescript
const user = {
  name: "Alice",
  role: "admin",
} as const;
```

Inferred type:

```typescript
{
  readonly name: "Alice";
  readonly role: "admin";
}
```

Without `as const`, the properties would generally be inferred as `string`.

Another example:

```typescript
const colors = ["red", "green", "blue"] as const;
```

Inferred type:

```typescript
readonly ["red", "green", "blue"]
```

This preserves the exact values and tuple structure.

# 🗁 Literal Types with Discriminated Unions

Literal types are commonly used to build discriminated unions.

A discriminated union uses a shared property containing distinct literal values to identify each possible object shape.

```typescript
type SuccessResponse = {
  status: "success";
  data: string;
};

type ErrorResponse = {
  status: "error";
  message: string;
};

type ApiResponse = SuccessResponse | ErrorResponse;
```

Handle each case using the discriminant:

```typescript
function handleResponse(response: ApiResponse): void {
  if (response.status === "success") {
    console.log(response.data);
  } else {
    console.log(response.message);
  }
}
```

TypeScript narrows the type based on the value of `status`.

# 🗁 Literal Types with Switch Statements

Literal types work naturally with switch statements.

```typescript
type PaymentStatus =
  | "pending"
  | "processing"
  | "completed"
  | "failed";

function getPaymentMessage(status: PaymentStatus): string {
  switch (status) {
    case "pending":
      return "Payment is waiting";

    case "processing":
      return "Payment is processing";

    case "completed":
      return "Payment completed";

    case "failed":
      return "Payment failed";
  }
}
```

TypeScript understands the possible values and can help identify missing cases.

# 🗁 Literal Types with Generic Functions

Generic functions can preserve literal types when their type parameters are inferred appropriately.

```typescript
function identity<T>(value: T): T {
  return value;
}

const result = identity("hello");
```

The inferred type of `result` is `string` in this example because the argument is a string expression that is widened during generic inference.

You can explicitly provide a literal type:

```typescript
const result = identity<"hello">("hello");
```

Now the result has the exact type `"hello"`.

You can also use a `const` type parameter to preserve literal information during inference:

```typescript
function preserve<const T>(value: T): T {
  return value;
}

const result = preserve("hello");
```

The inferred type of `result` is `"hello"`.

# 🗁 Literal Types with the Satisfies Operator

The `satisfies` operator checks whether an expression conforms to a type without replacing the expression's inferred type with that target type.

```typescript
type Config = {
  mode: "development" | "production";
  port: number;
};

const config = {
  mode: "development",
  port: 3000,
} satisfies Config;
```

The object is checked against `Config`, while its inferred property types are preserved as far as normal inference allows.

For an exact literal type on a property, combine it with `as const` when appropriate:

```typescript
const config = {
  mode: "development",
  port: 3000,
} as const satisfies Config;
```

The `mode` property retains the literal type `"development"` and the properties are readonly.

# 🗁 Literal Types with Template Literal Types

Template literal types build new string types from existing literal types.

```typescript
type Size = "small" | "medium" | "large";

type ButtonClass = `button-${Size}`;
```

The resulting type is equivalent to:

```typescript
type ButtonClass =
  | "button-small"
  | "button-medium"
  | "button-large";
```

Usage:

```typescript
const buttonClass: ButtonClass = "button-small";
```

Invalid assignment:

```typescript
// TypeScript error
// const invalidClass: ButtonClass = "button-extra-large";
```

Template literal types are useful for generating structured string types.

# 🗁 Literal Types with Enums

Literal types and enums can both represent fixed sets of values.

Enum:

```typescript
enum Status {
  Pending = "pending",
  Approved = "approved",
  Rejected = "rejected",
}

let status: Status = Status.Pending;
```

Literal union:

```typescript
type Status =
  | "pending"
  | "approved"
  | "rejected";

let status: Status = "pending";
```

Use enums when a runtime enum object is useful. Use literal unions when you only need compile-time restrictions.

# 🗁 Common Mistakes

**Using a general type instead of a literal type**

```typescript
let direction: string = "north";
```

This accepts any string.

Use a literal union when only specific values are allowed:

```typescript
type Direction = "north" | "south";
```

**Forgetting literal type widening**

```typescript
let status = "active";
```

The inferred type is `string`, not `"active"`.

Use an explicit annotation or `as const` when exact literal types are needed.

**Assigning an invalid literal**

```typescript
type Role = "admin" | "editor";

// TypeScript error
// const role: Role = "viewer";
```

Only declared values are accepted.

**Expecting `as const` to validate external data**

`as const` preserves types at compile time. It does not perform runtime validation.

**Confusing literal types with runtime constants**

A literal type exists only in the TypeScript type system. It does not create a runtime validation mechanism.

# 🗁 Best Practices

* Use literal unions for fixed sets of allowed values.
* Prefer descriptive type aliases for reusable literal types.
* Use literal types for function parameters and object properties when values are restricted.
* Understand literal type widening.
* Use `as const` when exact literal values and readonly properties are required.
* Use discriminated unions to model different object states.
* Use exhaustive checking when handling all possible literal values.
* Use template literal types when building structured string types.
* Validate external data at runtime instead of relying on compile-time types.
* Prefer literal unions over enums when no runtime enum object is needed.

# 🗁 Summary

* Literal types represent exact values rather than broad primitive types.
* TypeScript supports string, number, boolean, and bigint literal types.
* Union types combine multiple allowed literal values.
* Literal types can be used in variables, functions, objects, interfaces, and arrays.
* TypeScript may widen literal types from specific values to general primitive types.
* `const` declarations generally preserve primitive literal types.
* `as const` preserves literal values and makes object properties and arrays readonly.
* Literal types are fundamental to discriminated unions.
* Template literal types build new string types from existing literals.
* Runtime validation is still necessary for external data.

# 🗁 Related Topics

* Basic Types
* Union Types
* Type Aliases
* Type Inference
* Literal Type Widening
* Const Assertions
* Satisfies Operator
* Template Literal Types
* Discriminated Unions
* Exhaustiveness Checking
* Enums
