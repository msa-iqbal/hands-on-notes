# Constructors

A constructor is a special method in a TypeScript class that runs automatically when a new instance is created.

Constructors are primarily used to initialize object properties and establish the initial state of an instance.

TypeScript constructors support:

* Parameter initialization.
* Default parameters.
* Optional parameters.
* Access modifiers.
* Parameter properties.
* Constructor overload signatures.
* Inheritance initialization through `super()`.

# 🗁 What Is a Constructor?

A constructor is declared using the `constructor` keyword.

```typescript
class User {
  name: string;
  age: number;

  constructor(name: string, age: number) {
    this.name = name;
    this.age = age;
  }
}
```

Create an instance:

```typescript
const user = new User("Alice", 25);

console.log(user.name);
console.log(user.age);
```

Output:

```text
Alice
25
```

When `new User("Alice", 25)` executes, TypeScript creates an object and invokes the constructor to initialize it.

# 🗁 Constructor Syntax

The general constructor syntax is:

```typescript
class ClassName {
  property: string;

  constructor(parameter: string) {
    this.property = parameter;
  }
}
```

Important characteristics:

* A constructor uses the `constructor` keyword.
* It does not have an explicit return type annotation.
* It cannot be declared as `async`.
* A class can have only one constructor implementation.
* Constructors are invoked automatically when using `new`.
* Constructors initialize instance state.

# 🗁 Constructor Without Parameters

A constructor does not need to accept parameters.

```typescript
class User {
  name: string;
  age: number;

  constructor() {
    this.name = "Guest";
    this.age = 18;
  }
}
```

Usage:

```typescript
const user = new User();

console.log(user.name);
console.log(user.age);
```

Output:

```text
Guest
18
```

This is useful when every instance should begin with the same default values.

# 🗁 Constructor with Parameters

Constructors can accept parameters to initialize properties with different values.

```typescript
class Product {
  id: number;
  title: string;
  price: number;

  constructor(id: number, title: string, price: number) {
    this.id = id;
    this.title = title;
    this.price = price;
  }
}
```

Usage:

```typescript
const product1 = new Product(101, "Keyboard", 75);
const product2 = new Product(102, "Mouse", 25);

console.log(product1.title);
console.log(product2.title);
```

Each instance receives its own property values.

# 🗁 Constructor with Default Parameters

A constructor parameter can have a default value.

```typescript
class User {
  name: string;
  role: string;

  constructor(name: string, role: string = "user") {
    this.name = name;
    this.role = role;
  }
}
```

Usage:

```typescript
const user1 = new User("Alice");
const user2 = new User("Bob", "admin");

console.log(user1.role);
console.log(user2.role);
```

Output:

```text
user
admin
```

The default value is used when the argument is omitted or explicitly passed as `undefined`.

# 🗁 Constructor with Optional Parameters

Optional parameters allow callers to omit certain arguments.

```typescript
class User {
  name: string;
  email?: string;

  constructor(name: string, email?: string) {
    this.name = name;
    this.email = email;
  }
}
```

Usage:

```typescript
const user1 = new User("Alice");
const user2 = new User("Bob", "bob@example.com");

console.log(user1.email);
console.log(user2.email);
```

Output:

```text
undefined
bob@example.com
```

Optional parameters must appear after required parameters.

# 🗁 Parameter Properties

Parameter properties provide a concise way to declare and initialize class properties directly through constructor parameters.

Instead of writing:

```typescript
class User {
  readonly id: number;
  name: string;
  age: number;

  constructor(id: number, name: string, age: number) {
    this.id = id;
    this.name = name;
    this.age = age;
  }
}
```

You can write:

```typescript
class User {
  constructor(
    public readonly id: number,
    public name: string,
    public age: number,
  ) {}
}
```

TypeScript automatically:

* Declares the corresponding class properties.
* Assigns constructor arguments to those properties.
* Applies the specified access modifiers.
* Applies `readonly` when specified.

Usage:

```typescript
const user = new User(101, "Alice", 25);

console.log(user.id);
console.log(user.name);
console.log(user.age);
```

Parameter properties support:

* `public`
* `private`
* `protected`
* `readonly`

They are available only in constructor parameter declarations.

# 🗁 Constructor with Access Modifiers

Access modifiers can control who is allowed to create instances of a class.

**Public constructor**

A public constructor allows normal instantiation.

```typescript
class User {
  public constructor(public name: string) {}
}

const user = new User("Alice");
```

**Protected constructor**

A protected constructor prevents direct instantiation outside the class hierarchy.

