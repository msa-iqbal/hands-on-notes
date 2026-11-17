# TypeScript Type Annotations

Type annotations are explicit declarations that tell TypeScript what type of value a variable, parameter, property, or function should accept or return.

They are one of the fundamental features of TypeScript's static type system. Type annotations help developers catch type-related errors during development, improve code readability, and provide better editor support.

Type annotations use a colon (`:`) followed by a type.

# 🗁 Basic Type Annotation Syntax

The general syntax is:

```typescript
let variableName: Type = value;
```

Example:

```typescript
let username: string = "John";
let age: number = 25;
let isActive: boolean = true;
```

In this example:

* `username` can store string values.
* `age` can store number values.
* `isActive` can store Boolean values.

Assigning an incompatible value produces a TypeScript error.

```typescript
let age: number = 25;

// TypeScript error
// age = "Twenty-five";
```

# 🗁 Variable Type Annotations

Type annotations can be applied to variables declared with `let`, `const`, and `var`.

```typescript
let firstName: string = "John";
let age: number = 30;
let isLoggedIn: boolean = false;

const country: string = "Bangladesh";
const pi: number = 3.14159;
```

A variable's declared type determines which values can be assigned to it.

```typescript
let score: number = 100;

score = 150; // Valid

// TypeScript error
// score = "150";
```

# 🗁 Type Annotations with Arrays

Array types can be declared using two common syntaxes.

**Array type syntax:**

```typescript
let numbers: number[] = [10, 20, 30];

let names: string[] = ["John", "Jane", "David"];
```

**Generic array syntax:**

```typescript
let numbers: Array<number> = [10, 20, 30];

let names: Array<string> = ["John", "Jane", "David"];
```

Both syntaxes describe arrays containing elements of a specific type.

# 🗁 Type Annotations with Functions

Function parameters can have explicit type annotations.

```typescript
function add(a: number, b: number): number {
    return a + b;
}

console.log(add(10, 20)); // 30
```

In this example:

* `a: number` specifies the first parameter's type.
* `b: number` specifies the second parameter's type.
* `: number` after the parameter list specifies the return type.

Function annotations help prevent incorrect arguments and return values.

```typescript
function greet(name: string): string {
    return `Hello, ${name}`;
}

greet("John");

// TypeScript error
// greet(100);
```

# 🗁 Type Annotations with Objects

Object properties can have explicit types.

```typescript
let user: {
    id: number;
    name: string;
    email: string;
    isActive: boolean;
} = {
    id: 1,
    name: "John",
    email: "john@example.com",
    isActive: true
};
```

Every required property must exist and have a compatible value.

For reusable object structures, interfaces and type aliases are generally preferable.

Using an interface:

```typescript
interface User {
    id: number;
    name: string;
    email: string;
}

const user: User = {
    id: 1,
    name: "John",
    email: "john@example.com"
};
```

Using a type alias:

```typescript
type Product = {
    id: number;
    name: string;
    price: number;
};

const product: Product = {
    id: 101,
    name: "Keyboard",
    price: 50
};
```

# 🗁 Type Annotations with Union Types

Union types allow a variable to accept values from multiple specified types.

The union operator is represented by the pipe symbol (`|`).

```typescript
let id: number | string;

id = 101;
id = "USER-101";

// TypeScript error
// id = true;
```

Union types are useful when a value can legitimately have different representations.

Example:

```typescript
function printId(id: number | string): void {
    console.log(id);
}

printId(101);
printId("USER-101");
```

# 🗁 Type Annotations with Literal Types

Literal types restrict a variable to specific values.

```typescript
let status: "pending" | "approved" | "rejected";

status = "pending";
status = "approved";

// TypeScript error
// status = "completed";
```

Literal types are useful for representing predefined states, modes, and options.

# 🗁 Type Annotations with Tuples

Tuples define arrays with a fixed structure and known element types.

```typescript
let user: [number, string, boolean];

user = [1, "John", true];

// TypeScript error
// user = ["John", 1, true];
```

Each position in the tuple has a specific expected type.

```typescript
type Coordinate = [number, number];

const point: Coordinate = [10, 20];
```

# 🗁 Type Annotations with Optional Properties

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

The `age` property is optional, but when provided, it must contain a compatible value.

# 🗁 Type Annotations with Readonly Properties

The `readonly` modifier prevents a property from being reassigned after initialization.

```typescript
interface User {
    readonly id: number;
    name: string;
}

const user: User = {
    id: 1,
    name: "John"
};

user.name = "David"; // Valid

// TypeScript error
// user.id = 2;
```

`readonly` provides compile-time protection against reassignment. It does not automatically make nested objects immutable at runtime.

# 🗁 Type Annotations with Generics

