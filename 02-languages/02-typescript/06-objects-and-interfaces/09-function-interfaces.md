# Function Interfaces

Function interfaces are TypeScript interfaces that describe the structure of functions.

They define:

* Function parameter types.
* Parameter order.
* Optional and required parameters.
* Return types.
* Function signatures.
* Callback function contracts.

Function interfaces help ensure that functions follow a consistent structure throughout an application.

They are especially useful when working with callbacks, event handlers, reusable function contracts, and dependency injection.

# 🗁 Basic Syntax

An interface can describe a function using a call signature.

```typescript
interface FunctionName {
  (parameter: Type): ReturnType;
}
```

Example:

```typescript
interface Greeting {
  (name: string): string;
}
```

This interface describes a function that:

* Accepts one `string` parameter.
* Returns a `string`.

Create a function that follows this interface:

```typescript
const greet: Greeting = (name) => {
  return `Hello, ${name}!`;
};

console.log(greet("Alice"));
```

Output:

```text
Hello, Alice!
```

TypeScript checks whether the assigned function satisfies the interface.

# 🗁 Function Type Signatures

A function signature defines the parameters and return type of a function without providing its implementation.

```typescript
interface Calculator {
  (a: number, b: number): number;
}
```

Example:

```typescript
const add: Calculator = (a, b) => {
  return a + b;
};

const subtract: Calculator = (a, b) => {
  return a - b;
};

const multiply: Calculator = (a, b) => {
  return a * b;
};

console.log(add(10, 5));
console.log(subtract(10, 5));
console.log(multiply(10, 5));
```

All three functions follow the same interface.

# 🗁 Function Interfaces with Multiple Parameters

A function interface can define multiple parameters.

```typescript
interface UserCreator {
  (name: string, age: number, email: string): void;
}
```

Example:

```typescript
const createUser: UserCreator = (name, age, email) => {
  console.log({
    name,
    age,
    email,
  });
};

createUser("Alice", 25, "alice@example.com");
```

The implementation must accept compatible parameter types in the declared order.

# 🗁 Function Interfaces with Optional Parameters

Function interfaces can define optional parameters using `?`.

```typescript
interface Logger {
  (message: string, level?: string): void;
}
```

Example:

```typescript
const log: Logger = (message, level) => {
  console.log(`[${level ?? "INFO"}] ${message}`);
};

log("Application started");
log("Something went wrong", "ERROR");
```

The second parameter can be omitted.

# 🗁 Function Interfaces with Default Parameters

A function implementation can provide default values for parameters.

```typescript
interface Greeting {
  (name: string, message?: string): string;
}
```

Implementation:

```typescript
const greet: Greeting = (name, message = "Welcome") => {
  return `${message}, ${name}!`;
};

console.log(greet("Alice"));
console.log(greet("Bob", "Good morning"));
```

The interface describes the parameter as optional, while the implementation provides its default value.

# 🗁 Function Interfaces with Rest Parameters

Rest parameters allow a function to accept multiple arguments.

```typescript
interface NumberProcessor {
  (...numbers: number[]): number;
}
```

Example:

```typescript
const sum: NumberProcessor = (...numbers) => {
  return numbers.reduce((total, number) => total + number, 0);
};

console.log(sum(10, 20, 30));
console.log(sum(5, 10, 15, 20));
```

The rest parameter must be an array type.

# 🗁 Function Interfaces with Return Types

A function interface can specify different return types.

String return:

```typescript
interface StringFunction {
  (value: string): string;
}
```

Number return:

```typescript
interface NumberFunction {
  (value: number): number;
}
```

Boolean return:

```typescript
interface BooleanFunction {
  (value: number): boolean;
}
```

Implementations:

```typescript
const uppercase: StringFunction = (value) => {
  return value.toUpperCase();
};

const double: NumberFunction = (value) => {
  return value * 2;
};

const isPositive: BooleanFunction = (value) => {
  return value > 0;
};
```

Each implementation must return a value compatible with its interface.

# 🗁 Function Interfaces Returning Void

Use `void` when a function's return value is not intended to be used.

```typescript
interface MessageHandler {
  (message: string): void;
}
```

