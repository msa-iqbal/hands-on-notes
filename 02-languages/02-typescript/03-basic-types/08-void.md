# TypeScript Void Type

The `void` type represents the absence of a meaningful return value.

It is commonly used as the return type of functions that perform an action but do not return a result.

Examples include:

* Logging messages.
* Updating application state.
* Modifying the DOM.
* Sending notifications.
* Executing callback functions.
* Performing operations with side effects.

Unlike `never`, a function with a `void` return type can finish executing normally.

# 🗁 Understanding Void

The `void` type indicates that a function does not return a meaningful value.

```typescript
function sayHello(): void {
    console.log("Hello TypeScript");
}

sayHello();
```

The function executes successfully but does not return a value.

# 🗁 Declaring Void Variables

You can declare a variable with the `void` type.

```typescript
let result: void;

result = undefined;
```

With `strictNullChecks` enabled, `undefined` is the value generally assignable to `void`.

However, declaring ordinary variables as `void` is uncommon. The type is primarily intended for function return types.

# 🗁 Void as a Function Return Type

The most common use of `void` is specifying that a function does not return a meaningful value.

```typescript
function displayMessage(message: string): void {
    console.log(message);
}

displayMessage("Application started");
```

The return type tells TypeScript that callers should not expect a useful result from this function.

Another example:

```typescript
function calculateTotal(): void {
    const price = 100;
    const quantity = 5;

    const total = price * quantity;

    console.log(total);
}

calculateTotal();
```

The function calculates and displays a value but does not return it.

# 🗁 Void and Return Statements

A function with a `void` return type can use an empty `return` statement to exit early.

```typescript
function greet(name: string): void {
    if (name.length === 0) {
        console.log("Name is required");
        return;
    }

    console.log(`Hello, ${name}`);
}

greet("John");
greet("");
```

You cannot return a meaningful value from a function explicitly declared as `void`.

```typescript
function getNumber(): void {
    // TypeScript error:
    // return 100;
}
```

An empty return is valid:

```typescript
function stopProcess(): void {
    console.log("Stopping process");

    return;
}
```

# 🗁 Void and Undefined

The `void` and `undefined` types are related but have different meanings.

* `undefined` is a JavaScript value and a TypeScript type.
* `void` describes the absence of a meaningful function return value.

Example:

```typescript
function getValue(): undefined {
    return undefined;
}

function logValue(): void {
    console.log("Hello");
}
```

The first function explicitly returns `undefined`.

The second function is declared to have no meaningful return value.

# 🗁 Void vs Never

Both types are commonly used as function return types, but they represent different execution behaviors.

* `void` means the function completes normally without returning a meaningful value.
* `never` means the function never completes normally.

**Using void:**

```typescript
function logMessage(): void {
    console.log("Application started");
}

logMessage();
```

**Using never:**

```typescript
function throwError(): never {
    throw new Error("Application failed");
}
```

Comparison:

| Feature | `void` | `never` |
| --- | --- | --- |
| Meaning | No meaningful return value | No normal completion |
| Function completes normally | Yes | No |
| Empty return allowed | Yes | No |
| Can throw an error | Yes | Yes |
| Can contain an infinite loop | Yes | Yes |
| Common usage | Logging and callbacks | Errors and impossible states |

# 🗁 Void in Arrow Functions

Arrow functions can also use `void` as their return type.

```typescript
const greet = (name: string): void => {
    console.log(`Hello, ${name}`);
};

greet("John");
```

Another example:

```typescript
const showMessage = (): void => {
    console.log("Welcome");
};

showMessage();
```

# 🗁 Void in Callback Functions

The `void` type is commonly used when defining callback function signatures.

```typescript
type Callback = () => void;

function executeCallback(callback: Callback): void {
    callback();
}

executeCallback(() => {
    console.log("Callback executed");
});
```

The callback is expected to perform an action rather than provide a meaningful result.

You can also define callbacks with parameters.

```typescript
type MessageCallback = (message: string) => void;

function notifyUser(callback: MessageCallback): void {
    callback("Welcome to the application");
}

notifyUser((message) => {
    console.log(message);
});
```

