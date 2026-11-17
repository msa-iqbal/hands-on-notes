# Abstract Classes

An abstract class in TypeScript is a class that cannot be instantiated directly. It is designed to serve as a base class for other classes.

Abstract classes define a common structure and behavior that derived classes must follow or can reuse.

They are useful for:

* Defining common class contracts.
* Enforcing implementation of required methods.
* Sharing common properties and methods.
* Supporting inheritance and polymorphism.
* Designing extensible application architectures.

Use the `abstract` keyword to declare an abstract class.

# 🗁 What Is an Abstract Class?

An abstract class acts as a blueprint for other classes.

```typescript
abstract class Animal {
  name: string;

  constructor(name: string) {
    this.name = name;
  }

  eat(): void {
    console.log(`${this.name} is eating`);
  }
}
```

The class contains a property, a constructor, and a concrete method.

However, you cannot create an instance directly:

```typescript
// Error: Cannot create an instance of an abstract class
// const animal = new Animal("Animal");
```

A subclass can extend the abstract class and create an instance.

```typescript
class Dog extends Animal {
  bark(): void {
    console.log(`${this.name} is barking`);
  }
}

const dog = new Dog("Buddy");

dog.eat();
dog.bark();
```

Output:

```text
Buddy is eating
Buddy is barking
```

# 🗁 Declaring an Abstract Class

Use the `abstract` keyword before the `class` keyword.

Syntax:

```typescript
abstract class ClassName {
  // Properties
  // Concrete methods
  // Abstract methods
}
```

Example:

```typescript
abstract class Vehicle {
  brand: string;

  constructor(brand: string) {
    this.brand = brand;
  }

  start(): void {
    console.log(`${this.brand} started`);
  }
}
```

An abstract class can contain both implemented and unimplemented members.

# 🗁 Abstract Methods

An abstract method is a method declared without an implementation.

It defines a requirement that a concrete subclass must fulfill.

```typescript
abstract class Animal {
  abstract makeSound(): void;
}
```

A concrete subclass must implement the abstract method.

```typescript
class Dog extends Animal {
  makeSound(): void {
    console.log("Bark");
  }
}

class Cat extends Animal {
  makeSound(): void {
    console.log("Meow");
  }
}
```

Usage:

```typescript
const dog = new Dog();
const cat = new Cat();

dog.makeSound();
cat.makeSound();
```

Output:

```text
Bark
Meow
```

The abstract class defines what subclasses must do, while each subclass determines how to do it.

# 🗁 Abstract Methods with Parameters

Abstract methods can accept parameters and return typed values.

```typescript
abstract class Calculator {
  abstract calculate(a: number, b: number): number;
}

class Addition extends Calculator {
  calculate(a: number, b: number): number {
    return a + b;
  }
}

class Multiplication extends Calculator {
  calculate(a: number, b: number): number {
    return a * b;
  }
}
```

Usage:

```typescript
const addition = new Addition();
const multiplication = new Multiplication();

console.log(addition.calculate(10, 5));
console.log(multiplication.calculate(10, 5));
```

Output:

```text
15
50
```

The subclasses must provide implementations compatible with the abstract method's signature.

# 🗁 Abstract Properties

Abstract classes can declare abstract properties.

A concrete subclass must provide the required property.

```typescript
abstract class User {
  abstract name: string;
  abstract email: string;
}

class Customer extends User {
  name: string;
  email: string;

  constructor(name: string, email: string) {
    super();

    this.name = name;
    this.email = email;
  }
}
```

Usage:

```typescript
const customer = new Customer(
  "Alice",
  "alice@example.com",
);

console.log(customer.name);
console.log(customer.email);
```

Abstract properties define required members without providing their implementation.

# 🗁 Abstract Readonly Properties

Abstract properties can also use `readonly`.

```typescript
abstract class Entity {
  abstract readonly id: number;
}

class User extends Entity {
  readonly id: number;

  constructor(id: number) {
    super();

    this.id = id;
  }
}
```

The subclass must provide a compatible readonly property.

# 🗁 Concrete Methods in Abstract Classes

An abstract class can contain normal methods with complete implementations.

These methods are inherited by subclasses.

```typescript
abstract class Employee {
  constructor(
    public name: string,
    public salary: number,
  ) {}

  getDetails(): string {
    return `${this.name} earns ${this.salary}`;
  }

  abstract calculateBonus(): number;
}
```

A subclass can use the existing method and implement the abstract method.

```typescript
class Developer extends Employee {
  calculateBonus(): number {
    return this.salary * 0.15;
  }
}
```

Usage:

```typescript
const developer = new Developer("Alice", 60000);

console.log(developer.getDetails());
console.log(developer.calculateBonus());
```

Output:

```text
Alice earns 60000
9000
```

This allows abstract classes to provide shared functionality while requiring specialized behavior.

# 🗁 Constructors in Abstract Classes

Abstract classes can have constructors.

