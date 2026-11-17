# Type System in TypeScript

A type system is a set of rules that determines how values, variables, functions, and expressions can be used in a programming language.

It defines:

* What kinds of values can be stored in variables.
* Which operations are valid for different values.
* How functions receive and return values.
* How objects and their properties are structured.
* How different types relate to one another.
* How type errors are detected.

TypeScript uses a static type system built on top of JavaScript.

Example:

```typescript
let username: string = "Alice";

let age: number = 25;

let isActive: boolean = true;
```

TypeScript checks these types before the code executes.

# 🗁 Why Type Systems Matter

A type system helps developers:

* Detect programming mistakes earlier.
* Define clear contracts between components.
* Improve code readability.
* Make refactoring safer.
* Improve editor autocomplete.
* Build maintainable applications.
* Express complex data structures.
* Reduce unexpected runtime errors.

Without a type system, developers must rely more heavily on runtime behavior and testing to discover certain mistakes.

# 🗁 Static Typing vs Dynamic Typing

Programming languages use different approaches to type checking.

**Static typing**

Types are checked before program execution, usually during compilation or static analysis.

Examples:

* TypeScript
* Java
* C#
* Rust

**Dynamic typing**

Types are associated with runtime values, and many type-related errors are detected during execution.

Examples:

* JavaScript
* Python
* Ruby
* PHP

JavaScript example:

```javascript
let value = 100;

value = "Hello";

console.log(value);
```

JavaScript allows the variable to reference values of different types.

TypeScript example:

```typescript
let value: number = 100;

value = "Hello"; // Type error
```

TypeScript reports that a string cannot be assigned to a number variable.

# 🗁 TypeScript Is Structurally Typed

TypeScript primarily uses structural typing.

This means compatibility is determined by the structure of types rather than their declared names.

Example:

```typescript
interface User {
  name: string;
  age: number;
}

interface Employee {
  name: string;
  age: number;
}

const user: User = {
  name: "Alice",
  age: 25,
};

const employee: Employee = user;
```

This assignment is valid because both interfaces have compatible structures.

TypeScript does not require the two interfaces to have the same name or inheritance relationship.

# 🗁 Type Annotations

Type annotations explicitly describe the expected type of a value.

Syntax:

```typescript
let variableName: Type;
```

Examples:

```typescript
let username: string = "Alice";

let age: number = 25;

let isActive: boolean = true;

let scores: number[] = [80, 90, 95];
```

Function annotations:

```typescript
function add(a: number, b: number): number {
  return a + b;
}
```

Type annotations improve clarity and allow TypeScript to check whether values are used correctly.

# 🗁 Type Inference

TypeScript can automatically determine types from values and expressions.

This is called type inference.

Example:

```typescript
let username = "Alice";

let age = 25;

let isActive = true;
```

TypeScript infers:

```typescript
let username: string;

let age: number;

let isActive: boolean;
```

You do not always need to write explicit annotations.

Type inference reduces unnecessary repetition while preserving static type checking.

# 🗁 Types vs Values

One of the most important concepts in TypeScript is the distinction between types and runtime values.

**Values**

Values exist at runtime.

```typescript
const username = "Alice";

function greet() {
  console.log("Hello");
}
```

**Types**

Types exist for static analysis.

```typescript
type User = {
  name: string;
  age: number;
};

interface Product {
  id: number;
  title: string;
}
```

TypeScript removes most type-only constructs during compilation.

For example:

```typescript
type User = {
  name: string;
};

const user: User = {
  name: "Alice",
};
```

JavaScript output:

```javascript
const user = {
  name: "Alice",
};
```

The `User` type does not exist at runtime.

# 🗁 Primitive Types

TypeScript supports JavaScript's primitive value types.

Common primitive types include:

| Type | Description | Example |
| --- | --- | --- |
| `string` | Text | `"Hello"` |
| `number` | Numeric values | `100` |
| `boolean` | True or false | `true` |
| `bigint` | Large integers | `100n` |
| `symbol` | Unique identifiers | `Symbol("id")` |
| `null` | Intentional absence of a value | `null` |
| `undefined` | Missing or uninitialized value | `undefined` |

Example:

```typescript
let name: string = "Alice";

let price: number = 99.99;

let enabled: boolean = true;

let largeNumber: bigint = 100n;

let identifier: symbol = Symbol("id");

let emptyValue: null = null;

let missingValue: undefined = undefined;
```

# 🗁 Special Types

TypeScript also provides special types for representing broader or more restricted sets of values.

**any**

Allows values of any type and disables many type checks.

```typescript
let value: any = 100;

value = "Hello";

value = true;
```

**unknown**

Represents a value whose type is not yet known.

```typescript
let value: unknown = "Hello";

if (typeof value === "string") {
  console.log(value.toUpperCase());
}
```

**never**

Represents values that cannot occur.

```typescript
function throwError(message: string): never {
  throw new Error(message);
}
```

**void**

Commonly represents a function that does not return a meaningful value.

```typescript
function logMessage(): void {
  console.log("Hello");
}
```

# 🗁 Type Aliases

A type alias gives a name to a type.

Use the `type` keyword.

```typescript
type User = {
  id: number;
  name: string;
  email: string;
};

const user: User = {
  id: 1,
  name: "Alice",
  email: "alice@example.com",
};
```

Type aliases can represent primitives, unions, intersections, tuples, objects, and more.

Example:

```typescript
type UserId = number;

type Status = "pending" | "success" | "failed";

type Point = [number, number];
```

# 🗁 Interfaces

Interfaces describe the structure of objects and contracts that classes or other objects can implement.

