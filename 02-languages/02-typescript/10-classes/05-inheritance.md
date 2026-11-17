# Inheritance

Inheritance is an object-oriented programming concept that allows one class to acquire properties and methods from another class.

In TypeScript, inheritance is implemented using the `extends` keyword.

Inheritance helps developers:

* Reuse existing code.
* Extend existing class functionality.
* Reduce code duplication.
* Establish relationships between related classes.
* Implement method overriding.
* Build hierarchical object models.

The class being inherited from is called the **parent class**, **base class**, or **superclass**.

The class that inherits from another class is called the **child class**, **derived class**, or **subclass**.

# 🗁 Basic Inheritance

Use the `extends` keyword to inherit from another class.

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

Output:

```text
Buddy is eating
Buddy is barking
```

The `Dog` class inherits the `name` property and `eat()` method from `Animal`.

It also defines its own `bark()` method.

# 🗁 Parent and Child Classes

Consider the following class hierarchy:

```typescript
class Vehicle {
  start(): void {
    console.log("Vehicle started");
  }
}

class Car extends Vehicle {
  drive(): void {
    console.log("Car is driving");
  }
}

class Motorcycle extends Vehicle {
  ride(): void {
    console.log("Motorcycle is riding");
  }
}
```

Both `Car` and `Motorcycle` inherit from `Vehicle`.

Usage:

```typescript
const car = new Car();
const motorcycle = new Motorcycle();

car.start();
car.drive();

motorcycle.start();
motorcycle.ride();
```

Output:

```text
Vehicle started
Car is driving
Vehicle started
Motorcycle is riding
```

Each child class inherits the common functionality of the parent class while maintaining its own specialized behavior.

# 🗁 The extends Keyword

The `extends` keyword establishes an inheritance relationship.

Syntax:

```typescript
class ChildClass extends ParentClass {
  // Additional properties and methods
}
```

Example:

```typescript
class User {
  constructor(public name: string) {}

  login(): void {
    console.log(`${this.name} logged in`);
  }
}

class Admin extends User {
  manageUsers(): void {
    console.log(`${this.name} is managing users`);
  }
}
```

Usage:

```typescript
const admin = new Admin("Alice");

admin.login();
admin.manageUsers();
```

The child class automatically inherits accessible members from the parent class.

# 🗁 The super Keyword

The `super` keyword provides access to the parent class constructor and methods.

It is commonly used in two situations:

* Calling the parent constructor.
* Calling a parent class method.

**Calling the parent constructor**

When a child class defines its own constructor, it must call `super()` before accessing `this`.

```typescript
class Animal {
  constructor(public name: string) {}
}

class Dog extends Animal {
  constructor(name: string) {
    super(name);
  }
}
```

Usage:

```typescript
const dog = new Dog("Buddy");

console.log(dog.name);
```

**Calling a parent method**

A child class can call a parent method using `super.methodName()`.

```typescript
class Animal {
  eat(): void {
    console.log("Animal is eating");
  }
}

class Dog extends Animal {
  eat(): void {
    super.eat();

    console.log("Dog is eating");
  }
}
```

Usage:

```typescript
const dog = new Dog();

dog.eat();
```

Output:

```text
Animal is eating
Dog is eating
```

The parent implementation executes first, followed by the child implementation.

# 🗁 Constructor Inheritance

A child class without an explicit constructor can use the parent class constructor.

```typescript
class User {
  constructor(
    public name: string,
    public email: string,
  ) {}
}

class Admin extends User {
  manageUsers(): void {
    console.log(`${this.name} manages users`);
  }
}
```

Usage:

```typescript
const admin = new Admin(
  "Alice",
  "alice@example.com",
);

admin.manageUsers();
```

The `Admin` class can be instantiated using the constructor inherited from `User`.

When a child class defines its own constructor, it must explicitly invoke the parent constructor with `super()`.

# 🗁 Method Overriding

Method overriding occurs when a child class provides its own implementation of a method inherited from its parent.

