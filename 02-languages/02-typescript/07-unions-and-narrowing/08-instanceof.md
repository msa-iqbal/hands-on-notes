# Instanceof

The `instanceof` operator is a JavaScript operator used to determine whether an object is an instance of a particular constructor or class.

In TypeScript, `instanceof` also works as a **type-narrowing mechanism**.

It helps TypeScript identify the specific class type of an object when working with union types.

The `instanceof` operator is particularly useful when:

* Working with class-based object types.
* Narrowing union types containing class instances.
* Checking custom error instances.
* Handling different object implementations.
* Implementing type-safe class hierarchies.
* Working with built-in JavaScript objects such as `Date`, `Error`, and `RegExp`.

# 🗁 Basic Syntax

```typescript
object instanceof ClassName
```

The expression returns a boolean:

* `true` if the object's prototype chain contains `ClassName.prototype`.
* `false` otherwise.

Example:

```typescript
class User {
  constructor(public name: string) {}
}

const user = new User("Alice");

console.log(user instanceof User); // true
console.log(user instanceof Object); // true
```

# 🗁 Basic Type Narrowing

Consider two classes:

```typescript
class Admin {
  constructor(
    public name: string,
    public permissions: string[],
  ) {}
}

class Customer {
  constructor(
    public name: string,
    public address: string,
  ) {}
}

type User = Admin | Customer;
```

Use `instanceof` to identify the class:

```typescript
function displayUser(user: User): void {
  if (user instanceof Admin) {
    console.log("Admin:", user.name);
    console.log("Permissions:", user.permissions);
  } else {
    console.log("Customer:", user.name);
    console.log("Address:", user.address);
  }
}
```

TypeScript narrows the union automatically:

* Inside the `if` block, `user` is `Admin`.
* Inside the `else` block, `user` is `Customer`.

# 🗁 Using instanceof with if and else

The `instanceof` operator works naturally with conditional statements.

```typescript
class Car {
  constructor(
    public brand: string,
    public numberOfDoors: number,
  ) {}
}

class Motorcycle {
  constructor(
    public brand: string,
    public hasSidecar: boolean,
  ) {}
}

type Vehicle = Car | Motorcycle;
```

Example:

```typescript
function describeVehicle(vehicle: Vehicle): void {
  if (vehicle instanceof Car) {
    console.log(`${vehicle.brand} has ${vehicle.numberOfDoors} doors`);
  } else {
    console.log(`${vehicle.brand} has sidecar: ${vehicle.hasSidecar}`);
  }
}
```

The check determines which class-specific properties are available.

# 🗁 Using instanceof with switch-like Logic

JavaScript does not support `switch` statements directly on `instanceof` expressions for type narrowing in the same convenient way as discriminant properties.

For multiple classes, use `if` and `else if`.

```typescript
class EmailNotification {
  constructor(
    public recipient: string,
    public subject: string,
  ) {}
}

class SmsNotification {
  constructor(
    public phoneNumber: string,
    public message: string,
  ) {}
}

class PushNotification {
  constructor(
    public deviceToken: string,
    public title: string,
  ) {}
}

type Notification =
  | EmailNotification
  | SmsNotification
  | PushNotification;
```

Handle notifications:

```typescript
function sendNotification(notification: Notification): void {
  if (notification instanceof EmailNotification) {
    console.log("Sending email to:", notification.recipient);
  } else if (notification instanceof SmsNotification) {
    console.log("Sending SMS to:", notification.phoneNumber);
  } else if (notification instanceof PushNotification) {
    console.log("Sending push notification:", notification.deviceToken);
  }
}
```

Each condition narrows the value to its corresponding class.

# 🗁 Using instanceof with Inheritance

The `instanceof` operator recognizes inheritance through the prototype chain.

```typescript
class Animal {
  constructor(public name: string) {}
}

class Dog extends Animal {
  bark(): void {
    console.log("Woof!");
  }
}

class Cat extends Animal {
  meow(): void {
    console.log("Meow!");
  }
}
```

Create instances:

```typescript
const dog = new Dog("Buddy");
const cat = new Cat("Luna");
```

Check instances:

```typescript
console.log(dog instanceof Dog); // true
console.log(dog instanceof Animal); // true

console.log(cat instanceof Cat); // true
console.log(cat instanceof Animal); // true
```

A subclass instance is also an instance of its parent class.

# 🗁 Narrowing Inherited Class Unions

Consider a union of subclasses:

```typescript
type Pet = Dog | Cat;
```

Use `instanceof`:

```typescript
function makeSound(pet: Pet): void {
  if (pet instanceof Dog) {
    pet.bark();
  } else if (pet instanceof Cat) {
    pet.meow();
  }
}
```

TypeScript identifies the specific subclass and allows access to its methods.

# 🗁 Using instanceof with Abstract Classes

Abstract classes cannot be instantiated directly, but their subclasses can be checked against the abstract parent class.