```typescript
interface User {
  id: number;
  name: string;
  email?: string;
}

const user: User = {
  id: 1,
  name: "Alice",
};
```

Interfaces can also be extended:

```typescript
interface Person {
  name: string;
}

interface Employee extends Person {
  employeeId: number;
  department: string;
}
```

# 🗁 Union Types

A union type allows a value to belong to one of several possible types.

Use the `|` operator.

```typescript
let value: string | number;

value = "Hello";

value = 100;
```

Union types are useful when a variable or parameter can legitimately represent different kinds of values.

Example:

```typescript
function printId(id: string | number): void {
  console.log(id);
}
```

# 🗁 Intersection Types

An intersection type combines multiple types into one.

Use the `&` operator.

```typescript
type Person = {
  name: string;
};

type Employee = {
  employeeId: number;
};

type StaffMember = Person & Employee;

const employee: StaffMember = {
  name: "Alice",
  employeeId: 101,
};
```

The resulting type must satisfy both structures.

# 🗁 Literal Types

Literal types restrict a value to specific values.

```typescript
type Direction = "up" | "down" | "left" | "right";

let direction: Direction = "up";

direction = "left";
```

Numeric literal types are also supported:

```typescript
type HttpStatus = 200 | 201 | 400 | 404 | 500;

let status: HttpStatus = 200;
```

Literal types are useful for representing finite sets of valid values.

# 🗁 Type Compatibility

Type compatibility determines whether a value of one type can be assigned to another type.

Example:

```typescript
let first: number = 100;

let second: number = first;
```

This assignment is valid because both values have compatible types.

An incompatible assignment produces an error:

```typescript
let age: number = 25;

let username: string = age; // Type error
```

TypeScript checks compatibility using its type system rules, primarily structural compatibility.

# 🗁 Type Narrowing

Type narrowing reduces a broad type to a more specific type based on runtime checks.

Example:

```typescript
function printValue(value: string | number): void {
  if (typeof value === "string") {
    console.log(value.toUpperCase());
  } else {
    console.log(value.toFixed(2));
  }
}
```

Inside the first branch, TypeScript knows that `value` is a string.

Inside the second branch, TypeScript knows that it is a number.

Common narrowing techniques include:

* `typeof`
* `instanceof`
* `in`
* Equality checks
* Truthiness checks
* Discriminated unions
* User-defined type predicates

# 🗁 Type Assertions

Type assertions tell TypeScript to treat a value as a particular type.

They do not perform runtime conversion or validation.

Example:

```typescript
const input = document.querySelector("#username") as HTMLInputElement;

console.log(input.value);
```

The assertion informs TypeScript that the selected element should be treated as an `HTMLInputElement`.

A safer alternative when the element may not exist:

```typescript
const input = document.querySelector("#username");

if (input instanceof HTMLInputElement) {
  console.log(input.value);
}
```

# 🗁 Generic Types

Generics allow types to work with different data types while preserving type information.

Example:

```typescript
function identity<T>(value: T): T {
  return value;
}

const first = identity<string>("Hello");

const second = identity<number>(100);
```

TypeScript can also infer the generic type:

```typescript
const first = identity("Hello");

const second = identity(100);
```

Generics are useful for reusable functions, interfaces, classes, and utility types.

# 🗁 Type-Level Programming

TypeScript supports advanced operations that transform and combine types during static analysis.

Examples include:

* Conditional types.
* Mapped types.
* Indexed access types.
* Template literal types.
* `keyof`.
* `typeof`.
* `infer`.
* Recursive types.

Example:

```typescript
type User = {
  id: number;
  name: string;
  email: string;
};

type UserKeys = keyof User;
```

The resulting type is:

```typescript
type UserKeys = "id" | "name" | "email";
```

Type-level programming makes it possible to build sophisticated reusable type definitions.

# 🗁 Type System Limitations

TypeScript improves compile-time correctness, but it does not guarantee that a program is free from runtime errors.

Important limitations include:

**Types are erased**

Most TypeScript type information is removed when generating JavaScript.

**No automatic runtime validation**

Type annotations do not validate API responses, database records, or user input.

**Type assertions can be incorrect**

TypeScript trusts assertions even when the actual runtime value does not match the asserted type.

**any weakens type safety**

Using `any` can bypass many useful compiler checks.

**External JavaScript can violate declarations**

Incorrect third-party declarations or JavaScript implementations can produce runtime failures.

Example:

```typescript
interface User {
  id: number;
  name: string;
}

const user = JSON.parse(
  '{"id":"invalid","name":100}'
) as User;
```

This compiles, but the actual values do not match the `User` interface.

Runtime validation is necessary when data comes from untrusted sources.

# 🗁 Type System Best Practices

* Prefer explicit types for public APIs.
* Use type inference for straightforward local variables.
* Enable strict compiler options.
* Avoid unnecessary `any`.
* Prefer `unknown` when the type is not known.
* Use union types to represent multiple valid possibilities.
* Use interfaces and type aliases to describe domain models.
* Apply generics when building reusable abstractions.
* Narrow types before performing type-specific operations.
* Validate external data at runtime.
* Keep type definitions aligned with actual application behavior.

# 🗁 Key Takeaways

* A type system defines rules for how values can be used.
* TypeScript provides static type checking for JavaScript.
* TypeScript primarily uses structural typing.
* Types can be explicitly annotated or inferred.
* Types and runtime values are fundamentally different.
* TypeScript supports primitive, special, union, intersection, literal, and generic types.
* Type narrowing helps safely work with multiple possible types.
* Type assertions do not perform runtime validation.
* Advanced TypeScript supports type-level programming.
* Static type safety improves correctness but does not replace runtime validation.