```typescript
class Animal {
  makeSound(): void {
    console.log("Animal makes a sound");
  }
}

class Dog extends Animal {
  override makeSound(): void {
    console.log("Dog barks");
  }
}
```

Usage:

```typescript
const animal = new Animal();
const dog = new Dog();

animal.makeSound();
dog.makeSound();
```

Output:

```text
Animal makes a sound
Dog barks
```

The `override` keyword explicitly indicates that the method is intended to override an inherited member.

It also helps TypeScript detect mistakes.

For example:

```typescript
class Dog extends Animal {
  // Error: No matching member exists in the base class
  // override makeSounds(): void {}
}
```

Enable `noImplicitOverride` in `tsconfig.json` to require explicit `override` modifiers when overriding inherited methods.

# 🗁 Overriding Properties

A child class can override an inherited property when the new declaration is type-compatible with the parent declaration.

```typescript
class Animal {
  name: string = "Animal";
}

class Dog extends Animal {
  override name: string = "Dog";
}
```

Usage:

```typescript
const dog = new Dog();

console.log(dog.name);
```

Output:

```text
Dog
```

The child property replaces the inherited property's behavior for instances of the child class.

When overriding properties, preserve compatibility with the parent class type.

# 🗁 Access Modifiers in Inheritance

Access modifiers determine which inherited members can be accessed by a child class.

**Public members**

Public members are accessible everywhere.

```typescript
class Animal {
  public name: string = "Animal";
}

class Dog extends Animal {
  showName(): void {
    console.log(this.name);
  }
}

const dog = new Dog();

console.log(dog.name);
```

**Protected members**

Protected members are accessible inside the parent class and its subclasses.

```typescript
class Animal {
  protected name: string = "Animal";
}

class Dog extends Animal {
  showName(): void {
    console.log(this.name);
  }
}

const dog = new Dog();

dog.showName();

// Error: Protected member cannot be accessed externally
// console.log(dog.name);
```

**Private members**

Private members cannot be accessed directly from subclasses.

```typescript
class Animal {
  private secret: string = "Hidden";
}

class Dog extends Animal {
  showSecret(): void {
    // Error: Property 'secret' is private
    // console.log(this.secret);
  }
}
```

Use `protected` when a subclass genuinely needs direct access to an inherited member.

# 🗁 Multilevel Inheritance

Multilevel inheritance occurs when a class inherits from another child class.

```typescript
class Animal {
  eat(): void {
    console.log("Eating");
  }
}

class Mammal extends Animal {
  walk(): void {
    console.log("Walking");
  }
}

class Dog extends Mammal {
  bark(): void {
    console.log("Barking");
  }
}
```

Usage:

```typescript
const dog = new Dog();

dog.eat();
dog.walk();
dog.bark();
```

Output:

```text
Eating
Walking
Barking
```

The `Dog` class inherits members from both `Mammal` and `Animal`.

# 🗁 Hierarchical Inheritance

Hierarchical inheritance occurs when multiple child classes inherit from the same parent class.

```typescript
class Animal {
  eat(): void {
    console.log("Eating");
  }
}

class Dog extends Animal {
  bark(): void {
    console.log("Barking");
  }
}

class Cat extends Animal {
  meow(): void {
    console.log("Meowing");
  }
}
```

Usage:

```typescript
const dog = new Dog();
const cat = new Cat();

dog.eat();
dog.bark();

cat.eat();
cat.meow();
```

Both child classes reuse the common functionality defined in `Animal`.

# 🗁 Single Inheritance

TypeScript classes support single class inheritance.

A class can directly extend only one class.

```typescript
class Animal {
  eat(): void {
    console.log("Eating");
  }
}

class Dog extends Animal {
  bark(): void {
    console.log("Barking");
  }
}
```

The following is not valid TypeScript:

```typescript
class Animal {}
class Machine {}

// Invalid: A class cannot extend multiple classes
// class Robot extends Animal, Machine {}
```