```typescript
abstract class Payment {
  abstract process(): void;
}

class CreditCardPayment extends Payment {
  process(): void {
    console.log("Processing credit card payment");
  }
}

class BankTransferPayment extends Payment {
  process(): void {
    console.log("Processing bank transfer");
  }
}
```

Create an instance:

```typescript
const payment = new CreditCardPayment();

console.log(payment instanceof Payment); // true
console.log(payment instanceof CreditCardPayment); // true
```

The abstract class participates in the prototype chain even though it cannot be instantiated directly.

# 🗁 Using instanceof with Built-in Objects

JavaScript provides several built-in constructors that can be used with `instanceof`.

**Date**

```typescript
const value = new Date();

console.log(value instanceof Date); // true
```

**Array**

```typescript
const items = [10, 20, 30];

console.log(items instanceof Array); // true
```

**Error**

```typescript
const error = new Error("Something went wrong");

console.log(error instanceof Error); // true
```

**RegExp**

```typescript
const pattern = /hello/i;

console.log(pattern instanceof RegExp); // true
```

**Map**

```typescript
const users = new Map<number, string>();

console.log(users instanceof Map); // true
```

# 🗁 Using instanceof with Error Handling

JavaScript applications frequently work with different error classes.

```typescript
class ValidationError extends Error {
  constructor(message: string) {
    super(message);
    this.name = "ValidationError";
  }
}

class DatabaseError extends Error {
  constructor(message: string) {
    super(message);
    this.name = "DatabaseError";
  }
}
```

Handle errors:

```typescript
function handleError(error: unknown): void {
  if (error instanceof ValidationError) {
    console.log("Validation error:", error.message);
  } else if (error instanceof DatabaseError) {
    console.log("Database error:", error.message);
  } else if (error instanceof Error) {
    console.log("General error:", error.message);
  } else {
    console.log("Unknown error:", error);
  }
}
```

This is useful because JavaScript allows any value to be thrown, not just instances of `Error`.

# 🗁 Using instanceof with Unknown

Values typed as `unknown` must be narrowed before their properties or methods can be accessed.

The `instanceof` operator can perform this narrowing.

```typescript
function processValue(value: unknown): void {
  if (value instanceof Date) {
    console.log(value.toISOString());
  } else if (value instanceof Error) {
    console.log(value.message);
  } else if (value instanceof Array) {
    console.log(value.length);
  } else {
    console.log("Unsupported value");
  }
}
```

TypeScript understands the type inside each condition.

# 🗁 Creating a Custom Class Type Guard

You can use `instanceof` inside a reusable type guard.

```typescript
class Admin {
  constructor(
    public name: string,
    public permissions: string[],
  ) {}
}

function isAdmin(value: unknown): value is Admin {
  return value instanceof Admin;
}
```

Usage:

```typescript
function displayUser(value: unknown): void {
  if (isAdmin(value)) {
    console.log(value.name);
    console.log(value.permissions);
  }
}
```

The function checks the runtime class and communicates the narrowed type through its type predicate.

# 🗁 instanceof vs typeof

Both operators can help narrow types, but they solve different problems.

**Using typeof**

```typescript
function displayValue(value: string | number): void {
  if (typeof value === "string") {
    console.log(value.toUpperCase());
  } else {
    console.log(value.toFixed(2));
  }
}
```

**Using instanceof**

```typescript
class User {
  constructor(public name: string) {}
}

function displayUser(value: User | Date): void {
  if (value instanceof User) {
    console.log(value.name);
  } else {
    console.log(value.toISOString());
  }
}
```

Comparison:

| Feature | typeof | instanceof |
| --- | --- | --- |
| Checks primitive types | Yes | No |
| Checks class instances | No | Yes |
| Returns a boolean | Yes | Yes |
| Uses prototype chain | No | Yes |
| Useful for class unions | Limited | Yes |
| Useful for primitive unions | Yes | No |

Use `typeof` for primitive values such as strings, numbers, and booleans.

Use `instanceof` for class instances and constructor-based runtime checks.

# 🗁 instanceof vs in Operator

Both operators can narrow object unions, but they use different mechanisms.

**Using instanceof:**

```typescript
class Admin {
  constructor(public permissions: string[]) {}
}

class Customer {
  constructor(public address: string) {}
}

type User = Admin | Customer;

function displayUser(user: User): void {
  if (user instanceof Admin) {
    console.log(user.permissions);
  } else {
    console.log(user.address);
  }
}
```

**Using in:**

```typescript
type Admin = {
  permissions: string[];
};

type Customer = {
  address: string;
};

type User = Admin | Customer;

function displayUser(user: User): void {
  if ("permissions" in user) {
    console.log(user.permissions);
  } else {
    console.log(user.address);
  }
}
```

Comparison:

| Feature | instanceof | in |
| --- | --- | --- |
| Checks class instances | Yes | No |
| Checks property existence | No | Yes |
| Requires constructors | Yes | No |
| Works with plain objects | Not reliably for structural types | Yes |
| Uses prototype chain | Yes | Checks property existence through prototype chain |
| Common use | Class-based narrowing | Object-property narrowing |