Their constructors are executed when a concrete subclass is instantiated.

```typescript
abstract class Animal {
  constructor(public name: string) {
    console.log(`${name} initialized`);
  }

  abstract makeSound(): void;
}

class Dog extends Animal {
  makeSound(): void {
    console.log("Bark");
  }
}
```

Usage:

```typescript
const dog = new Dog("Buddy");

dog.makeSound();
```

Output:

```text
Buddy initialized
Bark
```

The abstract class constructor initializes the inherited properties.

An abstract class itself cannot be instantiated, but its constructor participates in creating subclass instances.

# 🗁 Inheritance with Abstract Classes

Abstract classes can be extended by other abstract classes or concrete classes.

An abstract subclass does not need to implement every inherited abstract member immediately.

```typescript
abstract class Animal {
  abstract makeSound(): void;

  eat(): void {
    console.log("Eating");
  }
}

abstract class Mammal extends Animal {
  walk(): void {
    console.log("Walking");
  }
}
```

A concrete subclass must implement the remaining abstract members.

```typescript
class Dog extends Mammal {
  makeSound(): void {
    console.log("Bark");
  }
}
```

Usage:

```typescript
const dog = new Dog();

dog.eat();
dog.walk();
dog.makeSound();
```

Output:

```text
Eating
Walking
Bark
```

# 🗁 Abstract Class and Access Modifiers

Abstract classes support access modifiers.

```typescript
abstract class User {
  public name: string;

  protected role: string;

  private password: string;

  constructor(
    name: string,
    role: string,
    password: string,
  ) {
    this.name = name;
    this.role = role;
    this.password = password;
  }

  abstract getPermissions(): string[];

  protected verifyPassword(password: string): boolean {
    return this.password === password;
  }
}
```

A subclass can access public and protected members according to their access rules, but it cannot directly access private members.

```typescript
class Admin extends User {
  getPermissions(): string[] {
    return [
      "create-user",
      "update-user",
      "delete-user",
    ];
  }

  authenticate(password: string): boolean {
    return this.verifyPassword(password);
  }
}
```

# 🗁 Abstract Static Members

Abstract classes can define static members with implementations.

```typescript
abstract class Application {
  static version = "1.0.0";

  static getVersion(): string {
    return this.version;
  }

  abstract start(): void;
}
```

Usage:

```typescript
console.log(Application.getVersion());
```

Output:

```text
1.0.0
```

Static members belong to the class itself rather than individual instances.

Abstract static members have additional restrictions and should not be confused with ordinary abstract instance members. Modern TypeScript also supports static abstract members in certain class and interface contracts, with specific implementation rules.

# 🗁 Abstract Class as a Type

An abstract class can be used as a type for variables, parameters, and other type declarations.

```typescript
abstract class Animal {
  abstract makeSound(): void;
}

class Dog extends Animal {
  makeSound(): void {
    console.log("Bark");
  }
}

function playSound(animal: Animal): void {
  animal.makeSound();
}

const dog = new Dog();

playSound(dog);
```

Although `Animal` cannot be instantiated, its instance type can describe objects created from concrete subclasses.

# 🗁 Abstract Constructors and Factory Functions

Abstract classes can also be referenced through abstract constructor types.

```typescript
abstract class Animal {
  abstract makeSound(): void;
}

type AnimalConstructor = abstract new () => Animal;
```

This describes a constructor type that can represent abstract or concrete subclasses of `Animal`.

A function that needs to instantiate a class must accept a concrete constructor type instead.

```typescript
type ConcreteAnimalConstructor = new () => Animal;

function createAnimal(
  AnimalClass: ConcreteAnimalConstructor,
): Animal {
  return new AnimalClass();
}
```

Example:

```typescript
class Dog extends Animal {
  makeSound(): void {
    console.log("Bark");
  }
}

const dog = createAnimal(Dog);

dog.makeSound();
```

An abstract constructor type can describe abstract classes, but it cannot be used directly with `new`.

# 🗁 Abstract Class vs Regular Class

| Feature | Regular Class | Abstract Class |
| --- | --- | --- |
| Can be instantiated | Yes | No |
| Can contain properties | Yes | Yes |
| Can contain concrete methods | Yes | Yes |
| Can contain abstract methods | No | Yes |
| Can contain abstract properties | No | Yes |
| Can have constructors | Yes | Yes |
| Can be extended | Yes | Yes |
| Can implement interfaces | Yes | Yes |

Use a regular class when instances should be created directly.

Use an abstract class when it should serve as a shared foundation for other classes.

# 🗁 Abstract Class vs Interface

Abstract classes and interfaces can both define contracts, but they serve different purposes.

| Feature | Abstract Class | Interface |
| --- | --- | --- |
| Runtime existence | Yes | No |
| Can be instantiated | No | No |
| Can contain implementation | Yes | No runtime implementation |
| Can contain constructors | Yes | No |
| Can contain instance properties | Yes | Yes |
| Supports access modifiers | Yes | No |
| Supports private implementation state | Yes | No |
| Inheritance | Single class inheritance | Multiple interface extension |
| Class relationship | `extends` | `implements` |