Generics allow types to be represented as parameters, making functions and data structures reusable across different types.

```typescript
function identity<T>(value: T): T {
    return value;
}

const numberValue = identity<number>(100);
const stringValue = identity<string>("Hello");
```

TypeScript can often infer the generic type from the supplied argument.

```typescript
const numberValue = identity(100);
const stringValue = identity("Hello");
```

Generics are particularly useful when building reusable utilities, collections, and APIs.

# 🗁 Type Annotations vs Type Inference

TypeScript can determine many types automatically from assigned values. This is called type inference.

Explicit annotation:

```typescript
let age: number = 25;
```

Type inference:

```typescript
let age = 25;
```

In the second example, TypeScript infers that `age` has the type `number`.

Both examples provide type safety.

**When to use explicit annotations:**

* When a variable is declared without an initial value.
* When a function parameter requires a specific type.
* When a function's return type should be explicit.
* When an object follows a reusable interface or type alias.
* When a public API needs a clearly defined contract.

**When type inference is sufficient:**

* When a variable is initialized with an obvious value.
* When an expression clearly determines the resulting type.
* When explicit annotations would unnecessarily repeat information.

Example:

```typescript
const username = "John"; // Inferred as string
const age = 25;          // Inferred as number
const isActive = true;   // Inferred as boolean
```

# 🗁 Type Annotations and the Any Type

The `any` type disables most type checking for a value.

```typescript
let value: any = "Hello";

value = 100;
value = true;

value.someUndefinedMethod();
```

TypeScript allows these operations because `any` bypasses normal type checking.

Use `any` sparingly. Prefer `unknown` when a value's type is not known and must be checked before use.

```typescript
let value: unknown = "Hello";

if (typeof value === "string") {
    console.log(value.toUpperCase());
}
```

# 🗁 Type Annotations with Null and Undefined

TypeScript allows explicit declarations for `null` and `undefined`.

```typescript
let emptyValue: null = null;
let missingValue: undefined = undefined;
```

With `strictNullChecks` enabled, these types are handled separately from ordinary types.

Union types can be used when a value may contain a normal value or a nullish value.

```typescript
let username: string | null = "John";

username = null;
username = "David";
```

This makes potentially missing values explicit in the type system.

# 🗁 Type Annotations with Type Assertions

Type assertions tell TypeScript to treat a value as a particular type.

They use either the `as` syntax or angle-bracket syntax.

**Using `as`:**

```typescript
const value: unknown = "Hello";

const message = value as string;

console.log(message.toUpperCase());
```

**Using angle brackets:**

```typescript
const value: unknown = "Hello";

const message = <string>value;

console.log(message.toUpperCase());
```

The `as` syntax is generally preferred because angle-bracket assertions conflict with JSX syntax in `.tsx` files.

Type assertions do not perform runtime conversion or validation. They only affect how TypeScript understands the expression.

# 🗁 Type Annotations in Class Properties

Class properties can have explicit type annotations.

```typescript
class User {
    id: number;
    name: string;
    isActive: boolean;

    constructor(id: number, name: string) {
        this.id = id;
        this.name = name;
        this.isActive = true;
    }
}

const user = new User(1, "John");
```

Type annotations help ensure that class properties and constructor arguments follow the expected types.

# 🗁 Type Annotations with Arrow Functions

Arrow function parameters and return values can also be annotated.

```typescript
const add = (a: number, b: number): number => {
    return a + b;
};
```

For concise expressions:

```typescript
const multiply = (a: number, b: number): number => a * b;
```

A function variable can also be assigned a function type.

```typescript
let calculate: (a: number, b: number) => number;

calculate = (a, b) => a + b;
```

# 🗁 Best Practices

* Use explicit types when they improve clarity or define important contracts.
* Let TypeScript infer obvious variable types.
* Avoid unnecessary repetition of inferred types.
* Prefer `unknown` over `any` when handling values of uncertain types.
* Use interfaces or type aliases for reusable object structures.
* Define function parameter types clearly.
* Use explicit return types for important public APIs and complex functions.
* Use union types when a value can legitimately represent multiple types.
* Use optional properties only when a property is genuinely allowed to be absent.
* Avoid excessive type assertions because they can hide genuine type errors.
* Enable `strict` mode in `tsconfig.json` for stronger type checking.

# 🗁 Summary

Type annotations define the expected types of values in TypeScript.

They can be applied to:

* Variables and constants.
* Function parameters and return values.
* Arrays and tuples.
* Object properties.
* Interfaces and type aliases.
* Class properties and methods.
* Generic functions and data structures.

Type annotations provide explicit type contracts, while type inference allows TypeScript to determine types automatically.

Using both appropriately results in readable, maintainable, and type-safe applications.