If a class needs behavior from multiple sources, interfaces, composition, or mixins can be used.

# 🗁 Inheritance and Abstract Classes

Abstract classes can act as base classes for related implementations.

```typescript
abstract class Shape {
  abstract calculateArea(): number;

  describe(): void {
    console.log("This is a shape");
  }
}

class Circle extends Shape {
  constructor(public radius: number) {
    super();
  }

  override calculateArea(): number {
    return Math.PI * this.radius ** 2;
  }
}
```

Usage:

```typescript
const circle = new Circle(5);

circle.describe();

console.log(circle.calculateArea());
```

The abstract parent defines a required method, while the child class provides its implementation.

Abstract classes are covered in detail in the dedicated abstract classes topic.

# 🗁 Inheritance and Polymorphism

Polymorphism allows different classes to be treated through a common parent type while providing different implementations of shared methods.

```typescript
class Animal {
  makeSound(): void {
    console.log("Unknown sound");
  }
}

class Dog extends Animal {
  override makeSound(): void {
    console.log("Bark");
  }
}

class Cat extends Animal {
  override makeSound(): void {
    console.log("Meow");
  }
}
```

A function can accept the parent type:

```typescript
function playSound(animal: Animal): void {
  animal.makeSound();
}

playSound(new Dog());
playSound(new Cat());
```

Output:

```text
Bark
Meow
```

Although both objects are treated as `Animal`, the actual instance determines which overridden method executes.

This is an important foundation of runtime polymorphism.

# 🗁 Inheritance and Interfaces

A class can extend another class and implement one or more interfaces.

```typescript
interface Printable {
  print(): void;
}

interface Downloadable {
  download(): void;
}

class Document {
  constructor(public title: string) {}

  describe(): void {
    console.log(this.title);
  }
}

class PDFDocument extends Document implements Printable, Downloadable {
  print(): void {
    console.log("Printing PDF");
  }

  download(): void {
    console.log("Downloading PDF");
  }
}
```

Usage:

```typescript
const pdf = new PDFDocument("Report");

pdf.describe();
pdf.print();
pdf.download();
```

A class can extend only one class but implement multiple interfaces.

# 🗁 Inheritance and Type Compatibility

TypeScript uses structural typing for class instance compatibility, with special rules for private and protected members.

```typescript
class Animal {
  name: string;

  constructor(name: string) {
    this.name = name;
  }
}

class Dog extends Animal {
  bark(): void {
    console.log("Barking");
  }
}

function describeAnimal(animal: Animal): void {
  console.log(animal.name);
}

const dog = new Dog("Buddy");

describeAnimal(dog);
```

A `Dog` instance can be passed to a function expecting an `Animal` because it contains the required members.

This is known as substitutability: a derived instance can be used where its base type is expected.

# 🗁 Practical Example: Employee Management

```typescript
class Employee {
  constructor(
    public readonly id: number,
    public name: string,
    protected department: string,
  ) {}

  getDetails(): string {
    return `${this.name} works in ${this.department}`;
  }

  calculateSalary(): number {
    return 30000;
  }
}

class Developer extends Employee {
  constructor(
    id: number,
    name: string,
    department: string,
    public programmingLanguage: string,
  ) {
    super(id, name, department);
  }

  override calculateSalary(): number {
    return 60000;
  }

  writeCode(): void {
    console.log(`${this.name} writes ${this.programmingLanguage}`);
  }
}

class Manager extends Employee {
  constructor(
    id: number,
    name: string,
    department: string,
    public teamSize: number,
  ) {
    super(id, name, department);
  }

  override calculateSalary(): number {
    return 80000;
  }

  manageTeam(): void {
    console.log(`${this.name} manages ${this.teamSize} employees`);
  }
}
```

Usage:

```typescript
const developer = new Developer(
  101,
  "Alice",
  "Engineering",
  "TypeScript",
);

const manager = new Manager(
  102,
  "Bob",
  "Management",
  8,
);

console.log(developer.getDetails());
console.log(developer.calculateSalary());

developer.writeCode();

console.log(manager.getDetails());
console.log(manager.calculateSalary());

manager.manageTeam();
```

