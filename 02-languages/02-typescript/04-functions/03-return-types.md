# Function Return Types

A function return type defines the type of value that a function can return to its caller.

TypeScript allows you to explicitly declare return types so that the compiler can verify whether a function returns the expected value.

Return types improve code readability, prevent unexpected return values, and make function contracts easier to understand.

# 🗁 Basic Return Type

Specify a return type after the function's parameter list using a colon (`:`).

```typescript
function add(a: number, b: number): number {
    return a + b;
}

const result = add(10, 20);

console.log(result); // 30
```

Here:

* `a: number` defines the first parameter.
* `b: number` defines the second parameter.
* `: number` specifies the return type.
* `return a + b` returns a number.

# 🗁 String Return Type

A function can return a string.

```typescript
function greet(name: string): string {
    return `Hello, ${name}!`;
}

const message = greet("Alice");

console.log(message); // Hello, Alice!
```

The function must return a string because its declared return type is `string`.

# 🗁 Boolean Return Type

Use `boolean` when a function returns either `true` or `false`.

```typescript
function isAdult(age: number): boolean {
    return age >= 18;
}

console.log(isAdult(25)); // true
console.log(isAdult(15)); // false
```

Boolean return types are commonly used for validation and conditional checks.

# 🗁 Void Return Type

Use `void` when a function does not return a meaningful value.

```typescript
function logMessage(message: string): void {
    console.log(message);
}

logMessage("Application started");
```

A `void` function may return `undefined` implicitly.

```typescript
function displayMessage(): void {
    console.log("Hello TypeScript!");
}
```

A function declared with `void` cannot return an ordinary value as its result:

```typescript
function getNumber(): void {
    // Error: Type 'number' is not assignable to type 'void'.
    return 100;
}
```

# 🗁 Undefined Return Type

A function can explicitly return `undefined`.

```typescript
function getValue(): undefined {
    return undefined;
}

const result = getValue();

console.log(result); // undefined
```

The difference between `void` and `undefined` is important:

* `void` means the function's return value is not intended to be used.
* `undefined` means the function specifically returns the value `undefined`.

# 🗁 Never Return Type

Use `never` when a function never completes normally by returning a value.

This commonly happens when a function always throws an exception or enters an infinite loop.

```typescript
function throwError(message: string): never {
    throw new Error(message);
}
```

Another example:

```typescript
function infiniteLoop(): never {
    while (true) {
        console.log("Running...");
    }
}
```

The `never` type indicates that normal execution cannot produce a return value.

# 🗁 Array Return Type

A function can return an array of values.

```typescript
function getNumbers(): number[] {
    return [10, 20, 30, 40];
}

const numbers = getNumbers();

console.log(numbers);
```

You can also use the generic array syntax:

```typescript
function getNames(): Array<string> {
    return ["Alice", "Bob", "Charlie"];
}
```

Both `number[]` and `Array<number>` represent arrays of numbers.

# 🗁 Object Return Type

A function can return an object with a defined structure.

```typescript
type User = {
    id: number;
    name: string;
    email: string;
};

function getUser(): User {
    return {
        id: 1,
        name: "Alice",
        email: "alice@example.com"
    };
}

const user = getUser();

console.log(user.name);
```

Using a type alias makes object return types reusable.

# 🗁 Union Return Types

A function can return values of different types using a union type.

```typescript
function getValue(isNumber: boolean): string | number {
    if (isNumber) {
        return 100;
    }

    return "One hundred";
}

const result = getValue(true);

console.log(result);
```

The function must return either a string or a number.

When consuming the result, use type narrowing before performing type-specific operations.

```typescript
const value = getValue(false);

if (typeof value === "number") {
    console.log(value.toFixed(2));
} else {
    console.log(value.toUpperCase());
}
```

# 🗁 Literal Return Types

A function can return a specific literal value instead of a general type.

```typescript
function getStatus(): "success" {
    return "success";
}
```

You can also use a union of literal types:

```typescript
type Status = "success" | "error" | "pending";

function getStatus(isSuccessful: boolean): Status {
    return isSuccessful ? "success" : "error";
}
```

Literal return types are useful when a function has a limited set of possible results.

# 🗁 Promise Return Types

An asynchronous function returns a `Promise`.

Use `Promise<T>` to describe the type of the value that the promise eventually fulfills with.

```typescript
async function getUser(): Promise<string> {
    return "Alice";
}
```

The function returns a `Promise<string>`, not a plain string.

You can consume the result using `await`:

```typescript
async function main(): Promise<void> {
    const user = await getUser();

    console.log(user);
}

main();
```

A promise that does not fulfill with a meaningful value can use `Promise<void>`:

```typescript
async function saveData(): Promise<void> {
    console.log("Data saved");
}
```

# 🗁 Type Inference for Return Types

TypeScript can automatically infer a function's return type.

```typescript
function add(a: number, b: number) {
    return a + b;
}
```

TypeScript infers the return type as `number`.

Another example:

```typescript
function getUser() {
    return {
        id: 1,
        name: "Alice"
    };
}
```

TypeScript infers an object return type containing `id: number` and `name: string`.

Explicit return annotations are not always necessary, but they can make important function contracts clearer.

# 🗁 Return Type from Conditional Statements

When a function has multiple execution paths, every path must satisfy its declared return type.

```typescript
function getDiscount(isMember: boolean): number {
    if (isMember) {
        return 20;
    }

    return 0;
}
```

An incomplete function can produce a compiler error:

```typescript
function getDiscount(isMember: boolean): number {
    if (isMember) {
        return 20;
    }

    // Error: Not all code paths return a value.
}
```

The function must return a number for every possible execution path.

# 🗁 Using ReturnType Utility Type

TypeScript provides the built-in `ReturnType<T>` utility type to extract the return type of a function type.

```typescript
function getUser() {
    return {
        id: 1,
        name: "Alice"
    };
}

type User = ReturnType<typeof getUser>;
```

The resulting `User` type is equivalent to:

```typescript
type User = {
    id: number;
    name: string;
};
```

This is useful when you want to reuse an existing function's return type without defining the structure again.

# 🗁 Practical Example

Consider a user service that returns different results depending on whether a user exists.

```typescript
type User = {
    id: number;
    name: string;
    email: string;
};

type UserResult = User | null;

function findUser(id: number): UserResult {
    if (id === 1) {
        return {
            id: 1,
            name: "Alice",
            email: "alice@example.com"
        };
    }

    return null;
}

const user = findUser(1);

if (user !== null) {
    console.log(user.name);
} else {
    console.log("User not found");
}
```

The return type `User | null` clearly communicates that the function may return a user or no result.

# 🗁 Best Practices

* Define explicit return types for important public functions and APIs.
* Use `void` when callers should not consume a function's return value.
* Use `never` for functions that cannot complete normally.
* Use `Promise<T>` for asynchronous functions.
* Use union types when a function legitimately returns different kinds of values.
* Use literal return types when the possible results are limited and meaningful.
* Ensure every execution path satisfies the declared return type.
* Use `ReturnType<T>` to reuse existing function return types.
* Allow TypeScript to infer simple return types when explicit annotations add little value.

# 🗁 Summary

* Return types define what a function can return.
* TypeScript supports primitive, object, array, union, and literal return types.
* `void` represents a function whose return value is not intended to be consumed.
* `undefined` represents the specific value `undefined`.
* `never` represents execution that cannot complete normally.
* `Promise<T>` describes the eventual fulfillment value of an asynchronous operation.
* TypeScript can infer return types automatically.
* `ReturnType<T>` extracts the return type of an existing function type.
