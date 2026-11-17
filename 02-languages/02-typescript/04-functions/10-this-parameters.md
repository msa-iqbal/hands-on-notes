# This Parameters

In TypeScript, the `this` keyword refers to the context in which a function is executed.

Unlike languages that always associate `this` with an object instance, JavaScript determines the value of `this` primarily from how a function is called.

TypeScript provides a special `this` parameter that allows developers to explicitly declare the expected type of `this` inside a function.

This feature helps catch incorrect function calls at compile time.

# 🗁 Understanding `this` in JavaScript

Consider the following example:

```typescript
const user = {
  name: "Alice",

  greet() {
    console.log(`Hello, ${this.name}!`);
  },
};

user.greet();
```

Output:

```text
Hello, Alice!
```

Here, `this` refers to the `user` object because the method is called through `user.greet()`.

However, extracting the method changes its invocation context:

```typescript
const greet = user.greet;

// The function is called without user as its receiver.
greet();
```

With strict-mode behavior, `this` is `undefined` in this regular function call. Accessing `this.name` therefore causes a runtime error.

# 🗁 What Is a TypeScript `this` Parameter?

A `this` parameter is a special parameter used to declare the expected type of `this` inside a function.

It is not a normal runtime parameter. TypeScript removes it during compilation.

Syntax:

```typescript
function functionName(this: SomeType, parameters): ReturnType {
  // Function body
}
```

Example:

```typescript
interface User {
  name: string;
}

function greet(this: User): void {
  console.log(`Hello, ${this.name}!`);
}

const user: User = {
  name: "Alice",
};

greet.call(user);
```

The `this: User` annotation tells TypeScript that the function expects `this` to be a `User` object.

# 🗁 Preventing Incorrect Function Calls

A major benefit of explicit `this` parameters is that TypeScript can detect calls with an invalid context.

```typescript
interface User {
  name: string;
}

function greet(this: User): void {
  console.log(`Hello, ${this.name}!`);
}

const user: User = {
  name: "Alice",
};

greet.call(user);

// Type error: the function requires a User context.
greet();
```

The incorrect call is rejected during type checking.

# 🗁 Typing `this` in Object Methods

You can use an explicit `this` parameter in object methods.

```typescript
interface Calculator {
  value: number;
}

function increment(this: Calculator, amount: number): void {
  this.value += amount;
}

const calculator: Calculator = {
  value: 10,
};

increment.call(calculator, 5);

console.log(calculator.value);
```

Output:

```text
15
```

The method expects a `Calculator` context and modifies its `value` property.

# 🗁 Typing `this` in Class Methods

Class methods usually infer their `this` type from the class instance.

```typescript
class Counter {
  count = 0;

  increment(): void {
    this.count++;
  }
}

const counter = new Counter();

counter.increment();

console.log(counter.count);
```

Output:

```text
1
```

You generally do not need to declare `this` explicitly in ordinary class methods because TypeScript already understands the instance type.

However, explicit `this` parameters are useful when defining standalone functions that operate on objects.

# 🗁 `this` Parameters in Callbacks

Callbacks can execute with a different `this` context depending on how they are invoked.

Consider this example:

```typescript
interface Handler {
  message: string;
}

function handleEvent(this: Handler): void {
  console.log(this.message);
}

const handler: Handler = {
  message: "Event handled",
};

handleEvent.call(handler);
```

Output:

```text
Event handled
```

The explicit `this` parameter documents the required callback context.

# 🗁 Typing `this` as `void`

Use `this: void` when a function should not depend on a calling context.

```typescript
function logMessage(this: void, message: string): void {
  console.log(message);
}

logMessage("Hello TypeScript");
```

This is particularly useful for callback APIs where the function must not rely on a particular `this` value.

For example:

```typescript
interface Button {
  label: string;
  onClick: (this: void, event: Event) => void;
}

const button: Button = {
  label: "Submit",

  onClick(event) {
    console.log(event.type);
  },
};
```

The callback contract specifies that the callback does not require a `Button` instance as its `this` context.

# 🗁 `this` Parameters and Arrow Functions

Arrow functions do not have their own `this`. They capture `this` from their surrounding lexical scope.

You cannot declare an explicit `this` parameter in an arrow function.

Incorrect:

```typescript
// Invalid syntax for an arrow function.
const greet = (this: User) => {
  console.log(this.name);
};
```

Use a regular function when an explicit `this` parameter is required:

```typescript
interface User {
  name: string;
}

function greet(this: User): void {
  console.log(this.name);
}
```

Use an arrow function when you want to preserve the surrounding `this`:

```typescript
class Counter {
  count = 0;

  incrementLater(): void {
    setTimeout(() => {
      this.count++;
      console.log(this.count);
    }, 1000);
  }
}

const counter = new Counter();

counter.incrementLater();
```

# 🗁 Using `this` with Function Types

You can include a `this` parameter in a function type.

```typescript
interface User {
  name: string;
}

type UserMethod = (this: User) => void;

const greet: UserMethod = function () {
  console.log(`Hello, ${this.name}!`);
};

const user: User = {
  name: "Alice",
};

greet.call(user);
```

The function type describes both the expected `this` context and the function's behavior.

# 🗁 Using `this` with Interfaces

Interfaces can describe methods that require a specific `this` context.

```typescript
interface Account {
  balance: number;

  deposit(this: Account, amount: number): void;
}

const account: Account = {
  balance: 100,

  deposit(amount) {
    this.balance += amount;
  },
};

account.deposit(50);

console.log(account.balance);
```

Output:

```text
150
```

This makes the expected method context explicit in the interface contract.

# 🗁 `this` Type in Classes

TypeScript also provides a polymorphic `this` type.

The `this` type represents the actual type of the current instance, including derived classes.

```typescript
class Builder {
  setName(name: string): this {
    console.log(name);
    return this;
  }
}

class UserBuilder extends Builder {
  setAge(age: number): this {
    console.log(age);
    return this;
  }
}

const builder = new UserBuilder();

builder
  .setName("Alice")
  .setAge(25);
```

Using `this` as a return type allows fluent method chaining while preserving the derived class type.

# 🗁 Explicit `this` Parameter vs Polymorphic `this` Type

These two features serve different purposes.

| Feature | Explicit `this` Parameter | Polymorphic `this` Type |
| --- | --- | --- |
| Purpose | Declares the expected function context | Represents the current instance type |
| Common usage | Standalone functions and callbacks | Class methods and fluent APIs |
| Runtime parameter | No | No |
| Example | `function f(this: User)` | `setName(): this` |

# 🗁 Common Mistakes

**Calling a function without its required context**

```typescript
interface User {
  name: string;
}

function greet(this: User): void {
  console.log(this.name);
}

const user: User = {
  name: "Alice",
};

greet.call(user);

// Type error:
greet();
```

Use the correct object context when invoking the function.

**Assuming extracted methods preserve `this`**

```typescript
const user = {
  name: "Alice",

  greet() {
    console.log(this.name);
  },
};

const greet = user.greet;

// The original object context is not preserved.
greet();
```

Use an explicit binding when necessary:

```typescript
const greet = user.greet.bind(user);

greet();
```

**Trying to declare `this` in an arrow function**

Arrow functions inherit `this` lexically and cannot declare their own `this` parameter.

Use a regular function when a specific `this` contract is required.

**Confusing the `this` parameter with a normal parameter**

```typescript
interface User {
  name: string;
}

function greet(this: User, message: string): void {
  console.log(`${message}, ${this.name}`);
}
```

The `this` parameter is not supplied as an ordinary argument.

The `message` parameter is the first actual argument.

# 🗁 Best Practices

* Declare explicit `this` types for standalone functions that depend on an object context.
* Use `this: void` when callbacks should not depend on a calling context.
* Prefer arrow functions when lexical `this` is desired.
* Use regular functions when dynamic `this` behavior is required.
* Use `.bind()` when a function must retain a specific object context.
* Use polymorphic `this` return types for fluent class APIs.
* Avoid unnecessary explicit `this` annotations in ordinary class methods.

# 🗁 Summary

* JavaScript determines regular function `this` primarily from the invocation context.
* TypeScript supports explicit `this` parameters for declaring expected function contexts.
* Explicit `this` parameters exist only during type checking and are removed from generated JavaScript.
* The `this: void` annotation indicates that a callback does not require a specific `this` context.
* Arrow functions capture `this` lexically and cannot declare their own `this` parameter.
* The polymorphic `this` type is useful for inheritance and fluent APIs.
* `.call()`, `.apply()`, and `.bind()` can control the invocation context of regular functions.

# 🗁 Related Topics

* Function Types
* Function Parameters
* Arrow Functions
* Callback Functions
* Classes
* Interfaces
* Inheritance
* Polymorphic `this` Type
