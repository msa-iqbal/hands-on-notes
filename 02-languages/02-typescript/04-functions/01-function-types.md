# Function Types

Functions are one of the fundamental building blocks of TypeScript applications.

TypeScript allows you to define function types to describe:

* The types of function parameters.
* The function's return type.
* The number and order of arguments.
* The overall structure of a callable value.

Function types help ensure that functions receive the correct arguments and return values that match their intended behavior.

# 🗁 Basic Function Type

A function type describes the parameters and return type of a function.

The general syntax is:

```typescript
(parameter1: Type, parameter2: Type) => ReturnType
```

Example:

```typescript
let add: (a: number, b: number) => number;

add = (a, b) => {
    return a + b;
};

console.log(add(10, 20)); // 30
```

Here:

* `a: number` defines the first parameter.
* `b: number` defines the second parameter.
* `=> number` specifies the return type.
* `add` can only reference a function compatible with this signature.

# 🗁 Function Type Annotation

You can explicitly define a function's parameter and return types.

```typescript
function multiply(a: number, b: number): number {
    return a * b;
}
```

The function accepts two numbers and returns a number.

TypeScript checks the function implementation against its declared types.

```typescript
function greet(name: string): string {
    return `Hello, ${name}!`;
}

console.log(greet("Alice"));
```

# 🗁 Function Type Alias

You can use a type alias to create reusable function types.

```typescript
type MathOperation = (a: number, b: number) => number;
```

Use the alias when declaring variables:

```typescript
type MathOperation = (a: number, b: number) => number;

const add: MathOperation = (a, b) => a + b;

const subtract: MathOperation = (a, b) => a - b;

const multiply: MathOperation = (a, b) => a * b;

console.log(add(10, 5));      // 15
console.log(subtract(10, 5)); // 5
console.log(multiply(10, 5)); // 50
```

A single function type can describe multiple compatible functions.

# 🗁 Function Type with Multiple Parameters

A function type can define any number of parameters.

```typescript
type CreateUser = (
    name: string,
    email: string,
    age: number
) => string;

const createUser: CreateUser = (name, email, age) => {
    return `${name} (${email}), age ${age}`;
};

console.log(createUser("Alice", "alice@example.com", 25));
```

Parameter types and their positions are part of the function signature.

# 🗁 Function Type with No Parameters

A function type can describe a function that accepts no arguments.

```typescript
type GetMessage = () => string;

const getMessage: GetMessage = () => {
    return "Welcome to TypeScript!";
};

console.log(getMessage());
```

The empty parentheses indicate that the function does not accept parameters.

# 🗁 Function Type with Void Return

Use `void` when a function does not return a meaningful value to its caller.

```typescript
type Logger = (message: string) => void;

const logMessage: Logger = (message) => {
    console.log(message);
};

logMessage("Application started");
```

A `void` function may still return `undefined` implicitly.

For function expressions and callbacks, TypeScript also permits a value-returning implementation to be assigned to a `void`-returning function type when the returned value is intentionally ignored.

```typescript
type Logger = (message: string) => void;

const logger: Logger = (message) => {
    return console.log(message);
};
```

The caller treats the function's result as `void`.

# 🗁 Function Type with Object Parameters

Function types can accept objects with defined structures.

```typescript
type User = {
    id: number;
    name: string;
    email: string;
};

type DisplayUser = (user: User) => void;

const displayUser: DisplayUser = (user) => {
    console.log(user.name);
    console.log(user.email);
};

const user: User = {
    id: 1,
    name: "Alice",
    email: "alice@example.com"
};

displayUser(user);
```

This approach is useful when functions operate on structured application data.

# 🗁 Function Type with Array Parameters

Function types can also accept arrays.

```typescript
type CalculateTotal = (prices: number[]) => number;

const calculateTotal: CalculateTotal = (prices) => {
    return prices.reduce((total, price) => total + price, 0);
};

console.log(calculateTotal([100, 200, 300])); // 600
```

The parameter type ensures that the function receives an array of numbers.

# 🗁 Function Types in Object Properties

You can define function types directly inside object types.

