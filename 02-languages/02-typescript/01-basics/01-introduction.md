# TypeScript Introduction

**TypeScript** is a strongly typed programming language developed and maintained by Microsoft. It extends JavaScript with additional features, especially a static type system that helps developers build reliable, maintainable, and scalable applications.

TypeScript is designed to solve many challenges associated with developing large JavaScript applications.

It provides:

* Static type checking.
* Type inference.
* Interfaces and type aliases.
* Generics.
* Advanced type manipulation.
* Object-oriented programming features.
* Improved IDE support.
* Better code navigation and refactoring.

TypeScript does not introduce a separate runtime. It is compiled or transformed into JavaScript, which executes in standard JavaScript environments.

# 🗁 What Is TypeScript?

TypeScript is a superset of JavaScript that adds a static type system and other development-time language features.

JavaScript:

```javascript
function calculateTotal(price, quantity) {
    return price * quantity;
}

console.log(calculateTotal(100, 5));
```

TypeScript:

```typescript
function calculateTotal(
    price: number,
    quantity: number
): number {
    return price * quantity;
}

console.log(calculateTotal(100, 5));
```

In the TypeScript version:

* `price: number` specifies that `price` must be a number.
* `quantity: number` specifies that `quantity` must be a number.
* `: number` after the parameter list specifies the return type.

If an incompatible value is supplied, TypeScript can report a type error during development or compilation.

```typescript
calculateTotal("100", 5);
```

TypeScript reports an error because the first argument is expected to be a number.

# 🗁 History of TypeScript

TypeScript was introduced by Microsoft in October 2012.

It was created to improve the development experience of large-scale JavaScript applications.

Important milestones:

| Version | Release Year | Notable Development |
| --- | ---: | --- |
| TypeScript 0.8 | 2012 | Initial public release |
| TypeScript 1.0 | 2014 | First stable release |
| TypeScript 2.0 | 2016 | Major improvements to the type system |
| TypeScript 3.0 | 2018 | Tuple and project-reference improvements |
| TypeScript 4.0 | 2020 | Variadic tuple types and other improvements |
| TypeScript 4.9 | 2022 | Introduced the `satisfies` operator |
| TypeScript 5.0 | 2023 | Major decorator and type-system improvements |
| TypeScript 5.x | 2023–2025 | Continued compiler, language, and tooling improvements |

TypeScript continues to evolve with new language features, compiler improvements, and ecosystem support.

# 🗁 TypeScript vs JavaScript

TypeScript builds on JavaScript rather than replacing it.

| Feature | JavaScript | TypeScript |
| --- | --- | --- |
| Language category | Dynamically typed | Statically type-checked |
| Type annotations | Not supported | Supported |
| Type inference | Runtime behavior determines types | Compiler infers types |
| Interfaces | Not available as a type-system feature | Supported |
| Generics | Not available as a type-system feature | Supported |
| Compile-time type checking | Not built into the language | Supported |
| Runtime execution | Directly executed by JavaScript engines | Executes after transformation into JavaScript |
| Editor support | Available | Enhanced through static type information |
| Learning curve | Generally easier initially | Requires understanding of types |
| Large application maintenance | Requires additional discipline and tooling | Provides additional structural guarantees |

Example:

JavaScript:

```javascript
let age = 25;

age = "twenty-five";
```

This assignment is valid JavaScript.

TypeScript:

```typescript
let age: number = 25;

age = "twenty-five";
```

TypeScript reports a type error because a string cannot be assigned to a variable declared as `number`.

# 🗁 Why TypeScript Was Created

JavaScript is flexible and productive, but large applications introduce challenges.

Common challenges include:

* Unexpected data types.
* Incorrect function arguments.
* Missing object properties.
* Inconsistent API response structures.
* Difficult refactoring.
* Limited documentation of data contracts.
* Errors that appear only when specific code executes.

TypeScript addresses many of these problems through static analysis.

Example without TypeScript:

```javascript
function getUser(user) {
    return user.profile.name;
}

console.log(
    getUser({
        username: "Alice"
    })
);
```

The code fails at runtime because `profile` does not exist.

With TypeScript:

```typescript
interface User {
    profile: {
        name: string;
    };
}

function getUser(user: User): string {
    return user.profile.name;
}
```

The expected object structure is explicitly defined.

An incorrectly shaped object can be detected before execution.

# 🗁 Static Typing

Static typing means checking type compatibility without executing the program.

TypeScript analyzes types during development and compilation.

Example:

```typescript
let username: string = "Alice";

let age: number = 25;

let isActive: boolean = true;
```

Incorrect assignments produce type errors:

```typescript
username = 100;

age = "25";

isActive = "yes";
```

Static type checking helps detect these mistakes before runtime.

# 🗁 Dynamic Typing

JavaScript uses dynamic typing.

A variable can hold values of different types during execution.

Example:

```javascript
let value = 100;

value = "Hello";

value = true;
```

This is valid JavaScript.

TypeScript can also represent values whose types change, but it tracks the possible types through its type system.

Example:

```typescript
let value: string | number = 100;

value = "Hello";

value = 200;
```

The union type `string | number` explicitly permits both strings and numbers.

# 🗁 Type Inference

Type inference allows TypeScript to determine types automatically from expressions and assigned values.

Example:

```typescript
let username = "Alice";

let age = 25;

let isActive = true;
```

TypeScript infers:

| Variable | Inferred Type |
| --- | --- |
| `username` | `string` |
| `age` | `number` |
| `isActive` | `boolean` |

Explicit annotations are not always necessary.

However, annotations are useful when defining public APIs, function contracts, complex data structures, and values whose intended types are not obvious.

# 🗁 Type Annotations

Type annotations explicitly describe the expected type of a variable, parameter, or return value.

Syntax:

```typescript
let variableName: type = value;
```

Example:

```typescript
let username: string = "Alice";

let age: number = 25;

let isAdmin: boolean = false;
```

Function annotations:

```typescript
function greet(name: string): string {
    return `Hello, ${name}`;
}
```

Type annotations improve readability and help TypeScript detect incompatible operations.

# 🗁 TypeScript Is Not a Runtime Validation System

TypeScript checks types during development and compilation, but its types are generally erased from emitted JavaScript.

Example:

```typescript
function greet(name: string): string {
    return `Hello, ${name}`;
}
```

Emitted JavaScript:

```javascript
function greet(name) {
    return `Hello, ${name}`;
}
```

The JavaScript runtime does not know that `name` was declared as a string.

External data must be validated at runtime.

Example:

```typescript
function processUser(data: unknown): string {
    if (
        typeof data === "object" &&
        data !== null &&
        "name" in data &&
        typeof data.name === "string"
    ) {
        return data.name;
    }

    throw new Error("Invalid user data");
}
```

This distinction is important when working with API responses, user input, files, and databases.

# 🗁 TypeScript Compilation

TypeScript source files commonly use the `.ts` extension.

The TypeScript compiler, `tsc`, checks and transforms TypeScript code.

Example:

```typescript
const message: string = "Hello TypeScript";

console.log(message);
```

Save the code as:

```text
index.ts
```

Compile it:

```bash
tsc index.ts
```

The compiler generates:

```text
index.js
```

Run the JavaScript:

```bash
node index.js
```

Output:

```text
Hello TypeScript
```

# 🗁 TypeScript File Extensions

TypeScript supports several file extensions.

| Extension | Purpose |
| --- | --- |
| `.ts` | Standard TypeScript source file |
| `.tsx` | TypeScript source file containing JSX |
| `.d.ts` | Type declaration file |
| `.mts` | TypeScript source treated as an ECMAScript module |
| `.cts` | TypeScript source treated as a CommonJS module |
| `.d.mts` | Declaration file for an ECMAScript module |
| `.d.cts` | Declaration file for a CommonJS module |

Examples:

