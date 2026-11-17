# Class Basics

Classes in TypeScript provide a blueprint for creating objects with properties and methods.

TypeScript supports object-oriented programming (OOP) concepts such as:

* Classes and objects.
* Constructors.
* Properties and methods.
* Access modifiers.
* Inheritance.
* Encapsulation.
* Abstraction.
* Polymorphism.
* Interfaces and class contracts.
* Static members.

TypeScript classes are based on JavaScript classes but provide additional compile-time type checking and object-oriented features.

They are useful for organizing related data and behavior into reusable structures.

# 🗁 What Is a Class?

A class is a blueprint that defines the structure and behavior of objects.

Example:

```typescript
class User {
  name: string;
  age: number;

  greet(): void {
    console.log(`Hello, my name is ${this.name}`);
  }
}
```

Here:

* `User` is the class name.
* `name` and `age` are properties.
* `greet()` is a method.
* `this` refers to the current object instance.

The class defines what a `User` object contains and what it can do.

# 🗁 Creating an Object

An object created from a class is called an instance.

Use the `new` keyword to create an instance.

```typescript
class User {
  name: string;
  age: number;

  greet(): void {
    console.log(`Hello, my name is ${this.name}`);
  }
}

const user = new User();

user.name = "Alice";
user.age = 25;

user.greet();
```

Output:

```text
Hello, my name is Alice
```

The `user` variable holds an instance of the `User` class.

You can create multiple instances from the same class.

```typescript
const user1 = new User();
const user2 = new User();

user1.name = "Alice";
user2.name = "Bob";

user1.greet();
user2.greet();
```

Each instance maintains its own property values.

# 🗁 Class Properties

Properties represent the data associated with a class instance.

```typescript
class Product {
  id: number;
  title: string;
  price: number;
  available: boolean;
}
```

TypeScript requires class properties to be initialized or definitely assigned, depending on the compiler configuration.

One way to initialize properties is through a constructor.

```typescript
class Product {
  id: number;
  title: string;
  price: number;

  constructor(
    id: number,
    title: string,
    price: number,
  ) {
    this.id = id;
    this.title = title;
    this.price = price;
  }
}
```

Usage:

```typescript
const product = new Product(
  101,
  "Keyboard",
  75,
);
```

# 🗁 Class Methods

Methods are functions defined inside a class.

They describe the behavior of an instance.

```typescript
class Calculator {
  add(a: number, b: number): number {
    return a + b;
  }

  subtract(a: number, b: number): number {
    return a - b;
  }

  multiply(a: number, b: number): number {
    return a * b;
  }

  divide(a: number, b: number): number {
    if (b === 0) {
      throw new Error("Division by zero is not allowed");
    }

    return a / b;
  }
}
```

Usage:

```typescript
const calculator = new Calculator();

console.log(calculator.add(10, 5));
console.log(calculator.subtract(10, 5));
console.log(calculator.multiply(10, 5));
console.log(calculator.divide(10, 5));
```

Output:

```text
15
5
50
2
```

# 🗁 The this Keyword

The `this` keyword refers to the current class instance inside an instance method or constructor.

It allows methods to access and modify instance properties.

```typescript
class User {
  name: string;

  constructor(name: string) {
    this.name = name;
  }

  updateName(name: string): void {
    this.name = name;
  }

  getName(): string {
    return this.name;
  }
}
```

Usage:

```typescript
const user = new User("Alice");

console.log(user.getName());

user.updateName("Bob");

console.log(user.getName());
```

Output:

```text
Alice
Bob
```

The `this.name` expression refers to the `name` property belonging to the current instance.

# 🗁 Constructors

A constructor is a special method that executes when a new instance of a class is created.

It is commonly used to initialize instance properties.

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

Usage:

```typescript
const user = new User("Alice", 25);

console.log(user.name);
console.log(user.age);
```

A class can have only one constructor implementation.

Constructors can accept required, optional, and default parameters.

```typescript
class Product {
  constructor(
    public title: string,
    public price: number = 0,
  ) {}
}
```

Constructor behavior and parameter properties are covered in detail in the dedicated constructor topic.

# 🗁 Property Initialization

TypeScript checks whether class properties are initialized.

Consider:

```typescript
class User {
  name: string;
  age: number;
}
```