```typescript
class Database {
  protected constructor() {}
}

class MySQLDatabase extends Database {
  constructor() {
    super();
  }
}

const database = new MySQLDatabase();

// Error: Constructor is protected
// const anotherDatabase = new Database();
```

**Private constructor**

A private constructor prevents external instantiation and subclassing.

```typescript
class Singleton {
  private constructor() {}

  static create(): Singleton {
    return new Singleton();
  }
}

const instance = Singleton.create();

// Error: Constructor is private
// const anotherInstance = new Singleton();
```

Private constructors are useful for factory methods and controlled instance creation.

# 🗁 Constructor Overloads

TypeScript supports constructor overload signatures.

They allow a class to accept different parameter combinations while maintaining one implementation.

```typescript
class User {
  name: string;
  age: number;

  constructor(name: string);
  constructor(name: string, age: number);
  constructor(name: string, age?: number) {
    this.name = name;
    this.age = age ?? 0;
  }
}
```

Usage:

```typescript
const user1 = new User("Alice");
const user2 = new User("Bob", 30);

console.log(user1.age);
console.log(user2.age);
```

Output:

```text
0
30
```

The overload signatures describe the supported calling patterns.

The implementation signature handles all supported cases.

The implementation signature itself is not exposed as an additional overload.

# 🗁 Constructor with Union Types

A constructor can accept union types to support multiple input types.

```typescript
class User {
  id: number | string;

  constructor(id: number | string) {
    this.id = id;
  }
}
```

Usage:

```typescript
const user1 = new User(101);
const user2 = new User("USR-101");

console.log(user1.id);
console.log(user2.id);
```

Union types are useful when a property can legitimately contain different types of values.

# 🗁 Constructor with Object Parameters

Instead of accepting many individual arguments, a constructor can accept an object.

```typescript
type UserOptions = {
  name: string;
  age: number;
  email?: string;
};

class User {
  name: string;
  age: number;
  email?: string;

  constructor(options: UserOptions) {
    this.name = options.name;
    this.age = options.age;
    this.email = options.email;
  }
}
```

Usage:

```typescript
const user = new User({
  name: "Alice",
  age: 25,
  email: "alice@example.com",
});
```

Object parameters improve readability when a constructor has many options.

They also make it easier to add optional configuration fields without changing the order of positional arguments.

# 🗁 Constructor and Property Initialization

TypeScript checks whether class properties are initialized when `strictPropertyInitialization` is enabled.

Consider:

```typescript
class User {
  name: string;
  age: number;

  constructor() {
    this.name = "Guest";
  }
}
```

The `age` property is not initialized, so TypeScript reports an error.

Initialize all required properties:

```typescript
class User {
  name: string;
  age: number;

  constructor() {
    this.name = "Guest";
    this.age = 18;
  }
}
```

Alternatively, provide property initializers:

```typescript
class User {
  name: string = "Guest";
  age: number = 18;

  constructor() {}
}
```

A definite assignment assertion can also be used when initialization occurs through a mechanism TypeScript cannot verify.

```typescript
class User {
  name!: string;
}
```

Use this approach only when the application guarantees initialization.

# 🗁 Constructor and Inheritance

When a subclass defines a constructor, it must call `super()` before accessing `this`.

The `super()` call executes the parent class constructor.

```typescript
class Animal {
  name: string;

  constructor(name: string) {
    this.name = name;
  }
}

class Dog extends Animal {
  breed: string;

  constructor(name: string, breed: string) {
    super(name);

    this.breed = breed;
  }
}
```

Usage:

```typescript
const dog = new Dog("Buddy", "German Shepherd");

console.log(dog.name);
console.log(dog.breed);
```

Output:

```text
Buddy
German Shepherd
```

Important rules:

* A derived class constructor must call `super()` before accessing `this`.
* Parent constructor parameters must be passed appropriately.
* If a derived class has no constructor, it can inherit the parent's constructor behavior.
* A derived constructor can perform additional initialization after calling `super()`.

# 🗁 Constructor and Static Members

Constructors initialize instance members, while static members belong to the class itself.

```typescript
class User {
  static count = 0;

  constructor(public name: string) {
    User.count++;
  }
}
```

Usage:

```typescript
const user1 = new User("Alice");
const user2 = new User("Bob");

console.log(User.count);
```

Output:

```text
2
```

Static members are accessed through the class name rather than an instance.

# 🗁 Constructor Return Behavior

A constructor does not declare a return type like an ordinary method.

```typescript
class User {
  constructor(public name: string) {}
}
```

Do not write:

```typescript
class User {
  // Invalid constructor return type annotation
  // constructor(): void {}
}
```

In JavaScript, constructors invoked with `new` normally produce the newly created instance.

JavaScript has special object-return behavior for constructors, but explicit primitive returns do not replace the instance. TypeScript class constructors are designed around initializing and returning the instance created by `new`.

# 🗁 Constructor vs Method

A constructor and an ordinary method have different purposes.

| Feature | Constructor | Method |
| --- | --- | --- |
| Name | `constructor` | Any valid method name |
| Invocation | Automatically through `new` | Explicitly called |
| Purpose | Initialize an instance | Perform an operation |
| Return type annotation | Not allowed | Supported |
| Number of implementations | One per class | Multiple methods allowed |
| Can be inherited | Constructor behavior can be inherited | Yes |

Example:

```typescript
class User {
  constructor(public name: string) {}

  greet(): string {
    return `Hello, ${this.name}`;
  }
}

const user = new User("Alice");

console.log(user.greet());
```

# 🗁 Practical Example: E-commerce Order

```typescript
type OrderItem = {
  productId: number;
  quantity: number;
  price: number;
};

class Order {
  readonly orderId: string;
  readonly createdAt: Date;

  private items: OrderItem[];

  constructor(
    orderId: string,
    items: OrderItem[],
    public customerName: string,
  ) {
    this.orderId = orderId;
    this.items = [...items];
    this.createdAt = new Date();
  }

  getTotal(): number {
    return this.items.reduce(
      (total, item) => total + item.price * item.quantity,
      0,
    );
  }

  getItemCount(): number {
    return this.items.reduce(
      (total, item) => total + item.quantity,
      0,
    );
  }
}
```

Usage:

```typescript
const order = new Order(
  "ORD-1001",
  [
    {
      productId: 101,
      quantity: 2,
      price: 75,
    },
    {
      productId: 102,
      quantity: 1,
      price: 25,
    },
  ],
  "Alice",
);

console.log(order.orderId);
console.log(order.customerName);
console.log(order.getTotal());
console.log(order.getItemCount());
```

Output:

```text
ORD-1001
Alice
175
3
```

This example demonstrates:

* Constructor parameter initialization.
* Readonly properties.
* A private property.
* A public parameter property.
* Instance methods.
* Initialization of internal state.

# 🗁 Common Mistakes

**Declaring multiple constructor implementations**

A class can have only one constructor implementation. Use overload signatures when multiple calling patterns are needed.

**Forgetting to initialize required properties**

With strict property initialization enabled, every required property must be initialized or handled appropriately.

**Using this before super()**

A derived class must call `super()` before accessing `this`.

**Adding a return type annotation**

Constructors do not use explicit return type annotations.

**Confusing constructor parameters with properties**

Ordinary constructor parameters are not automatically class properties.

```typescript
class User {
  constructor(name: string) {
    // name is a local constructor parameter
  }
}
```

To create a property, declare it separately or use a parameter property.

**Overloading constructors with unrelated behavior**

Too many constructor overloads can make class creation difficult to understand. Consider object parameters or factory methods.

**Performing excessive work inside constructors**

Constructors should primarily establish a valid initial state. Complex asynchronous operations and unrelated business workflows are better handled through separate methods or factory functions.

# 🗁 Best Practices

* Use constructors to establish valid initial object state.
* Initialize required properties consistently.
* Enable `strictPropertyInitialization`.
* Prefer parameter properties when they improve readability.
* Use object parameters for constructors with many configuration options.
* Keep constructor logic simple and predictable.
* Use constructor overloads only when they provide meaningful API flexibility.
* Call `super()` before accessing `this` in derived constructors.
* Use private constructors for controlled instance creation when appropriate.
* Prefer factory methods when object creation requires complex logic.
* Avoid unnecessary side effects during construction.

# 🗁 Summary

* A constructor runs automatically when a new class instance is created.
* Constructors initialize instance properties.
* Constructors can accept required, optional, and default parameters.
* Parameter properties combine property declaration and initialization.
* Access modifiers can control constructor accessibility.
* Constructor overloads support multiple calling patterns.
* A subclass constructor must call `super()` before accessing `this`.
* Constructors initialize instance members, while static members belong to the class.
* A class can have only one constructor implementation.
* Constructors should establish a valid and predictable initial state.

# 🗁 Related Topics

* Class Basics
* Access Modifiers
* Readonly Properties
* Inheritance
* Abstract Classes
* Implements
* Static Members
* Parameter Properties
* Constructor Overloads
* Factory Methods
* Object Initialization