Example:

```typescript
const showMessage: MessageHandler = (message) => {
  console.log(message);
};

showMessage("Hello TypeScript");
```

A function that returns a value can also be assigned to a `void`-returning function type.

```typescript
const showNumber: (value: number) => void = (value) => {
  return value * 2;
};
```

The returned value is ignored by callers using the `void` function type.

# 🗁 Function Interfaces with Object Parameters

A function interface can accept an object as a parameter.

```typescript
interface User {
  id: number;
  name: string;
  email: string;
}

interface UserHandler {
  (user: User): void;
}
```

Example:

```typescript
const displayUser: UserHandler = (user) => {
  console.log(`ID: ${user.id}`);
  console.log(`Name: ${user.name}`);
  console.log(`Email: ${user.email}`);
};

displayUser({
  id: 1,
  name: "Alice",
  email: "alice@example.com",
});
```

This approach is useful when functions operate on structured application data.

# 🗁 Function Interfaces with Generic Parameters

Function interfaces can use generic type parameters to support different data types.

```typescript
interface Identity {
  <T>(value: T): T;
}
```

Example:

```typescript
const identity: Identity = <T>(value: T): T => {
  return value;
};

const name = identity("Alice");
const age = identity(25);
const active = identity(true);
```

TypeScript preserves the input type in the return value.

# 🗁 Generic Function Interfaces

You can declare the generic parameter at the interface level.

```typescript
interface Container<T> {
  (value: T): T[];
}
```

Example:

```typescript
const createStringArray: Container<string> = (value) => {
  return [value];
};

const createNumberArray: Container<number> = (value) => {
  return [value];
};

console.log(createStringArray("Hello"));
console.log(createNumberArray(100));
```

The type argument determines the parameter and return types.

# 🗁 Function Interfaces with Overloads

An interface can define multiple call signatures for a function.

```typescript
interface Formatter {
  (value: string): string;
  (value: number): string;
}
```

Implementation:

```typescript
const format: Formatter = (value: string | number): string => {
  return String(value);
};

console.log(format("Hello"));
console.log(format(100));
```

The function supports both string and number arguments.

# 🗁 Function Interfaces with Properties

An interface can describe a callable object that also contains properties.

```typescript
interface Counter {
  (initialValue: number): number;
  count: number;
  reset(): void;
}
```

This interface describes an object that:

* Can be called like a function.
* Contains a numeric `count` property.
* Provides a `reset()` method.

Example:

```typescript
const counter: Counter = Object.assign(
  (initialValue: number) => {
    counter.count += initialValue;
    return counter.count;
  },
  {
    count: 0,

    reset() {
      counter.count = 0;
    },
  },
);

console.log(counter(5));
console.log(counter(10));

counter.reset();

console.log(counter.count);
```

This pattern is known as a callable object.

# 🗁 Function Interfaces vs Function Type Aliases

TypeScript supports both interfaces and type aliases for describing function types.

Using an interface:

```typescript
interface Calculator {
  (a: number, b: number): number;
}
```

Using a type alias:

```typescript
type Calculator = (a: number, b: number) => number;
```

Both can describe the same function structure.

Example:

```typescript
const add: Calculator = (a, b) => a + b;
```

| Feature | Function Interface | Function Type Alias |
| --- | --- | --- |
| Function signature | Supported | Supported |
| Parameters | Supported | Supported |
| Return types | Supported | Supported |
| Generic functions | Supported | Supported |
| Callable object properties | Supported | Supported through intersections |
| Declaration merging | Supported | Not supported |
| Extending object contracts | Supported | Uses intersections |

Use whichever representation makes the function contract easier to understand.

# 🗁 Function Interfaces vs Method Signatures

Interfaces can describe functions in two different ways.

**Call signature:**

```typescript
interface Formatter {
  (value: string): string;
}
```

This describes an object that can be called directly.

```typescript
const formatter: Formatter = (value) => value.toUpperCase();

formatter("hello");
```

**Method signature:**

```typescript
interface Formatter {
  format(value: string): string;
}
```

This describes an object that contains a method named `format`.