With `strictPropertyInitialization` enabled, TypeScript reports errors because the properties are not initialized.

There are several ways to solve this.

**Initialize properties directly**

```typescript
class User {
  name: string = "Guest";
  age: number = 0;
}
```

**Initialize properties in a constructor**

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

**Use a definite assignment assertion**

```typescript
class User {
  name!: string;
  age!: number;
}
```

The `!` tells TypeScript that the property will be assigned through some mechanism that the compiler cannot verify.

Use it only when initialization is guaranteed by the actual application logic.

# 🗁 Class Expressions

Classes can also be created using class expressions.

```typescript
const User = class {
  name: string;

  constructor(name: string) {
    this.name = name;
  }

  greet(): string {
    return `Hello, ${this.name}`;
  }
};
```

Usage:

```typescript
const user = new User("Alice");

console.log(user.greet());
```

Class expressions can be assigned to variables, passed as values, or used in other expressions.

They can also have an internal class name.

```typescript
const User = class UserModel {
  constructor(
    public name: string,
  ) {}
};
```

The internal name is useful within the class body, but the external reference is the variable `User`.

# 🗁 Class Types

When you declare a class, TypeScript creates two related types:

* The instance type.
* The constructor and static type.

Example:

```typescript
class User {
  name: string;

  constructor(name: string) {
    this.name = name;
  }

  greet(): string {
    return `Hello, ${this.name}`;
  }

  static role = "user";
}
```

The instance type is represented by the class name:

```typescript
type UserInstance = User;
```

The constructor and static side can be referenced with `typeof`:

```typescript
type UserConstructor = typeof User;
```

These represent different structures.

The instance type contains instance properties and methods.

The static side contains the constructor and static members.

# 🗁 Class as a Type

A class name can be used as a type to describe objects created from that class.

```typescript
class User {
  constructor(
    public name: string,
  ) {}

  greet(): string {
    return `Hello, ${this.name}`;
  }
}

function displayUser(user: User): void {
  console.log(user.greet());
}
```

Usage:

```typescript
const user = new User("Alice");

displayUser(user);
```

The parameter must satisfy the instance type of `User`.

TypeScript uses structural compatibility for most class instance types. An object with compatible members can sometimes be assigned even if it was not created with `new User()`.

Private and protected members introduce additional compatibility restrictions.

# 🗁 Class Inheritance Preview

Inheritance allows one class to reuse and extend another class.

Use the `extends` keyword.

```typescript
class Animal {
  name: string;

  constructor(name: string) {
    this.name = name;
  }

  eat(): void {
    console.log(`${this.name} is eating`);
  }
}

class Dog extends Animal {
  bark(): void {
    console.log(`${this.name} is barking`);
  }
}
```

Usage:

```typescript
const dog = new Dog("Buddy");

dog.eat();
dog.bark();
```

The `Dog` class inherits the properties and methods of `Animal`.

Inheritance is covered in detail in the dedicated inheritance topic.

# 🗁 Class and Interface Preview

A class can implement an interface to guarantee that it provides the required structure.

```typescript
interface Printable {
  print(): void;
}

class Document implements Printable {
  print(): void {
    console.log("Printing document");
  }
}
```

The class must implement the required members.

Interfaces define contracts, while classes provide implementations.

# 🗁 Class Members Overview

TypeScript classes support several kinds of members.

| Member | Purpose |
| --- | --- |
| Properties | Store instance data |
| Methods | Define instance behavior |
| Constructors | Initialize new instances |
| Access modifiers | Control member accessibility |
| Readonly properties | Prevent reassignment |
| Static members | Belong to the class itself |
| Getters | Provide controlled property access |
| Setters | Provide controlled property updates |
| Abstract members | Define requirements for subclasses |

These features allow classes to represent complex application models and services.

# 🗁 Class vs Object

A class and an object are related but different concepts.

| Class | Object |
| --- | --- |
| Blueprint for creating instances | Actual instance containing values |
| Defines properties and methods | Holds property values and behavior |
| Declared using `class` | Created using `new` or object literals |
| Describes a reusable structure | Represents a particular entity |

Example:

```typescript
class User {
  constructor(
    public name: string,
  ) {}
}

const user1 = new User("Alice");
const user2 = new User("Bob");
```

`User` is the class, while `user1` and `user2` are separate instances.

