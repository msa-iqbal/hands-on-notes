# Function Parameters

Function parameters are variables declared in a function definition that receive values when the function is called.

TypeScript allows you to specify parameter types to ensure that functions receive values of the expected types.

Parameters help define the input contract of a function and make code easier to understand, maintain, and debug.

# 🗁 Basic Function Parameters

In TypeScript, you can specify a type for each function parameter.

```typescript
function greet(name: string): void {
    console.log(`Hello, ${name}!`);
}

greet("Alice");
```

Here:

* `name` is the parameter.
* `string` is the parameter type.
* `"Alice"` is the argument passed to the function.
* `void` indicates that the function does not return a meaningful value.

# 🗁 Multiple Parameters

A function can accept multiple parameters.

```typescript
function add(a: number, b: number): number {
    return a + b;
}

console.log(add(10, 20)); // 30
```

Each parameter can have a different type.

```typescript
function createUser(
    name: string,
    age: number,
    isActive: boolean
): string {
    return `${name}, ${age}, Active: ${isActive}`;
}

console.log(createUser("Alice", 25, true));
```

TypeScript checks the type and position of every argument.

# 🗁 Parameter Order

Arguments are assigned to parameters according to their position.

```typescript
function introduce(name: string, age: number): void {
    console.log(`${name} is ${age} years old.`);
}

introduce("Alice", 25);
```

Passing arguments in the wrong order causes a type error:

```typescript
function introduce(name: string, age: number): void {
    console.log(`${name} is ${age} years old.`);
}

// Error: Argument of type 'number' is not assignable to parameter of type 'string'.
introduce(25, "Alice");
```

The argument order must match the function's parameter order unless another supported parameter pattern is used.

# 🗁 Parameter Type Inference

TypeScript can infer parameter types from contextual information.

For example, when assigning a function to a variable with an explicit function type:

```typescript
type Calculator = (a: number, b: number) => number;

const add: Calculator = (a, b) => {
    return a + b;
};
```

TypeScript automatically understands that `a` and `b` are numbers.

However, standalone function parameters generally need explicit type annotations unless the compiler can infer their types from other contextual information.

# 🗁 Default Parameter Values

A parameter can have a default value.

```typescript
function greet(name: string = "Guest"): string {
    return `Hello, ${name}!`;
}

console.log(greet());        // Hello, Guest!
console.log(greet("Alice")); // Hello, Alice!
```

When an argument is omitted or explicitly passed as `undefined`, the default value is used.

```typescript
function calculatePrice(price: number, discount: number = 0): number {
    return price - discount;
}

console.log(calculatePrice(100));      // 100
console.log(calculatePrice(100, 20));  // 80
console.log(calculatePrice(100, undefined)); // 100
```

# 🗁 Optional Parameters

Use `?` to declare a parameter as optional.

```typescript
function greet(name: string, age?: number): void {
    if (age !== undefined) {
        console.log(`${name} is ${age} years old.`);
    } else {
        console.log(`Hello, ${name}!`);
    }
}

greet("Alice");
greet("Bob", 30);
```

An optional parameter can be omitted when calling the function.

Inside the function, TypeScript treats an optional parameter as potentially `undefined`.

```typescript
function printMessage(message?: string): void {
    console.log(message ?? "No message provided");
}

printMessage();
printMessage("Hello");
```

Optional parameters must appear after required parameters.

```typescript
// Valid
function createUser(name: string, age?: number): void {
    console.log(name, age);
}

// Invalid: A required parameter cannot follow an optional parameter.
function createProduct(name?: string, price: number): void {
    console.log(name, price);
}
```

# 🗁 Rest Parameters

Rest parameters allow a function to receive any number of arguments.

Use `...` before the parameter name.

```typescript
function sum(...numbers: number[]): number {
    return numbers.reduce((total, number) => total + number, 0);
}

console.log(sum(10, 20));       // 30
console.log(sum(10, 20, 30));   // 60
console.log(sum(5, 10, 15, 20)); // 50
```

A rest parameter collects all remaining arguments into an array.

Rest parameters must be the last parameter in a function definition.

```typescript
function logMessages(prefix: string, ...messages: string[]): void {
    for (const message of messages) {
        console.log(`${prefix}: ${message}`);
    }
}

logMessages("INFO", "Server started", "Database connected");
```

# 🗁 Object Parameters

Functions can accept objects as parameters.

You can define the object structure directly in the parameter annotation.

```typescript
function displayUser(user: {
    id: number;
    name: string;
    email: string;
}): void {
    console.log(user.name);
    console.log(user.email);
}

displayUser({
    id: 1,
    name: "Alice",
    email: "alice@example.com"
});
```

For reusable structures, define a type alias.