# 🗁 Void and Function Type Compatibility

TypeScript treats `void` specially in function type compatibility.

A function returning a value can be assigned to a function type whose return type is `void`.

```typescript
type VoidFunction = () => void;

const getNumber = (): number => {
    return 100;
};

const callback: VoidFunction = getNumber;
```

This is valid because the `VoidFunction` contract tells callers that they should ignore the return value.

```typescript
const result = callback();

console.log(result); // TypeScript type: void
```

Although the underlying function returns `100` at runtime, TypeScript treats the result of calling `callback` as `void`.

This behavior is particularly useful for callback APIs.

# 🗁 Void in Array Methods

Several JavaScript array methods use callbacks whose return values are ignored.

For example, `forEach()` accepts a callback that returns `void`.

```typescript
const numbers: number[] = [10, 20, 30];

numbers.forEach((number): void => {
    console.log(number);
});
```

You can also omit the explicit return type because TypeScript can infer it.

```typescript
numbers.forEach((number) => {
    console.log(number);
});
```

The `forEach()` method performs an operation for each element and does not produce a transformed array.

# 🗁 Void in Event Handlers

Event handlers commonly use `void` because their purpose is to respond to an event rather than return a result.

```typescript
function handleClick(): void {
    console.log("Button clicked");
}
```

With DOM event handling:

```typescript
const button = document.querySelector("button");

button?.addEventListener("click", (): void => {
    console.log("Button clicked");
});
```

The event handler performs an action without returning a meaningful value.

# 🗁 Void in Class Methods

Class methods can use `void` when they perform actions without returning results.

```typescript
class User {
    name: string;

    constructor(name: string) {
        this.name = name;
    }

    displayName(): void {
        console.log(this.name);
    }

    updateName(name: string): void {
        this.name = name;
    }
}

const user = new User("John");

user.displayName();
user.updateName("David");
user.displayName();
```

Both methods modify or display data without returning a meaningful value.

# 🗁 Void in Async Functions

An asynchronous function can use `Promise<void>` when it completes without providing a meaningful result.

```typescript
async function saveUser(): Promise<void> {
    console.log("Saving user...");

    await new Promise((resolve) => {
        setTimeout(resolve, 1000);
    });

    console.log("User saved");
}
```

The function returns a promise that resolves without a meaningful value.

Usage:

```typescript
await saveUser();
```

Another example using an API request:

```typescript
async function deleteUser(userId: number): Promise<void> {
    const response = await fetch(
        `/api/users/${userId}`,
        {
            method: "DELETE"
        }
    );

    if (!response.ok) {
        throw new Error("Failed to delete user");
    }
}
```

The function completes after the deletion request succeeds but does not return a result.

# 🗁 Void and Side Effects

Functions with a `void` return type often perform side effects.

A side effect is an operation that changes something outside the function's local execution or interacts with the external environment.

Examples:

```typescript
function updateCounter(): void {
    counter++;
}

function saveSettings(): void {
    localStorage.setItem("theme", "dark");
}

function sendNotification(): void {
    console.log("Notification sent");
}
```

These functions perform actions instead of returning computed values.

# 🗁 Best Practices

* Use `void` for functions that do not return meaningful results.
* Use `Promise<void>` for asynchronous functions that resolve without a meaningful result.
* Use `void` in callback signatures when callback results are intentionally ignored.
* Prefer explicit return types for public APIs and reusable functions.
* Do not confuse `void` with `undefined` or `never`.
* Avoid using `void` for ordinary variables unless there is a specific type-level reason.
* Return a specific type when callers need to consume the result of a function.

# 🗁 Summary

The `void` type represents the absence of a meaningful return value.

Important concepts include:

* Understanding the `void` type.
* Using `void` in function declarations.
* Empty return statements.
* Difference between `void` and `undefined`.
* Difference between `void` and `never`.
* Arrow functions and callbacks.
* Function type compatibility.
* Array methods and event handlers.
* Class methods.
* Asynchronous functions with `Promise<void>`.
* Functions that perform side effects.

The key principle is that `void` describes a function that does not provide a meaningful result, even though it can execute and complete normally.
