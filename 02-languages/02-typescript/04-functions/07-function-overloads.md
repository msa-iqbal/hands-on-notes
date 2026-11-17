# Function Overloads

Function overloading allows you to define multiple function signatures for a single function implementation.

In TypeScript, function overloads let a function accept different combinations of parameters and return different types depending on the arguments provided.

Function overloading is useful when:

* A function supports multiple input types.
* Different argument combinations produce different return types.
* You want to provide a flexible but type-safe API.
* A function needs to support multiple calling patterns.

# 🗁 Basic Function Overloading

TypeScript function overloading consists of two main parts:

* **Overload signatures:** Define the supported ways to call the function.
* **Implementation signature:** Contains the actual function implementation.

Example:

```typescript
function add(a: number, b: number): number;
function add(a: string, b: string): string;

function add(a: number | string, b: number | string): number | string {
    if (typeof a === "number" && typeof b === "number") {
        return a + b;
    }

    if (typeof a === "string" && typeof b === "string") {
        return a + b;
    }

    throw new Error("Arguments must have matching types");
}

console.log(add(10, 20));       // 30
console.log(add("Hello, ", "World")); // Hello, World
```

Here:

* The first overload accepts two numbers and returns a number.
* The second overload accepts two strings and returns a string.
* The implementation handles both supported cases.

# 🗁 Overload Signatures

An overload signature describes a valid function call without providing an implementation body.

```typescript
function greet(name: string): string;
function greet(age: number): string;
```

These signatures tell TypeScript that `greet()` supports either a string or a number argument.

You must provide an implementation signature after the overload declarations.

```typescript
function greet(name: string): string;
function greet(age: number): string;

function greet(value: string | number): string {
    if (typeof value === "string") {
        return `Hello, ${value}!`;
    }

    return `Your age is ${value}.`;
}
```

# 🗁 Implementation Signature

The implementation signature defines the actual function body.

```typescript
function process(value: string): string;
function process(value: number): number;

function process(value: string | number): string | number {
    if (typeof value === "string") {
        return value.toUpperCase();
    }

    return value * 2;
}
```

The implementation signature must be compatible with all overload signatures.

An important distinction is that callers cannot use the implementation signature directly. They can only call the function using one of its declared overload signatures.

For example:

```typescript
function process(value: string): string;
function process(value: number): number;

function process(value: string | number): string | number {
    return typeof value === "string"
        ? value.toUpperCase()
        : value * 2;
}

process("hello"); // Valid
process(10);      // Valid

// Error: No overload matches this call.
process(Math.random() > 0.5 ? "hello" : 10);
```

Although the implementation accepts `string | number`, no overload explicitly accepts that union type.

Add a union overload if callers should be allowed to pass either type through a union:

```typescript
function process(value: string): string;
function process(value: number): number;
function process(value: string | number): string | number;

function process(value: string | number): string | number {
    return typeof value === "string"
        ? value.toUpperCase()
        : value * 2;
}
```

# 🗁 Multiple Parameter Combinations

A function can support different parameter combinations through overloads.

```typescript
function createUser(name: string): string;
function createUser(name: string, age: number): string;
function createUser(
    name: string,
    age?: number
): string {
    if (age !== undefined) {
        return `${name} is ${age} years old.`;
    }

    return `User: ${name}`;
}

console.log(createUser("Alice"));
console.log(createUser("Bob", 25));
```

The overloads explicitly describe the supported calling patterns.

# 🗁 Different Return Types

Function overloads are especially useful when the return type depends on the argument type.

```typescript
function getValue(value: string): string[];
function getValue(value: number): number[];

function getValue(value: string | number): string[] | number[] {
    if (typeof value === "string") {
        return value.split("");
    }

    return Array.from({ length: value }, (_, index) => index + 1);
}

const letters = getValue("hello"); // string[]
const numbers = getValue(5);       // number[]
```

TypeScript determines the return type from the matching overload signature.

# 🗁 Overloading with Object Parameters

Overloads can describe different object input structures.

```typescript
type User = {
    id: number;
    name: string;
};

type Product = {
    id: number;
    title: string;
};

function display(value: User): string;
function display(value: Product): string;

function display(value: User | Product): string {
    if ("name" in value) {
        return `User: ${value.name}`;
    }

    return `Product: ${value.title}`;
}

console.log(display({
    id: 1,
    name: "Alice"
}));

console.log(display({
    id: 2,
    title: "Laptop"
}));
```

The `in` operator helps distinguish between the two object structures.

# 🗁 Overloading with Optional Parameters

Overloads can describe different combinations of optional arguments.

```typescript
function formatDate(date: Date): string;
function formatDate(date: Date, includeTime: true): string;
function formatDate(date: Date, includeTime: false): string;

function formatDate(
    date: Date,
    includeTime?: boolean
): string {
    if (includeTime) {
        return date.toLocaleString();
    }

    return date.toLocaleDateString();
}

const date = new Date();

console.log(formatDate(date));
console.log(formatDate(date, true));
console.log(formatDate(date, false));
```

Each overload explicitly defines a supported call.

