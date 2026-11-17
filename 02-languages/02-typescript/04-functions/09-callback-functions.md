# Callback Functions

A callback function is a function passed as an argument to another function so that it can be executed later.

Callbacks are commonly used in JavaScript and TypeScript for:

* Event handling.
* Array operations.
* Asynchronous operations.
* API responses.
* Reusable function behavior.
* Custom processing logic.

TypeScript allows you to define callback parameter types, return types, optional parameters, and generic callback contracts.

# 🗁 Basic Callback Function

A callback function is passed to another function as an argument.

```typescript
function processUser(
  name: string,
  callback: (name: string) => void
): void {
  callback(name);
}

function greet(name: string): void {
  console.log(`Hello, ${name}!`);
}

processUser("Alice", greet);
```

Output:

```text
Hello, Alice!
```

In this example:

* `processUser` is the higher-order function.
* `greet` is the callback function.
* `(name: string) => void` describes the callback's function type.
* `callback(name)` executes the callback.

# 🗁 Inline Callback Functions

You do not need to declare a callback separately. You can define it directly where it is passed.

```typescript
function processNumber(
  value: number,
  callback: (result: number) => void
): void {
  callback(value * 2);
}

processNumber(10, result => {
  console.log(result);
});
```

Output:

```text
20
```

TypeScript infers the type of `result` as `number` from the callback parameter type.

# 🗁 Callback with Return Value

A callback can return a value that the calling function uses.

```typescript
function calculate(
  a: number,
  b: number,
  operation: (x: number, y: number) => number
): number {
  return operation(a, b);
}

const add = (x: number, y: number): number => x + y;

const result = calculate(10, 20, add);

console.log(result);
```

Output:

```text
30
```

The callback contract specifies:

* Two numeric parameters.
* A numeric return value.

# 🗁 Defining Callback Types with Type Aliases

When a callback type is reused, define it with a type alias.

```typescript
type Callback = (message: string) => void;

function executeCallback(callback: Callback): void {
  callback("Operation completed");
}

const logMessage: Callback = message => {
  console.log(message);
};

executeCallback(logMessage);
```

Type aliases make function signatures easier to read and maintain.

# 🗁 Defining Callback Types with Interfaces

Interfaces can also describe callback function signatures.

```typescript
interface Formatter {
  (value: string): string;
}

const uppercase: Formatter = value => {
  return value.toUpperCase();
};

console.log(uppercase("typescript"));
```

Output:

```text
TYPESCRIPT
```

A callable interface is useful when you want to represent a function as a named contract.

# 🗁 Optional Callback Parameters

A callback parameter can be optional.

```typescript
type CompletionCallback = (message?: string) => void;

function completeTask(callback: CompletionCallback): void {
  callback("Task completed");
}

completeTask(message => {
  console.log(message ?? "No message provided");
});
```

Output:

```text
Task completed
```

The `?` indicates that the callback parameter may be omitted.

A function assigned to this callback type must be compatible with the declared signature.

# 🗁 Callbacks with Multiple Parameters

Callbacks can accept multiple parameters.

```typescript
type UserCallback = (
  id: number,
  name: string,
  active: boolean
) => void;

function loadUser(callback: UserCallback): void {
  callback(101, "Alice", true);
}

loadUser((id, name, active) => {
  console.log(id, name, active);
});
```

Output:

```text
101 Alice true
```

TypeScript infers the callback parameter types from `UserCallback`.

# 🗁 Callback with Object Parameters

Callbacks frequently receive objects containing structured data.

```typescript
interface User {
  id: number;
  name: string;
  email: string;
}

type UserCallback = (user: User) => void;

function getUser(callback: UserCallback): void {
  const user: User = {
    id: 1,
    name: "Alice",
    email: "alice@example.com",
  };

  callback(user);
}

getUser(user => {
  console.log(user.name);
  console.log(user.email);
});
```

Using an interface for callback data makes the expected object structure explicit.

# 🗁 Generic Callback Functions

Generic callback types allow a callback to work with different data types.

```typescript
type Callback<T> = (value: T) => void;

function processValue<T>(
  value: T,
  callback: Callback<T>
): void {
  callback(value);
}

processValue<number>(100, value => {
  console.log(value.toFixed(2));
});

processValue<string>("TypeScript", value => {
  console.log(value.toUpperCase());
});
```

Output:

```text
100.00
TYPESCRIPT
```

The generic type parameter ensures that the input value and callback parameter use compatible types.

# 🗁 Callbacks in Array Methods

JavaScript array methods use callbacks extensively. TypeScript infers callback parameter types from the array.

**Using `forEach()`**

```typescript
const numbers: number[] = [10, 20, 30];

numbers.forEach((number, index) => {
  console.log(index, number);
});
```