```typescript
type User = {
    id: number;
    name: string;
    email: string;
};

function displayUser(user: User): void {
    console.log(`${user.name} (${user.email})`);
}
```

This makes function signatures cleaner and easier to maintain.

# 🗁 Array Parameters

You can pass arrays as function parameters.

```typescript
function calculateTotal(prices: number[]): number {
    return prices.reduce((total, price) => total + price, 0);
}

const prices = [100, 200, 300];

console.log(calculateTotal(prices)); // 600
```

You can also use the generic array syntax:

```typescript
function getNames(names: Array<string>): string[] {
    return names;
}
```

Both `string[]` and `Array<string>` represent arrays of strings.

# 🗁 Union Type Parameters

A parameter can accept more than one type using a union type.

```typescript
function printId(id: string | number): void {
    console.log(`ID: ${id}`);
}

printId(101);
printId("USER-101");
```

Use type narrowing when an operation requires a specific type.

```typescript
function formatId(id: string | number): string {
    if (typeof id === "number") {
        return id.toFixed(0);
    }

    return id.toUpperCase();
}

console.log(formatId(101));       // "101"
console.log(formatId("user-101")); // "USER-101"
```

# 🗁 Readonly Array Parameters

Use `readonly` when a function should accept an array without modifying its contents.

```typescript
function calculateTotal(prices: readonly number[]): number {
    return prices.reduce((total, price) => total + price, 0);
}

const prices = [100, 200, 300];

console.log(calculateTotal(prices)); // 600
```

The function cannot modify the array through the parameter.

```typescript
function processItems(items: readonly string[]): void {
    console.log(items.length);

    // Error: Cannot assign to '0' because it is a read-only property.
    // items[0] = "Updated";
}
```

This is useful when a function only needs to read input data.

# 🗁 Destructured Parameters

You can destructure object parameters directly in the function signature.

```typescript
type User = {
    name: string;
    age: number;
};

function displayUser({ name, age }: User): void {
    console.log(`${name} is ${age} years old.`);
}

displayUser({
    name: "Alice",
    age: 25
});
```

You can also destructure array parameters.

```typescript
function displayCoordinates([x, y]: [number, number]): void {
    console.log(`X: ${x}, Y: ${y}`);
}

displayCoordinates([10, 20]);
```

Destructuring provides direct access to object properties or array elements.

# 🗁 Function Parameters with Callbacks

A function can accept another function as a parameter.

```typescript
function processNumber(
    value: number,
    callback: (result: number) => void
): void {
    const result = value * 2;

    callback(result);
}

processNumber(10, (result) => {
    console.log(result); // 20
});
```

For reusable callback signatures, define a type alias.

```typescript
type Callback = (result: number) => void;

function executeTask(value: number, callback: Callback): void {
    callback(value * 2);
}
```

# 🗁 Parameter Validation

TypeScript checks parameter types during compilation, but it does not automatically validate external runtime data.

For example, data received from an API may not match the expected type.

```typescript
type User = {
    name: string;
    age: number;
};

function displayUser(user: User): void {
    console.log(user.name);
    console.log(user.age);
}
```

The `User` type does not guarantee that an API response actually contains valid user data.

Validate untrusted values before passing them to functions:

```typescript
function isUser(value: unknown): value is User {
    if (typeof value !== "object" || value === null) {
        return false;
    }

    return (
        "name" in value &&
        typeof value.name === "string" &&
        "age" in value &&
        typeof value.age === "number"
    );
}

const response: unknown = {
    name: "Alice",
    age: 25
};

if (isUser(response)) {
    displayUser(response);
}
```

This separates compile-time type checking from runtime data validation.

# 🗁 Best Practices

* Specify meaningful parameter types for standalone functions.
* Use descriptive parameter names.
* Keep required parameters before optional parameters.
* Use default parameters when a sensible fallback value exists.
* Use rest parameters when a function accepts a variable number of arguments.
* Use type aliases or interfaces for reusable object parameter structures.
* Use `readonly` array parameters when the function should not modify input arrays.
* Use union types when a parameter legitimately accepts multiple types.
* Validate external data at runtime instead of trusting TypeScript annotations.
* Avoid excessive parameters; consider using a configuration object for complex functions.

# 🗁 Summary

* Function parameters define the inputs a function accepts.
* Type annotations specify the expected type of each parameter.
* Multiple parameters are matched with arguments by position.
* Optional parameters use `?`.
* Default parameters provide fallback values.
* Rest parameters collect remaining arguments into an array.
* Object and array parameters support structured input.
* Union types allow parameters to accept multiple types.
* Readonly parameters help prevent accidental input mutation.
* TypeScript parameter annotations do not replace runtime validation.