# 🗁 Overloading with Generic Functions

Generic functions can also be overloaded.

```typescript
function identity(value: string): string;
function identity(value: number): number;

function identity<T extends string | number>(value: T): T {
    return value;
}

const name = identity("Alice"); // string
const age = identity(25);       // number
```

The generic implementation preserves the relationship between the input type and output type.

# 🗁 Constructor Overloads

TypeScript also supports constructor overloads in classes.

```typescript
class User {
    name: string;
    age: number;

    constructor(name: string);
    constructor(name: string, age: number);

    constructor(name: string, age: number = 18) {
        this.name = name;
        this.age = age;
    }
}

const user1 = new User("Alice");
const user2 = new User("Bob", 25);

console.log(user1);
console.log(user2);
```

Constructor overload signatures must be followed by a single implementation.

# 🗁 Method Overloads

Class methods can also have overload signatures.

```typescript
class Calculator {
    add(a: number, b: number): number;
    add(a: string, b: string): string;

    add(
        a: number | string,
        b: number | string
    ): number | string {
        if (typeof a === "number" && typeof b === "number") {
            return a + b;
        }

        if (typeof a === "string" && typeof b === "string") {
            return a + b;
        }

        throw new Error("Invalid arguments");
    }
}

const calculator = new Calculator();

console.log(calculator.add(10, 20));
console.log(calculator.add("Hello, ", "World"));
```

Method overloads are useful when a class provides multiple supported ways to perform an operation.

# 🗁 Arrow Functions and Overloading

Arrow functions do not support declaring multiple overload signatures directly using the same syntax as function declarations.

However, you can describe an overloaded callable type using an interface with multiple call signatures.

```typescript
interface Converter {
    (value: string): number;
    (value: number): string;
}

const convert: Converter = (value: string | number): string | number => {
    if (typeof value === "string") {
        return Number(value);
    }

    return String(value);
};

const numberValue = convert("100"); // number
const stringValue = convert(100);   // string
```

The interface defines the overloads, while the arrow function provides the implementation.

# 🗁 Overload Resolution

When a function is called, TypeScript examines its overload signatures and selects a compatible signature.

```typescript
function getLength(value: string): number;
function getLength(value: any[]): number;

function getLength(value: string | any[]): number {
    return value.length;
}

const textLength = getLength("TypeScript");
const arrayLength = getLength([10, 20, 30]);
```

TypeScript resolves:

* `string` arguments to the first overload.
* Array arguments to the second overload.

If no overload matches the supplied arguments, TypeScript reports an error.

# 🗁 Function Overloads vs Union Types

Overloads and union types can both support multiple input types, but they serve different purposes.

**Union type:**

```typescript
function format(value: string | number): string {
    return String(value);
}

format("Hello");
format(100);
```

**Function overloads:**

```typescript
function format(value: string): string[];
function format(value: number): number[];

function format(value: string | number): string[] | number[] {
    if (typeof value === "string") {
        return value.split("");
    }

    return [value];
}

format("Hello");
format(100);
```

Comparison:

| Feature | Union Types | Function Overloads |
| --- | --- | --- |
| Multiple input types | Yes | Yes |
| Different return types by input | Requires additional typing | Explicitly supported |
| Multiple calling signatures | No | Yes |
| Implementation complexity | Usually simpler | Can be more complex |
| Best use | Similar input behavior | Different input-output relationships |

Prefer union types when the function performs essentially the same operation regardless of input type.

Use overloads when different argument combinations require distinct behavior or return types.

# 🗁 Common Mistakes

**Providing an implementation without overload signatures**

```typescript
function getValue(value: string | number): string | number {
    return value;
}
```

This is a union-typed function, not an overloaded function.

**Calling a function with unsupported arguments**

```typescript
function print(value: string): void;
function print(value: number): void;

function print(value: string | number): void {
    console.log(value);
}

// Error: No overload matches this call.
print(true);
```

**Using incompatible implementation types**

```typescript
function calculate(value: string): number;
function calculate(value: number): number;

// Invalid implementation: boolean is not compatible
// with the declared overload parameter types.
function calculate(value: boolean): number {
    return value ? 1 : 0;
}
```

The implementation must support every declared overload.

# 🗁 Best Practices

* Use overloads when a function supports multiple distinct calling patterns.
* Keep overload signatures clear and specific.
* Place more specific overloads before more general ones when ordering affects resolution.
* Ensure the implementation supports every overload.
* Use union types when overloads do not provide additional type safety.
* Avoid unnecessary overloads that make function signatures difficult to understand.
* Document complex overload behavior.
* Test every supported calling pattern.
* Remember that callers can only use declared overload signatures, not the implementation signature.

# 🗁 Summary

* Function overloading allows multiple signatures for one function implementation.
* Overload signatures define supported function calls.
* The implementation signature contains the actual function body.
* The implementation must be compatible with every overload.
* Different overloads can provide different return types.
* Constructors and class methods support overloads.
* Arrow functions can implement overloaded callable interfaces.
* TypeScript resolves calls by matching arguments against overload signatures.
* Use overloads when they provide clearer and more precise typing than union types.