Output:

```text
Alice works in Engineering
60000
Alice writes TypeScript
Bob works in Management
80000
Bob manages 8 employees
```

This example demonstrates:

* Parent and child classes.
* Constructor inheritance.
* The `super()` keyword.
* Access modifiers.
* Readonly properties.
* Method overriding.
* Specialized child class behavior.

# 🗁 Inheritance vs Composition

Inheritance is not always the best way to reuse code.

Composition allows a class to use other objects as dependencies instead of inheriting their behavior.

**Inheritance example**

```typescript
class Engine {
  start(): void {
    console.log("Engine started");
  }
}

class Car extends Engine {}
```

This creates an inheritance relationship between `Car` and `Engine`, which is conceptually incorrect because a car is not an engine.

**Composition example**

```typescript
class Engine {
  start(): void {
    console.log("Engine started");
  }
}

class Car {
  constructor(private engine: Engine) {}

  start(): void {
    this.engine.start();

    console.log("Car started");
  }
}
```

Usage:

```typescript
const engine = new Engine();
const car = new Car(engine);

car.start();
```

Composition is often preferable when one object uses another object's functionality without being a specialized version of it.

| Inheritance | Composition |
| --- | --- |
| Establishes an is-a relationship | Establishes a has-a or uses-a relationship |
| Uses `extends` | Uses object references or dependencies |
| Creates a class hierarchy | Combines independent components |
| Can increase coupling | Often provides more flexibility |
| Useful for genuine specialization | Useful for reusable behavior |

# 🗁 Common Mistakes

**Forgetting to call super()**

A child constructor must call `super()` before accessing `this`.

**Trying to extend multiple classes**

TypeScript supports only single class inheritance.

**Accessing private parent members**

Private members cannot be directly accessed from subclasses.

**Overriding methods with incompatible signatures**

An override must remain compatible with the inherited member's type contract.

**Overusing inheritance**

Deep or unnecessary inheritance hierarchies can make applications difficult to maintain.

**Using inheritance for unrelated classes**

Inheritance should represent a genuine specialization relationship.

**Forgetting the override modifier**

The `override` modifier makes the intention explicit and helps detect incorrect method names.

**Assuming parent constructors are automatically called when a child constructor exists**

When a child defines its own constructor, it must explicitly call `super()`.

# 🗁 Best Practices

* Use inheritance for genuine is-a relationships.
* Keep class hierarchies shallow.
* Extract shared behavior into appropriate base classes.
* Use `protected` only when subclasses need access.
* Prefer `private` for implementation details.
* Use the `override` modifier when replacing inherited members.
* Keep overridden methods compatible with parent contracts.
* Use abstract classes when subclasses must implement shared contracts.
* Prefer composition when behavior can be combined through independent components.
* Design parent classes so that derived classes can safely substitute for them.
* Avoid making base classes responsible for unrelated functionality.

# 🗁 Summary

* Inheritance allows one class to reuse and extend another class.
* TypeScript uses the `extends` keyword to establish inheritance.
* Parent classes provide shared properties and methods.
* Child classes can define additional properties and methods.
* The `super` keyword accesses parent constructors and methods.
* Child constructors must call `super()` before accessing `this`.
* Method overriding allows subclasses to customize inherited behavior.
* The `override` modifier makes overriding explicit.
* TypeScript supports single class inheritance.
* A class can implement multiple interfaces while extending one class.
* Inheritance enables polymorphism and code reuse.
* Composition is often a more flexible alternative to inheritance.

# 🗁 Related Topics

* Class Basics
* Access Modifiers
* Readonly Properties
* Constructors
* Abstract Classes
* Implements
* Static Members
* Parameter Properties
* Polymorphism
* Encapsulation
* Interfaces
* Composition
* Mixins
