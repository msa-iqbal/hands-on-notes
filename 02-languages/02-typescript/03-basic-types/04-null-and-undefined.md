# TypeScript Null and Undefined

In TypeScript, `null` and `undefined` represent the absence of a meaningful value. Although they are related, they have different meanings and behaviors.

* `null` represents an intentional absence of a value.
* `undefined` generally represents a value that has not been assigned or is unavailable.

Understanding these types is important for handling optional properties, function parameters, API responses, database records, and application state.

# 🗁 Understanding Undefined

The `undefined` value indicates that a variable or property has not been assigned a value.

```typescript
let username: undefined = undefined;

console.log(username); // undefined
```

A declared variable without an initial value also contains `undefined` at runtime.

```typescript
let age: number | undefined;

console.log(age); // undefined

age = 25;

console.log(age); // 25
```

When `strictNullChecks` is enabled, a variable declared as `number` cannot be assigned `undefined` unless its type explicitly includes it.

# 🗁 Understanding Null

The `null` value represents an intentional absence of a value.

```typescript
let username: null = null;

console.log(username); // null
```

It is commonly used when a value is deliberately empty or unavailable.

```typescript
let selectedUser: string | null = null;

selectedUser = "John";

console.log(selectedUser); // John
```

With `strictNullChecks` enabled, `null` must be explicitly included in the declared type.

# 🗁 Null vs Undefined

Although both represent missing values, they have different meanings.

| Feature | `null` | `undefined` |
| --- | --- | --- |
| Meaning | Intentional absence | Missing or unassigned value |
| TypeScript type | `null` | `undefined` |
| Common usage | Explicitly empty value | Uninitialized or missing value |
| Default function parameter | Does not trigger default | Triggers default |
| JSON object serialization | Preserved as `null` | Property is omitted |
| JSON array serialization | Preserved as `null` | Converted to `null` |

Example:

```typescript
let firstName: string | null = null;
let lastName: string | undefined = undefined;

console.log(firstName); // null
console.log(lastName);  // undefined
```

# 🗁 Strict Null Checks

TypeScript provides the `strictNullChecks` compiler option to control how `null` and `undefined` are handled.

When enabled, `null` and `undefined` are treated as separate types and cannot be assigned to ordinary types without explicitly including them.

Example with strict null checks:

```typescript
let username: string = "John";

// TypeScript error
// username = null;

// TypeScript error
// username = undefined;
```

To allow both values:

```typescript
let username: string | null | undefined;

username = "John";
username = null;
username = undefined;
```

Enable strict null checking in `tsconfig.json`:

```json
{
    "compilerOptions": {
        "strictNullChecks": true
    }
}
```

The `strict` compiler option also enables `strictNullChecks`.

```json
{
    "compilerOptions": {
        "strict": true
    }
}
```

Using strict null checking helps prevent runtime errors caused by accessing properties or methods on missing values.

# 🗁 Undefined Variables

Variables declared without initial values can contain `undefined` at runtime.

```typescript
let count: number;

count = 10;

console.log(count); // 10
```

Reading a local variable before assigning it produces a TypeScript error under normal definite-assignment checking.

```typescript
let age: number;

// TypeScript error: variable used before being assigned.
// console.log(age);

age = 25;

console.log(age);
```

For variables that are intentionally allowed to remain uninitialized, include `undefined` in their type.

```typescript
let selectedAge: number | undefined;

console.log(selectedAge); // undefined

selectedAge = 25;
```

# 🗁 Optional Object Properties

The optional property modifier (`?`) allows an object property to be omitted.

```typescript
interface User {
    name: string;
    age?: number;
}

const user1: User = {
    name: "John"
};

const user2: User = {
    name: "Jane",
    age: 25
};
```

When reading an optional property, TypeScript considers that it may be `undefined`.

```typescript
const user: User = {
    name: "John"
};

const age = user.age;

// age has the type number | undefined.
console.log(age);
```

With `strictNullChecks` enabled, assigning `undefined` to an optional property is generally allowed unless `exactOptionalPropertyTypes` is enabled.

# 🗁 Exact Optional Property Types

TypeScript provides the `exactOptionalPropertyTypes` compiler option to distinguish between an omitted property and a property explicitly assigned `undefined`.

