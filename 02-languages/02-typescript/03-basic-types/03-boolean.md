# TypeScript Boolean Type

The `boolean` type represents logical values in TypeScript. It has only two possible values: `true` and `false`.

Boolean values are commonly used in conditional statements, comparisons, authentication checks, application settings, feature flags, and logical operations.

TypeScript provides static type checking to ensure that variables declared as `boolean` contain only Boolean values.

# 🗁 Declaring Boolean Variables

Use the `boolean` keyword to explicitly declare a Boolean variable.

```typescript
let isActive: boolean = true;
let isLoggedIn: boolean = false;
let hasPermission: boolean = true;

console.log(isActive);
console.log(isLoggedIn);
console.log(hasPermission);
```

TypeScript also supports type inference.

```typescript
let isOnline = true;

// TypeScript infers isOnline as boolean.
isOnline = false;

// TypeScript error
// isOnline = "true";
```

# 🗁 Boolean Literal Values

Boolean values are represented by two lowercase keywords:

* `true`
* `false`

```typescript
const isAvailable: boolean = true;
const isDeleted: boolean = false;
```

Boolean values are not strings or numbers.

```typescript
const isActive: boolean = true;

// These are not Boolean values:
// const value1: boolean = "true";
// const value2: boolean = 1;
```

# 🗁 Boolean Values with Comparison Operators

Comparison operators return Boolean values.

```typescript
const age: number = 25;

const isAdult: boolean = age >= 18;
const isMinor: boolean = age < 18;

console.log(isAdult); // true
console.log(isMinor); // false
```

Other comparison examples:

```typescript
const a: number = 10;
const b: number = 20;

console.log(a === b); // false
console.log(a !== b); // true
console.log(a > b);   // false
console.log(a < b);   // true
```

# 🗁 Boolean Values with Logical Operators

Logical operators combine or invert Boolean expressions.

| Operator | Name | Description |
| --- | --- | --- |
| `&&` | Logical AND | True when both operands are truthy |
| `\|\|` | Logical OR | True when at least one operand is truthy |
| `!` | Logical NOT | Reverses the truthiness of an operand |

**Logical AND (`&&`)**

Returns a truthy result only when both operands are truthy.

```typescript
const hasAccount: boolean = true;
const isVerified: boolean = true;

const canAccess: boolean = hasAccount && isVerified;

console.log(canAccess); // true
```

**Logical OR (`||`)**

Returns a truthy result when at least one operand is truthy.

```typescript
const isAdmin: boolean = false;
const isEditor: boolean = true;

const canManageContent: boolean = isAdmin || isEditor;

console.log(canManageContent); // true
```

**Logical NOT (`!`)**

Reverses the truthiness of an operand.

```typescript
const isLoggedIn: boolean = false;

const isLoggedOut: boolean = !isLoggedIn;

console.log(isLoggedOut); // true
```

When both operands are Boolean values, these operators produce Boolean results. In JavaScript, logical operators can also return non-Boolean operands, so their behavior is not limited to Boolean inputs.

# 🗁 Boolean Type with Conditional Statements

Boolean expressions are commonly used with `if`, `else if`, and `else` statements.

```typescript
const isAuthenticated: boolean = true;

if (isAuthenticated) {
    console.log("Welcome to the application");
} else {
    console.log("Please log in");
}
```

Using multiple conditions:

```typescript
const age: number = 25;
const hasPermission: boolean = true;

if (age >= 18 && hasPermission) {
    console.log("Access granted");
} else {
    console.log("Access denied");
}
```

# 🗁 Boolean Type with Ternary Operator

The ternary operator provides a concise way to select a value based on a condition.

```typescript
const isLoggedIn: boolean = true;

const message: string = isLoggedIn
    ? "Welcome back"
    : "Please log in";

console.log(message); // Welcome back
```

It is useful for conditional assignments and simple expressions.

# 🗁 Boolean Type with Functions

Functions can accept Boolean parameters and return Boolean values.

**Boolean parameter:**

```typescript
function toggleStatus(isActive: boolean): boolean {
    return !isActive;
}

console.log(toggleStatus(true));  // false
console.log(toggleStatus(false)); // true
```

**Boolean return type:**

```typescript
function isAdult(age: number): boolean {
    return age >= 18;
}

console.log(isAdult(25)); // true
console.log(isAdult(15)); // false
```

**Boolean callback:**

```typescript
function filterUsers(
    users: string[],
    condition: (user: string) => boolean
): string[] {
    return users.filter(condition);
}

const users: string[] = ["John", "Jane", "David"];

const filteredUsers = filterUsers(
    users,
    (user) => user.startsWith("J")
);

console.log(filteredUsers); // ["John", "Jane"]
```

# 🗁 Boolean Type with Arrays

Boolean arrays store multiple Boolean values.

```typescript
const permissions: boolean[] = [
    true,
    false,
    true,
    true
];

console.log(permissions);
```

You can also use generic array syntax.

```typescript
const statuses: Array<boolean> = [
    true,
    false,
    true
];
```