```typescript
const formatter: Formatter = {
  format(value) {
    return value.toUpperCase();
  },
};

formatter.format("hello");
```

The main difference is how the function is accessed.

# 🗁 Function Interfaces with Callback Functions

Function interfaces are useful for defining callback contracts.

```typescript
interface DataCallback {
  (data: string): void;
}
```

Create a function that accepts a callback:

```typescript
function processData(
  data: string,
  callback: DataCallback,
): void {
  const processedData = data.toUpperCase();

  callback(processedData);
}
```

Usage:

```typescript
processData("hello typescript", (result) => {
  console.log(result);
});
```

Output:

```text
HELLO TYPESCRIPT
```

The callback must follow the declared function signature.

# 🗁 Function Interfaces with Async Functions

An interface can describe a function that returns a promise.

```typescript
interface UserFetcher {
  (id: number): Promise<string>;
}
```

Example:

```typescript
const fetchUser: UserFetcher = async (id) => {
  return `User ${id}`;
};
```

Usage:

```typescript
async function main(): Promise<void> {
  const user = await fetchUser(1);

  console.log(user);
}

main();
```

The return type must represent the promise returned by the asynchronous function.

# 🗁 Practical Example: Payment Processor

Function interfaces can standardize different payment implementations.

```typescript
interface PaymentProcessor {
  (amount: number, currency: string): Promise<boolean>;
}
```

Create a payment implementation:

```typescript
const processCardPayment: PaymentProcessor = async (
  amount,
  currency,
) => {
  console.log(`Processing ${amount} ${currency}`);

  return true;
};
```

Create another implementation:

```typescript
const processMobilePayment: PaymentProcessor = async (
  amount,
  currency,
) => {
  console.log(`Processing mobile payment: ${amount} ${currency}`);

  return true;
};
```

Use a common function:

```typescript
async function checkout(
  processor: PaymentProcessor,
  amount: number,
  currency: string,
): Promise<void> {
  const successful = await processor(amount, currency);

  if (successful) {
    console.log("Payment successful");
  } else {
    console.log("Payment failed");
  }
}
```

Usage:

```typescript
checkout(processCardPayment, 100, "USD");
checkout(processMobilePayment, 200, "USD");
```

Both payment implementations follow the same contract.

# 🗁 Practical Example: Event Handler

Function interfaces are also useful for event handling.

```typescript
interface EventHandler {
  (event: {
    type: string;
    timestamp: number;
  }): void;
}
```

Implementation:

```typescript
const handleEvent: EventHandler = (event) => {
  console.log(`Event: ${event.type}`);
  console.log(`Timestamp: ${event.timestamp}`);
};
```

Usage:

```typescript
handleEvent({
  type: "USER_LOGIN",
  timestamp: Date.now(),
});
```

# 🗁 Best Practices

* Use function interfaces when a function contract is shared across multiple implementations.
* Define explicit parameter and return types when they improve clarity.
* Use descriptive interface names that communicate the function's responsibility.
* Prefer generic function interfaces when the same behavior must support multiple data types.
* Use optional parameters only when callers are allowed to omit them.
* Use rest parameters when a function accepts a variable number of arguments.
* Use callback interfaces to standardize event and data handling.
* Use `Promise<T>` for asynchronous function return types.
* Avoid unnecessarily complex callable object interfaces.
* Prefer simple function type aliases when an interface provides no meaningful advantage.
* Keep function contracts focused and predictable.

# 🗁 Summary

* Function interfaces describe function structures using call signatures.
* They define parameters, parameter types, and return types.
* Function interfaces support optional and rest parameters.
* They can describe generic functions and overloaded functions.
* Interfaces can describe callable objects with additional properties and methods.
* Function interfaces can define callback contracts.
* They can describe asynchronous functions returning promises.
* Function interfaces and function type aliases provide similar capabilities.
* Function interfaces are useful for reusable function contracts and consistent application behavior.

# 🗁 Related Topics

* Function Types
* Function Parameters
* Return Types
* Optional Parameters
* Rest Parameters
* Function Overloads
* Arrow Functions
* Callback Functions
* Generic Functions
* Interfaces
* Type Aliases
* Async Functions
* Promise