Consider this interface:

```typescript
interface User {
    name: string;
    age?: number;
}
```

Without `exactOptionalPropertyTypes`, the following assignment is allowed:

```typescript
const user: User = {
    name: "John",
    age: undefined
};
```

With `exactOptionalPropertyTypes` enabled, explicitly assigning `undefined` is rejected because the property type only declares `number` when present.

```json
{
    "compilerOptions": {
        "strict": true,
        "exactOptionalPropertyTypes": true
    }
}
```

If both a number and explicit `undefined` should be accepted, declare the property accordingly:

```typescript
interface User {
    name: string;
    age?: number | undefined;
}

const user: User = {
    name: "John",
    age: undefined
};
```

This distinction is useful when working with configuration objects, update operations, and APIs where omitting a property has different meaning from explicitly setting it to `undefined`.

# 🗁 Null and Undefined in Function Parameters

Function parameters can explicitly accept `null` or `undefined`.

```typescript
function greet(name: string | null): void {
    if (name === null) {
        console.log("Hello, Guest");
        return;
    }

    console.log(`Hello, ${name}`);
}

greet("John");
greet(null);
```

A parameter can also accept `undefined`.

```typescript
function displayName(name: string | undefined): void {
    console.log(name ?? "Guest");
}

displayName("John");
displayName(undefined);
```

# 🗁 Optional Function Parameters

Optional parameters can be omitted when calling a function.

```typescript
function greet(name?: string): void {
    console.log(`Hello, ${name ?? "Guest"}`);
}

greet("John");
greet();
```

An optional parameter behaves as a parameter whose type includes `undefined`.

```typescript
function greet(name: string | undefined): void {
    console.log(`Hello, ${name ?? "Guest"}`);
}

greet("John");
greet(undefined);

// TypeScript error: argument is required.
// greet();
```

# 🗁 Default Function Parameters

Default parameters provide a fallback value when an argument is omitted or explicitly passed as `undefined`.

```typescript
function greet(name: string = "Guest"): void {
    console.log(`Hello, ${name}`);
}

greet("John");     // Hello, John
greet();           // Hello, Guest
greet(undefined);  // Hello, Guest
```

Passing `null` does not trigger the default parameter.

```typescript
function greet(name: string | null = "Guest"): void {
    console.log(`Hello, ${name}`);
}

greet(null); // Hello, null
```

# 🗁 Nullish Coalescing Operator

The nullish coalescing operator (`??`) returns the right-hand value when the left-hand value is `null` or `undefined`.

```typescript
const username: string | null = null;

const displayName = username ?? "Guest";

console.log(displayName); // Guest
```

It is especially useful for providing default values.

```typescript
const quantity: number | undefined = undefined;

const result = quantity ?? 10;

console.log(result); // 10
```

Unlike the logical OR operator (`||`), nullish coalescing preserves valid falsy values such as `0`, `false`, and an empty string.

```typescript
const score: number = 0;

console.log(score || 100); // 100
console.log(score ?? 100); // 0
```

# 🗁 Optional Chaining Operator

The optional chaining operator (`?.`) safely accesses properties or methods when an intermediate value may be `null` or `undefined`.

```typescript
interface User {
    profile?: {
        name?: string;
    };
}

const user: User = {};

const username = user.profile?.name;

console.log(username); // undefined
```

Without optional chaining, accessing `user.profile.name` directly could produce a runtime error.

Optional chaining can also be used with arrays and function calls.

**Array access:**

```typescript
const users: string[] | undefined = undefined;

console.log(users?.[0]); // undefined
```

**Optional function calls:**

```typescript
interface Logger {
    log?: (message: string) => void;
}

const logger: Logger = {};

logger.log?.("Application started");
```

# 🗁 Type Narrowing for Null and Undefined

Type narrowing allows TypeScript to determine a more specific type based on conditional checks.

**Using strict equality:**

```typescript
function printName(name: string | null): void {
    if (name === null) {
        console.log("Name is missing");
        return;
    }

    console.log(name.toUpperCase());
}
```

**Checking against undefined:**

```typescript
function printAge(age: number | undefined): void {
    if (age === undefined) {
        console.log("Age is not available");
        return;
    }

    console.log(age.toFixed(0));
}
```