```typescript
type UserService = {
    getUser: (id: number) => string;
    deleteUser: (id: number) => void;
};

const userService: UserService = {
    getUser: (id) => {
        return `User ${id}`;
    },

    deleteUser: (id) => {
        console.log(`Deleting user ${id}`);
    }
};

console.log(userService.getUser(10));
userService.deleteUser(10);
```

This pattern is common when defining service objects, API clients, and application interfaces.

# 🗁 Function Type in Interfaces

Interfaces can describe callable properties.

```typescript
interface Calculator {
    add: (a: number, b: number) => number;
    subtract: (a: number, b: number) => number;
}

const calculator: Calculator = {
    add: (a, b) => a + b,
    subtract: (a, b) => a - b
};

console.log(calculator.add(10, 5));      // 15
console.log(calculator.subtract(10, 5)); // 5
```

An interface can also describe an object that is itself callable:

```typescript
interface Formatter {
    (value: string): string;
}

const uppercase: Formatter = (value) => {
    return value.toUpperCase();
};

console.log(uppercase("typescript")); // TYPESCRIPT
```

The second example describes a call signature rather than a function stored in a named property.

# 🗁 Function Type with Callback Parameters

A function type can describe callbacks passed to another function.

```typescript
type Callback = (message: string) => void;

function processMessage(
    message: string,
    callback: Callback
): void {
    callback(message);
}

processMessage("Task completed", (message) => {
    console.log(message);
});
```

Callback function types are widely used in event handling, asynchronous operations, and array methods.

# 🗁 Contextual Typing

TypeScript can infer parameter types from the function type assigned to a variable.

```typescript
type Operation = (a: number, b: number) => number;

const add: Operation = (a, b) => {
    return a + b;
};
```

You do not need to annotate `a` and `b` again because TypeScript already knows their types from `Operation`.

This behavior is called contextual typing.

# 🗁 Function Type Compatibility

TypeScript checks whether a function can be assigned to a variable with a particular function type.

```typescript
type Formatter = (value: string) => string;

const uppercase: Formatter = (value) => {
    return value.toUpperCase();
};
```

The implementation is compatible because it accepts a string and returns a string.

An incompatible return type produces an error:

```typescript
type Formatter = (value: string) => string;

// Error: Type 'number' is not assignable to type 'string'.
const formatter: Formatter = (value) => {
    return value.length;
};
```

Parameter compatibility also matters:

```typescript
type Formatter = (value: string) => string;

// Error: The parameter type is incompatible.
const formatter: Formatter = (value: number) => {
    return value.toString();
};
```

Function compatibility has additional rules involving parameter variance and optional parameters, which are covered in advanced TypeScript topics.

# 🗁 Practical Example

Consider a small application that supports multiple payment methods.

```typescript
type PaymentHandler = (
    amount: number,
    currency: string
) => boolean;

const processCardPayment: PaymentHandler = (amount, currency) => {
    console.log(`Processing card payment: ${amount} ${currency}`);
    return true;
};

const processCashPayment: PaymentHandler = (amount, currency) => {
    console.log(`Processing cash payment: ${amount} ${currency}`);
    return true;
};

function checkout(
    amount: number,
    currency: string,
    paymentHandler: PaymentHandler
): boolean {
    return paymentHandler(amount, currency);
}

checkout(100, "USD", processCardPayment);
checkout(50, "USD", processCashPayment);
```

Both payment handlers follow the same function contract.

This makes it easier to replace implementations without changing the checkout function.

# 🗁 Best Practices

* Define explicit function types when they are reused across multiple variables or functions.
* Use type aliases for reusable function signatures.
* Use interfaces when describing callable objects or objects containing function properties.
* Let TypeScript infer parameter types when contextual typing provides enough information.
* Use `void` for functions whose return values are not intended to be consumed.
* Keep function signatures focused and easy to understand.
* Use descriptive names for function type aliases.
* Avoid repeating complex function signatures throughout an application.

# 🗁 Summary

* A function type defines the structure of a function.
* Function types describe parameter types and return types.
* Type aliases make function signatures reusable.
* Interfaces can describe callable objects and function properties.
* Callback parameters can use function types.
* Contextual typing allows TypeScript to infer parameter types.
* TypeScript checks function compatibility during compilation.