**Using `map()`**

```typescript
const numbers: number[] = [1, 2, 3];

const doubled = numbers.map(number => number * 2);

console.log(doubled);
```

**Using `filter()`**

```typescript
const ages: number[] = [15, 20, 25, 17, 30];

const adults = ages.filter(age => age >= 18);

console.log(adults);
```

**Using `reduce()`**

```typescript
const prices: number[] = [100, 200, 300];

const total = prices.reduce(
  (sum, price) => sum + price,
  0
);

console.log(total);
```

# 🗁 Asynchronous Callback Functions

Callbacks can execute after an asynchronous operation completes.

```typescript
function fetchData(
  callback: (data: string) => void
): void {
  setTimeout(() => {
    callback("Data loaded successfully");
  }, 1000);
}

fetchData(data => {
  console.log(data);
});
```

The callback executes after the timer completes.

The `setTimeout` function is asynchronous, so the rest of the program can continue while the timer is waiting.

# 🗁 Error-First Callback Pattern

Some Node.js APIs use an error-first callback pattern.

The first parameter represents an error, while the second represents the result.

```typescript
type ReadCallback = (
  error: Error | null,
  data?: string
) => void;

function readData(callback: ReadCallback): void {
  const error: Error | null = null;
  const data = "File content";

  callback(error, data);
}

readData((error, data) => {
  if (error) {
    console.error(error.message);
    return;
  }

  console.log(data);
});
```

A more realistic Node.js example:

```typescript
import { readFile } from "node:fs";

readFile("example.txt", "utf8", (error, data) => {
  if (error) {
    console.error(error.message);
    return;
  }

  console.log(data);
});
```

The Node.js type definitions provide the callback signature for `readFile`.

# 🗁 Callback Compatibility

TypeScript checks whether a function is compatible with the expected callback signature.

```typescript
type NumberCallback = (value: number) => void;

const printNumber: NumberCallback = value => {
  console.log(value);
};

function execute(callback: NumberCallback): void {
  callback(100);
}

execute(printNumber);
```

A function with fewer parameters can generally be assigned to a callback type that provides more parameters because it can safely ignore the unused arguments.

```typescript
type DataCallback = (
  value: string,
  index: number
) => void;

const logValue = (value: string): void => {
  console.log(value);
};

const callback: DataCallback = logValue;
```

This is useful when a callback only needs some of the information supplied by the calling API.

# 🗁 Common Mistakes

**Passing the result instead of the function**

Incorrect:

```typescript
function greet(name: string): void {
  console.log(`Hello, ${name}`);
}

// Executes immediately and passes undefined.
setTimeout(greet("Alice"), 1000);
```

Correct:

```typescript
setTimeout(() => {
  greet("Alice");
}, 1000);
```

**Using an incompatible callback signature**

Incorrect:

```typescript
type NumberCallback = (value: number) => void;

const printName = (name: string): void => {
  console.log(name);
};

// Type error: string parameter is incompatible.
const callback: NumberCallback = printName;
```

Correct:

```typescript
const printNumber: NumberCallback = value => {
  console.log(value);
};
```

**Forgetting to handle errors**

When working with error-first callbacks, check the error before using the result.

```typescript
readFile("example.txt", "utf8", (error, data) => {
  if (error) {
    console.error(error);
    return;
  }

  console.log(data);
});
```

**Creating deeply nested callbacks**

Excessive nested callbacks can make code difficult to read and maintain. This problem is often called callback hell.

For complex asynchronous workflows, Promises and `async`/`await` can provide a clearer structure.

# 🗁 Best Practices

* Define reusable callback signatures with type aliases or interfaces.
* Use descriptive callback parameter names.
* Specify meaningful return types.
* Prefer inline arrow functions for short, local callbacks.
* Use named functions when callback logic is complex or reused.
* Handle errors explicitly in asynchronous callbacks.
* Avoid deeply nested callback structures.
* Use generics when callbacks should support multiple data types.
* Prefer Promises or `async`/`await` for complex asynchronous workflows.

# 🗁 Summary

* A callback is a function passed to another function as an argument.
* Callbacks can be synchronous or asynchronous.
* TypeScript supports callback parameter and return type annotations.
* Callback signatures can be defined using type aliases and interfaces.
* Generic callbacks support reusable type-safe behavior.
* Array methods such as `map()`, `filter()`, and `reduce()` use callbacks.
* Error-first callbacks are common in Node.js APIs.
* Promises and `async`/`await` can simplify complex asynchronous workflows.

# 🗁 Related Topics

* Function Types
* Function Parameters
* Return Types
* Arrow Functions
* Function Overloads
* Generics
* Promises
* Async and Await
* Error Handling