# 🗁 instanceof vs Discriminated Unions

Discriminated unions identify variants using a shared literal property.

`instanceof` identifies variants using runtime constructor relationships.

**Using instanceof:**

```typescript
class Admin {
  readonly role = "admin";

  constructor(public permissions: string[]) {}
}

class Customer {
  readonly role = "customer";

  constructor(public address: string) {}
}

type User = Admin | Customer;
```

**Using a discriminated union:**

```typescript
type Admin = {
  role: "admin";
  permissions: string[];
};

type Customer = {
  role: "customer";
  address: string;
};

type User = Admin | Customer;
```

Comparison:

| Feature | instanceof | Discriminated Union |
| --- | --- | --- |
| Requires classes | Yes, for normal class-based narrowing | No |
| Runtime check | Constructor relationship | Literal property value |
| Works with plain objects | Not reliably | Yes |
| Serialization-friendly | No constructor information is preserved automatically | Yes |
| Supports exhaustive checking | Possible with additional checks | Straightforward with `never` |

For API data, JSON objects, and serialized data, discriminated unions are generally more suitable.

For class-based application models, `instanceof` can be a natural choice.

# 🗁 Important Limitations

**Structural typing does not guarantee instanceof compatibility**

TypeScript uses structural typing for most object types.

Consider:

```typescript
type User = {
  name: string;
};

const user: User = {
  name: "Alice",
};
```

This object satisfies the `User` type, but it was not created using a `User` constructor.

```typescript
class UserClass {
  constructor(public name: string) {}
}

console.log(user instanceof UserClass); // false
```

A matching object structure does not make an object an instance of a class.

**Plain objects are not class instances**

```typescript
const user = {
  name: "Alice",
};

console.log(user instanceof Object); // true
console.log(user instanceof Date); // false
```

**Cross-realm objects can produce unexpected results**

Objects created in another JavaScript realm, such as a different browser window or iframe, may have different constructor objects.

For example, an array created in another window may fail a check against the current window's `Array` constructor.

```typescript
const items = [1, 2, 3];

console.log(items instanceof Array); // true
```

For cross-realm array checks, prefer:

```typescript
console.log(Array.isArray(items)); // true
```

**Interfaces and type aliases do not exist at runtime**

TypeScript removes interfaces and type aliases during compilation.

This is invalid:

```typescript
interface User {
  name: string;
}

// Cannot use an interface as a runtime constructor
// user instanceof User
```

`instanceof` requires a runtime constructor value, such as a class.

**The prototype chain determines the result**

An object can pass an `instanceof` check even if it was not created by directly calling the class constructor.

```typescript
class User {}

const user = Object.create(User.prototype);

console.log(user instanceof User); // true
```

The check evaluates the prototype chain, not the object's construction history.

# 🗁 Customizing instanceof with Symbol.hasInstance

JavaScript allows classes and constructor functions to customize `instanceof` behavior through `Symbol.hasInstance`.

Example:

```typescript
class PositiveNumber {
  static [Symbol.hasInstance](value: unknown): boolean {
    return typeof value === "number" && value > 0;
  }
}
```

Usage:

```typescript
console.log(10 instanceof PositiveNumber); // true
console.log(-5 instanceof PositiveNumber); // false
console.log("10" instanceof PositiveNumber); // false
```

Here, `instanceof` does not use the normal prototype-chain check. It delegates the check to the custom `Symbol.hasInstance` method.

This feature is useful for specialized runtime classification, although ordinary class-based checks are more common.

# 🗁 Best Practices

* Use `instanceof` to narrow class-based union types.
* Use `typeof` for primitive values.
* Use `in` when checking object properties.
* Use discriminated unions for plain objects and API responses.
* Use `Array.isArray()` for reliable array checks.
* Remember that interfaces and type aliases are erased at runtime.
* Be aware of prototype-chain behavior.
* Use custom type guards when runtime checks need to be reused.
* Avoid assuming that structural compatibility means an object is a class instance.
* Use custom `Symbol.hasInstance` behavior only when it provides a clear benefit.

# 🗁 Summary

* `instanceof` checks whether an object's prototype chain contains a constructor's prototype.
* It returns a boolean.
* TypeScript uses it to narrow class-based union types.
* It works with custom classes and built-in constructors.
* It supports inheritance and abstract class hierarchies.
* It is useful for handling custom errors and unknown values.
* It does not reliably identify plain objects based only on their structure.
* Interfaces and type aliases cannot be used directly with `instanceof`.
* `Symbol.hasInstance` allows custom runtime behavior.
* Use `instanceof` when constructor-based runtime identification is appropriate.

# 🗁 Related Topics

* Type Narrowing
* Type Guards
* Type Predicates
* Union Types
* Discriminated Unions
* In Operator
* Typeof Operator
* Classes
* Inheritance
* Prototype Chain
* Symbol.hasInstance
* Unknown Type