Boolean arrays are useful for representing multiple flags, permissions, and validation results.

# 🗁 Boolean Type with Objects

Boolean properties are commonly used to represent object states.

```typescript
interface User {
    id: number;
    name: string;
    isActive: boolean;
    isVerified: boolean;
    isAdmin: boolean;
}

const user: User = {
    id: 1,
    name: "John",
    isActive: true,
    isVerified: true,
    isAdmin: false
};
```

You can use these properties in conditional expressions.

```typescript
if (user.isActive && user.isVerified) {
    console.log("User can access the application");
}
```

# 🗁 Boolean Type with Union Types

Boolean values can be combined with other types using union types.

```typescript
let status: boolean | string;

status = true;
status = false;
status = "pending";

// TypeScript error
// status = 100;
```

This is useful when an application supports both Boolean states and descriptive status values.

However, when modeling complex states, a dedicated union of string literals may provide better clarity.

```typescript
type RequestStatus = "pending" | "success" | "failed";

let requestStatus: RequestStatus = "pending";
```

# 🗁 Boolean Type and Truthiness

JavaScript evaluates values according to their truthiness in conditional expressions.

Some values are falsy:

* `false`
* `0`
* `-0`
* `0n`
* `""`
* `null`
* `undefined`
* `NaN`

Most other values are truthy, including empty arrays (`[]`) and empty objects (`{}`).

Example:

```typescript
if ("Hello") {
    console.log("This executes");
}

if (0) {
    console.log("This does not execute");
}

if ([]) {
    console.log("An empty array is truthy");
}
```

TypeScript uses JavaScript's runtime truthiness rules.

# 🗁 Boolean Conversion

JavaScript provides several ways to convert values into Boolean values.

**Using `Boolean()`:**

```typescript
const value1: boolean = Boolean(1);
const value2: boolean = Boolean(0);
const value3: boolean = Boolean("");
const value4: boolean = Boolean("Hello");

console.log(value1); // true
console.log(value2); // false
console.log(value3); // false
console.log(value4); // true
```

**Using double negation (`!!`):**

```typescript
const value1: boolean = !!"Hello";
const value2: boolean = !!0;

console.log(value1); // true
console.log(value2); // false
```

Both approaches convert a value according to JavaScript truthiness rules.

# 🗁 Boolean Conversion vs Boolean Type Checking

Converting a value to Boolean is different from checking whether a value is already a Boolean.

Conversion:

```typescript
const value: boolean = Boolean("Hello");

console.log(value); // true
```

Type checking:

```typescript
const value: unknown = true;

if (typeof value === "boolean") {
    console.log("The value is a Boolean");
}
```

Use `typeof` when validating an unknown value.

# 🗁 Boolean Type with API Responses

API responses frequently contain Boolean properties.

```typescript
interface ApiResponse {
    success: boolean;
    message: string;
    authenticated: boolean;
}

const response: ApiResponse = {
    success: true,
    message: "Login successful",
    authenticated: true
};

if (response.success && response.authenticated) {
    console.log(response.message);
}
```

When receiving external data, remember that a TypeScript interface does not validate the actual runtime response. Use runtime validation when the data must be trusted.

# 🗁 Boolean Type with Optional Properties

Optional Boolean properties may be absent.

```typescript
interface Settings {
    darkMode?: boolean;
    notifications?: boolean;
}

const settings: Settings = {
    darkMode: true
};
```

An optional Boolean property can have three relevant states:

* `true`
* `false`
* `undefined` when omitted

Example:

```typescript
function displaySettings(settings: Settings): void {
    if (settings.darkMode === undefined) {
        console.log("Dark mode preference is not configured");
    } else if (settings.darkMode) {
        console.log("Dark mode enabled");
    } else {
        console.log("Dark mode disabled");
    }
}
```

Explicit comparisons are useful when distinguishing `false` from an absent value.

# 🗁 Best Practices

* Use the `boolean` type for logical values.
* Use `true` and `false` instead of string representations such as `"true"` and `"false"`.
* Use meaningful Boolean variable names such as `isActive`, `hasPermission`, and `canAccess`.
* Prefer explicit comparisons when distinguishing `false` from `undefined`.
* Use logical operators to combine Boolean conditions.
* Avoid unnecessary Boolean conversions.
* Do not use `any` for values that should be Boolean.
* Validate external API data at runtime when necessary.
* Use string literal unions when modeling multiple descriptive states.
* Keep Boolean expressions simple and readable.

# 🗁 Summary

The TypeScript `boolean` type represents logical values and supports only two values: `true` and `false`.

Important concepts include:

* Boolean declarations and type inference.
* Boolean literal values.
* Comparison and logical operators.
* Conditional statements and ternary expressions.
* Boolean parameters and return types.
* Boolean arrays and object properties.
* Truthiness and Boolean conversion.
* Optional Boolean properties.
* Boolean values in API responses.

Boolean values are fundamental to application logic because they allow programs to make decisions, evaluate conditions, control execution, and represent application states.