**Checking both null and undefined:**

```typescript
function displayValue(value: string | null | undefined): void {
    if (value == null) {
        console.log("No value available");
        return;
    }

    console.log(value.toUpperCase());
}
```

The loose equality check `value == null` matches both `null` and `undefined`.

When this behavior is intentional, it is a concise way to check both nullish values.

# 🗁 Non-Null Assertion Operator

The non-null assertion operator (`!`) tells TypeScript that a value is not `null` or `undefined`.

```typescript
const username: string | null = "John";

console.log(username!.toUpperCase());
```

It removes `null` and `undefined` from the type for that expression.

However, it does not perform runtime validation.

```typescript
const username: string | null = null;

// This compiles but throws a runtime error.
// console.log(username!.toUpperCase());
```

Use non-null assertions only when you can guarantee that the value exists.

Prefer explicit checks whenever possible.

```typescript
const username: string | null = null;

if (username !== null) {
    console.log(username.toUpperCase());
}
```

# 🗁 Null and Undefined with Arrays

Arrays can contain nullable or undefined elements when their element types allow them.

```typescript
const usernames: (string | null)[] = [
    "John",
    null,
    "Jane"
];
```

An array can also contain undefined elements.

```typescript
const values: (number | undefined)[] = [
    10,
    undefined,
    30
];
```

You can filter out nullish values using a type predicate.

```typescript
const values: (number | null | undefined)[] = [
    10,
    null,
    undefined,
    20,
    30
];

const validValues = values.filter(
    (value): value is number => value != null
);

console.log(validValues); // [10, 20, 30]
```

The type predicate `value is number` tells TypeScript that the resulting array contains only numbers.

# 🗁 Null and Undefined with API Responses

External APIs may return missing or null values.

Consider this response:

```typescript
interface ApiUser {
    id: number;
    name: string;
    email: string | null;
    phone?: string;
}

const user: ApiUser = {
    id: 1,
    name: "John",
    email: null
};
```

The `email` property is required but may contain `null`.

The `phone` property is optional and may be absent.

These two situations represent different data contracts.

You can handle both safely:

```typescript
const email = user.email ?? "No email available";
const phone = user.phone ?? "No phone available";

console.log(email);
console.log(phone);
```

For external JSON data, TypeScript interfaces do not perform runtime validation. Validate incoming data before treating it as a trusted `ApiUser`.

# 🗁 Null and Undefined in JSON

JSON supports `null`, but it does not support `undefined` as a JSON value.

When serializing objects:

```typescript
const user = {
    name: "John",
    email: null,
    phone: undefined
};

console.log(JSON.stringify(user));
```

Output:

```json
{
    "name": "John",
    "email": null
}
```

The property with the `undefined` value is omitted.

When serializing arrays, `undefined` elements are converted to `null`.

```typescript
const values = [10, undefined, 30];

console.log(JSON.stringify(values));

// [10,null,30]
```

This distinction is important when sending data through APIs.

# 🗁 Best Practices

* Enable `strictNullChecks` to detect potentially missing values.
* Use `null` when representing an intentional absence of a value.
* Understand that optional properties may be `undefined`.
* Use `??` instead of `||` when only nullish values should trigger a fallback.
* Use optional chaining when accessing potentially missing nested properties.
* Prefer explicit null and undefined checks over unnecessary non-null assertions.
* Use `exactOptionalPropertyTypes` when omitted properties and explicit `undefined` must be distinguished.
* Validate external API responses at runtime.
* Use clear types such as `string | null` when a value can intentionally be empty.
* Avoid assigning `null` or `undefined` to variables that do not need to represent missing values.

# 🗁 Summary

TypeScript uses `null` and `undefined` to represent different forms of missing values.

Important concepts include:

* The meaning of `null` and `undefined`.
* Strict null checking.
* Optional object properties.
* Optional and default function parameters.
* Nullish coalescing (`??`).
* Optional chaining (`?.`).
* Type narrowing.
* Non-null assertions (`!`).
* Nullable and undefined array elements.
* Handling missing API data.
* JSON serialization behavior.

Understanding these types helps developers build more predictable applications and avoid common runtime errors caused by accessing missing values.