An abstract class is useful when related classes share implementation and state.

An interface is useful when defining a structural contract that different classes or objects can implement.

# 🗁 Practical Example: Payment Processing

```typescript
abstract class Payment {
  constructor(
    public readonly amount: number,
    public readonly currency: string,
  ) {}

  abstract process(): boolean;

  getDetails(): string {
    return `${this.amount} ${this.currency}`;
  }
}

class CardPayment extends Payment {
  constructor(
    amount: number,
    currency: string,
    private cardNumber: string,
  ) {
    super(amount, currency);
  }

  override process(): boolean {
    console.log(`Processing card payment: ${this.cardNumber}`);

    return true;
  }
}

class PayPalPayment extends Payment {
  constructor(
    amount: number,
    currency: string,
    private email: string,
  ) {
    super(amount, currency);
  }

  override process(): boolean {
    console.log(`Processing PayPal payment: ${this.email}`);

    return true;
  }
}
```

Usage:

```typescript
const cardPayment = new CardPayment(
  100,
  "USD",
  "****1234",
);

const paypalPayment = new PayPalPayment(
  200,
  "USD",
  "alice@example.com",
);

console.log(cardPayment.getDetails());
cardPayment.process();

console.log(paypalPayment.getDetails());
paypalPayment.process();
```

Output:

```text
100 USD
Processing card payment: ****1234
200 USD
Processing PayPal payment: alice@example.com
```

The `Payment` class defines the common payment structure and requires every payment implementation to provide its own `process()` method.

# 🗁 Practical Example: Notification System

```typescript
abstract class Notification {
  constructor(
    public recipient: string,
    public message: string,
  ) {}

  abstract send(): void;

  getPreview(): string {
    return `To: ${this.recipient}\nMessage: ${this.message}`;
  }
}

class EmailNotification extends Notification {
  override send(): void {
    console.log(`Sending email to ${this.recipient}`);
  }
}

class SMSNotification extends Notification {
  override send(): void {
    console.log(`Sending SMS to ${this.recipient}`);
  }
}

class PushNotification extends Notification {
  override send(): void {
    console.log(`Sending push notification to ${this.recipient}`);
  }
}
```

Usage:

```typescript
const notifications: Notification[] = [
  new EmailNotification(
    "alice@example.com",
    "Welcome!",
  ),
  new SMSNotification(
    "+8801700000000",
    "Your OTP is 123456",
  ),
  new PushNotification(
    "user-101",
    "You have a new message",
  ),
];

for (const notification of notifications) {
  console.log(notification.getPreview());
  notification.send();
}
```

Each notification shares common data and behavior while implementing its own sending operation.

# 🗁 Common Mistakes

**Trying to instantiate an abstract class**

Abstract classes cannot be instantiated directly. Create a concrete subclass instead.

**Forgetting to implement abstract methods**

A concrete subclass must implement all inherited abstract members.

**Assuming every method in an abstract class must be abstract**

Abstract classes can contain fully implemented methods and ordinary properties.

**Using abstract classes only for contracts**

If no shared implementation or class state is needed, an interface may be simpler.

**Confusing abstract classes with interfaces**

Abstract classes exist at runtime, while interfaces are erased during compilation.

**Using abstract when a regular class is sufficient**

Avoid unnecessary abstraction. Use abstract classes when they provide meaningful shared behavior or enforce required subclass behavior.

# 🗁 Best Practices

* Use abstract classes for closely related classes with shared behavior.
* Declare abstract methods when subclasses must provide specific implementations.
* Keep shared functionality in concrete parent methods.
* Use interfaces for contracts that do not require shared implementation.
* Keep abstract class responsibilities focused.
* Prefer shallow inheritance hierarchies.
* Use access modifiers to encapsulate internal state.
* Use `override` when implementing inherited concrete methods.
* Ensure subclasses remain compatible with the parent class contract.
* Avoid unnecessary abstraction and overly complicated class hierarchies.

# 🗁 Summary

* An abstract class is a base class that cannot be instantiated directly.
* Use the `abstract` keyword to declare an abstract class.
* Abstract classes can contain abstract and concrete members.
* Abstract methods require implementation in concrete subclasses.
* Abstract properties define required properties for subclasses.
* Abstract classes can have constructors and access modifiers.
* Abstract classes support inheritance and polymorphism.
* Abstract classes can be used as instance types.
* Abstract classes exist at runtime, unlike interfaces.
* Use abstract classes when related classes need shared implementation and enforced contracts.

# 🗁 Related Topics

* Class Basics
* Access Modifiers
* Readonly Properties
* Constructors
* Inheritance
* Implements
* Static Members
* Parameter Properties
* Interfaces
* Polymorphism
* Encapsulation
* Abstract Constructor Types