```text
src/
├── index.ts
├── App.tsx
├── types.d.ts
├── module.mts
└── legacy.cts
```

# 🗁 Where TypeScript Is Used

TypeScript can be used in different application environments.

**Frontend Development**

TypeScript is widely used with:

* React.
* Angular.
* Vue.
* Svelte.
* Next.js.
* Other JavaScript-based frontend frameworks.

**Backend Development**

TypeScript can be used with:

* Node.js.
* Express.
* NestJS.
* Fastify.
* Other Node.js frameworks.

**Full-Stack Development**

TypeScript can provide shared type definitions across frontend and backend applications.

**Library Development**

Libraries can publish TypeScript declarations to provide type information to developers using their APIs.

**Enterprise Applications**

TypeScript helps maintain consistent data structures and contracts across large codebases and development teams.

# 🗁 TypeScript Ecosystem

TypeScript works with a large JavaScript development ecosystem.

| Tool | Purpose |
| --- | --- |
| TypeScript Compiler (`tsc`) | Type checking and JavaScript generation |
| Node.js | JavaScript runtime |
| npm | Package management |
| ts-node | TypeScript execution in Node.js environments |
| tsx | TypeScript and JSX execution |
| ESLint | Code quality and linting |
| Prettier | Code formatting |
| Vite | Frontend development and build tooling |
| webpack | Module bundling |
| Jest | Testing |
| Vitest | Testing |

Some tools transform TypeScript without performing complete TypeScript type checking. A separate `tsc --noEmit` check may therefore be necessary.

# 🗁 Advantages of TypeScript

* Detects many type-related errors before runtime.
* Improves code readability and maintainability.
* Provides better IDE autocomplete.
* Supports safer refactoring.
* Makes data structures explicit.
* Improves API contract definitions.
* Supports reusable generic components.
* Works with the JavaScript ecosystem.
* Helps teams maintain large applications.
* Supports advanced type-level programming.

# 🗁 Disadvantages of TypeScript

* Requires learning an additional type system.
* Introduces compilation and configuration requirements.
* Can require additional effort for complex type definitions.
* Type declarations may be needed for some JavaScript libraries.
* Incorrect use of `any` can weaken type safety.
* TypeScript cannot guarantee runtime correctness.
* Complex types can increase compiler workload.
* Some JavaScript tools require additional TypeScript configuration.

# 🗁 When Should You Use TypeScript?

TypeScript is particularly useful when:

* Building large applications.
* Working with multiple developers.
* Maintaining complex data models.
* Developing reusable libraries.
* Defining API contracts.
* Refactoring existing codebases.
* Building long-term software projects.

JavaScript may be sufficient for small scripts, simple prototypes, and projects where introducing a type system would add unnecessary overhead.

# 🗁 Best Practices

* Enable strict type checking.
* Prefer meaningful type annotations for public interfaces.
* Allow type inference when the type is obvious.
* Avoid unnecessary use of `any`.
* Use `unknown` for values with an unknown type.
* Define reusable interfaces and type aliases.
* Validate external data at runtime.
* Keep TypeScript and dependency versions consistent.
* Separate application logic from type definitions when appropriate.
* Avoid overly complicated type-level abstractions.
* Use automated tests alongside static type checking.

# 🗁 Key Takeaways

* TypeScript is a statically typed superset of JavaScript.
* It adds a powerful type system to JavaScript development.
* TypeScript supports static checking and type inference.
* It helps detect many errors before runtime.
* TypeScript code is transformed into JavaScript.
* Its types do not automatically exist at runtime.
* It supports frontend, backend, and full-stack development.
* It is particularly valuable for large and maintainable applications.

# 🗁 Related Topics

* JavaScript Fundamentals
* TypeScript Installation
* TypeScript Compiler
* Static Typing
* Dynamic Typing
* Type Inference
* Type Annotations
* Primitive Types
* Interfaces
* Type Aliases
* Generics
* TypeScript Configuration
* Node.js
* Software Architecture