# 🗁 Class vs Interface

Classes and interfaces serve different purposes.

| Class | Interface |
| --- | --- |
| Can create runtime instances | Does not create runtime instances |
| Can contain implementations | Defines structural contracts |
| Supports constructors | Does not have constructors |
| Supports inheritance | Supports interface extension |
| Can contain static members | Cannot define runtime static members |
| Exists in emitted JavaScript | Erased during compilation |

A class can implement one or more interfaces.

# 🗁 Practical Example: User Management

The following example demonstrates a simple user model.

```typescript
class User {
  id: number;
  name: string;
  email: string;
  active: boolean;

  constructor(
    id: number,
    name: string,
    email: string,
  ) {
    this.id = id;
    this.name = name;
    this.email = email;
    this.active = true;
  }

  updateEmail(email: string): void {
    this.email = email;
  }

  deactivate(): void {
    this.active = false;
  }

  getProfile(): string {
    return `${this.name} (${this.email})`;
  }
}
```

Create a user:

```typescript
const user = new User(
  1,
  "Alice",
  "alice@example.com",
);
```

Access properties and methods:

```typescript
console.log(user.getProfile());

user.updateEmail("new@example.com");

user.deactivate();

console.log(user.active);
```

Output:

```text
Alice (alice@example.com)
false
```

This example demonstrates how a class groups related data and behavior.

# 🗁 Practical Example: Product Management

```typescript
class Product {
  constructor(
    public id: number,
    public title: string,
    public price: number,
    public stock: number,
  ) {}

  getTotalValue(): number {
    return this.price * this.stock;
  }

  updateStock(quantity: number): void {
    if (quantity < 0) {
      throw new Error("Stock cannot be negative");
    }

    this.stock = quantity;
  }

  getDetails(): string {
    return `${this.title}: $${this.price}`;
  }
}
```

Usage:

```typescript
const product = new Product(
  101,
  "Keyboard",
  75,
  20,
);

console.log(product.getDetails());

console.log(product.getTotalValue());

product.updateStock(15);
```

The class keeps product-related data and operations together.

# 🗁 Common Mistakes

**Forgetting to initialize properties**

```typescript
class User {
  name: string;
}
```

With strict property initialization enabled, this produces an error.

Initialize the property or assign it in the constructor.

**Using this outside an instance context**

The `this` keyword must be used in an appropriate class or function context.

**Creating an instance without new**

```typescript
class User {}

// TypeScript error
// const user = User();
```

Correct:

```typescript
const user = new User();
```

**Confusing static and instance members**

Instance members belong to individual objects, while static members belong to the class itself.

**Assuming classes provide runtime validation automatically**

TypeScript checks types during compilation. It does not automatically validate external JSON, database records, or user input at runtime.

**Using classes when a simple object is sufficient**

Not every data structure needs a class. Interfaces, type aliases, and object literals are often simpler for plain data models.

# 🗁 Best Practices

* Use PascalCase for class names.
* Give each class a clear responsibility.
* Initialize required properties in constructors or declarations.
* Enable strict compiler settings.
* Use `readonly` for values that should not be reassigned.
* Use access modifiers to control encapsulation.
* Prefer composition when inheritance does not represent a genuine relationship.
* Keep methods focused and predictable.
* Use interfaces when defining contracts between components.
* Avoid unnecessary classes for simple data structures.
* Separate business logic from unrelated infrastructure concerns.
* Validate untrusted runtime data independently of TypeScript's static types.

# 🗁 Summary

* A class is a blueprint for creating objects.
* Objects created from classes are called instances.
* Properties represent instance data.
* Methods represent instance behavior.
* Constructors initialize new instances.
* The `this` keyword refers to the current instance.
* TypeScript provides compile-time checks for class members.
* Classes support inheritance, interfaces, access modifiers, and static members.
* A class declaration creates both an instance type and a constructor/static type.
* Classes are useful for organizing related data and behavior into reusable structures.

# 🗁 Related Topics

* Access Modifiers
* Readonly Properties
* Constructors
* Inheritance
* Abstract Classes
* Implements
* Static Members
* Parameter Properties
* Interfaces
* Object Types
* Generic Classes
* Object-Oriented Programming
* Encapsulation
* Polymorphism
* Composition
