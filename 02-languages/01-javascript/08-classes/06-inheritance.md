# Inheritance

Inheritance is an object-oriented programming (OOP) concept that allows one class to acquire properties and methods from another class.

It helps developers:

* Reuse existing code.
* Reduce duplication.
* Create hierarchical relationships between classes.
* Extend existing functionality.
* Organize related objects into logical structures.

In JavaScript, inheritance is implemented using the `extends` keyword.

Example:

```javascript
class Animal {
  eat() {
    console.log("Animal is eating");
  }
}

class Dog extends Animal {
  bark() {
    console.log("Dog is barking");
  }
}

const dog = new Dog();

dog.eat();
dog.bark();
```

Output:

```text
Animal is eating
Dog is barking
```

Here:

* `Animal` is the parent class.
* `Dog` is the child class.
* `Dog` inherits the `eat()` method from `Animal`.
* `Dog` defines its own `bark()` method.

# 🗁 Parent Class and Child Class

Inheritance involves two important class roles.

**Parent Class (Superclass or Base Class)**

The class whose properties and methods are inherited by another class.

**Child Class (Subclass or Derived Class)**

The class that inherits properties and methods from another class.

Example:

```javascript
class Vehicle {
  start() {
    console.log("Vehicle started");
  }
}

class Car extends Vehicle {
  drive() {
    console.log("Car is driving");
  }
}

const car = new Car();

car.start();
car.drive();
```

Output:

```text
Vehicle started
Car is driving
```

`Vehicle` provides shared functionality, while `Car` adds more specific behavior.

# 🗁 The extends Keyword

The `extends` keyword establishes an inheritance relationship between two classes.

Syntax:

```javascript
class ChildClass extends ParentClass {
  // Additional properties and methods
}
```

Example:

```javascript
class Person {
  introduce() {
    console.log("I am a person");
  }
}

class Developer extends Person {
  code() {
    console.log("Writing code");
  }
}

const developer = new Developer();

developer.introduce();
developer.code();
```

Output:

```text
I am a person
Writing code
```

The `Developer` class automatically inherits accessible instance methods from `Person`.

# 🗁 Inheriting Constructors

A child class can inherit the constructor behavior of its parent class.

If a child class does not define its own constructor, JavaScript provides a default constructor that forwards its arguments to the parent constructor.

Example:

```javascript
class Person {
  constructor(name) {
    this.name = name;
  }

  introduce() {
    console.log(`My name is ${this.name}`);
  }
}

class Developer extends Person {}

const developer = new Developer("Muhammad");

developer.introduce();
```

Output:

```text
My name is Muhammad
```

The `Developer` class inherits the constructor behavior from `Person`.

# 🗁 Defining a Constructor in a Child Class

A child class can define its own constructor to initialize additional properties.

However, a derived class must call `super()` before accessing `this`.

Example:

```javascript
class Person {
  constructor(name) {
    this.name = name;
  }
}

class Developer extends Person {
  constructor(name, language) {
    super(name);

    this.language = language;
  }

  introduce() {
    console.log(`${this.name} works with ${this.language}`);
  }
}

const developer = new Developer("Muhammad", "JavaScript");

developer.introduce();
```

Output:

```text
Muhammad works with JavaScript
```

Here:

* `super(name)` invokes the parent constructor.
* `this.name` is initialized by the parent constructor.
* `this.language` is initialized by the child constructor.

The detailed behavior of `super()` is covered in the next topic.

# 🗁 Inheriting Properties

A child class can access inherited instance properties initialized by the parent constructor.

Example:

```javascript
class User {
  constructor(name, email) {
    this.name = name;
    this.email = email;
  }
}

class Admin extends User {
  constructor(name, email, role) {
    super(name, email);

    this.role = role;
  }

  display() {
    console.log(this.name);
    console.log(this.email);
    console.log(this.role);
  }
}

const admin = new Admin(
  "Muhammad",
  "muhammad@example.com",
  "Administrator"
);

admin.display();
```

Output:

```text
Muhammad
muhammad@example.com
Administrator
```

The `Admin` instance contains properties initialized by both the parent and child constructors.

# 🗁 Inheriting Methods

A child class can use inherited instance methods without redefining them.

```javascript
class Animal {
  eat() {
    console.log("Eating...");
  }

  sleep() {
    console.log("Sleeping...");
  }
}

class Cat extends Animal {
  meow() {
    console.log("Meow!");
  }
}

const cat = new Cat();

cat.eat();
cat.sleep();
cat.meow();
```

Output:

```text
Eating...
Sleeping...
Meow!
```

Inherited methods are normally resolved through the prototype chain rather than copied directly onto each instance.

# 🗁 Adding New Methods in a Child Class

A child class can introduce methods that do not exist in its parent.

```javascript
class Employee {
  constructor(name) {
    this.name = name;
  }

  work() {
    console.log(`${this.name} is working`);
  }
}

class Developer extends Employee {
  writeCode() {
    console.log(`${this.name} is writing code`);
  }

  debug() {
    console.log(`${this.name} is debugging`);
  }
}

const developer = new Developer("Muhammad");

developer.work();
developer.writeCode();
developer.debug();
```

Output:

```text
Muhammad is working
Muhammad is writing code
Muhammad is debugging
```

The child class extends the parent functionality without modifying the parent class.

# 🗁 Method Overriding

Method overriding occurs when a child class defines a method with the same name as an inherited method.

The child class implementation takes precedence when that method is accessed through the child instance.

Example:

```javascript
class Animal {
  makeSound() {
    console.log("Animal makes a sound");
  }
}

class Dog extends Animal {
  makeSound() {
    console.log("Dog barks");
  }
}

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

The `Dog` class overrides the inherited `makeSound()` method.

The parent implementation still exists and can be accessed through `super.makeSound()` inside the child class.

# 🗁 Multilevel Inheritance

Multilevel inheritance occurs when a class inherits from another child class, creating multiple levels in the inheritance hierarchy.

Example:

```javascript
class Animal {
  eat() {
    console.log("Eating...");
  }
}

class Mammal extends Animal {
  walk() {
    console.log("Walking...");
  }
}

class Dog extends Mammal {
  bark() {
    console.log("Barking...");
  }
}

const dog = new Dog();

dog.eat();
dog.walk();
dog.bark();
```

Output:

```text
Eating...
Walking...
Barking...
```

Inheritance hierarchy:

```text
Animal
   |
   v
Mammal
   |
   v
Dog
```

The `Dog` class inherits functionality from both `Mammal` and `Animal` through the prototype chain.

# 🗁 Hierarchical Inheritance

Hierarchical inheritance occurs when multiple child classes inherit from the same parent class.

Example:

```javascript
class Animal {
  eat() {
    console.log("Eating...");
  }
}

class Dog extends Animal {
  bark() {
    console.log("Barking...");
  }
}

class Cat extends Animal {
  meow() {
    console.log("Meowing...");
  }
}

const dog = new Dog();
const cat = new Cat();

dog.eat();
dog.bark();

cat.eat();
cat.meow();
```

Output:

```text
Eating...
Barking...
Eating...
Meowing...
```

Both `Dog` and `Cat` inherit the shared `eat()` method from `Animal`.

# 🗁 JavaScript Does Not Support Multiple Class Inheritance

JavaScript classes cannot directly extend multiple parent classes.

The following syntax is invalid:

```javascript
class Child extends ParentA, ParentB {
  // Invalid syntax
}
```

A class can extend only one class.

However, similar code reuse can be achieved through composition, mixins, or delegation.

Example using composition:

```javascript
const canFly = {
  fly() {
    console.log("Flying...");
  }
};

const canSwim = {
  swim() {
    console.log("Swimming...");
  }
};

class Duck {
  constructor() {
    Object.assign(this, canFly, canSwim);
  }
}

const duck = new Duck();

duck.fly();
duck.swim();
```

Output:

```text
Flying...
Swimming...
```

This approach combines behaviors without requiring multiple class inheritance.

# 🗁 Inheritance and the Prototype Chain

JavaScript inheritance is based on prototypes.

When a method is called on an object, JavaScript searches for that method in the following order:

* The object itself.
* Its immediate prototype.
* The prototype's prototype.
* Subsequent prototypes.
* The end of the prototype chain, `null`.

Example:

```javascript
class Animal {
  eat() {
    console.log("Eating...");
  }
}

class Dog extends Animal {
  bark() {
    console.log("Barking...");
  }
}

const dog = new Dog();

console.log(Object.hasOwn(dog, "eat"));
console.log(Object.hasOwn(dog, "bark"));

console.log("eat" in dog);
console.log("bark" in dog);
```

Output:

```text
false
false
true
true
```

Neither method is an own property of the instance. Both are available through its prototype chain.

# 🗁 Checking Inheritance Relationships

JavaScript provides several ways to inspect inheritance relationships.

**instanceof**

Checks whether a constructor's prototype appears in an object's prototype chain.

```javascript
class Animal {}

class Dog extends Animal {}

const dog = new Dog();

console.log(dog instanceof Dog);
console.log(dog instanceof Animal);
console.log(dog instanceof Object);
```

Output:

```text
true
true
true
```

**Object.getPrototypeOf()**

Returns the prototype of an object.

```javascript
class Animal {}

class Dog extends Animal {}

const dog = new Dog();

console.log(Object.getPrototypeOf(dog) === Dog.prototype);

console.log(
  Object.getPrototypeOf(Dog.prototype) === Animal.prototype
);
```

Output:

```text
true
true
```

**Object.prototype.isPrototypeOf()**

Checks whether an object exists in another object's prototype chain.

```javascript
class Animal {}

class Dog extends Animal {}

const dog = new Dog();

console.log(Dog.prototype.isPrototypeOf(dog));
console.log(Animal.prototype.isPrototypeOf(dog));
```

Output:

```text
true
true
```

# 🗁 Inheriting Static Members

Static methods and properties belong to the class constructor rather than individual instances.

A child class can inherit accessible static members from its parent class.

Example:

```javascript
class User {
  static role = "User";

  static getRole() {
    return this.role;
  }
}

class Admin extends User {
  static role = "Admin";
}

console.log(User.getRole());
console.log(Admin.getRole());
```

Output:

```text
User
Admin
```

The `Admin` class inherits the static `getRole()` method.

Because the method uses `this.role`, it accesses the property belonging to the class through which the method is called.

Static inheritance follows the constructor's prototype chain.

# 🗁 Private Fields and Inheritance

Private fields declared with `#` are accessible only inside the class body where they are declared.

A child class cannot directly access a parent's private fields.

Example:

```javascript
class User {
  #password = "secret123";

  getPassword() {
    return this.#password;
  }
}

class Admin extends User {
  displayPassword() {
    return this.#password;
  }
}

const admin = new Admin();

console.log(admin.getPassword());
```

Calling `admin.displayPassword()` throws a `TypeError` because `#password` is not accessible from the `Admin` class.

The parent can expose controlled access through public or protected-style methods.

```javascript
class User {
  #password = "secret123";

  verifyPassword(password) {
    return this.#password === password;
  }
}

class Admin extends User {
  authenticate(password) {
    return this.verifyPassword(password);
  }
}

const admin = new Admin();

console.log(admin.authenticate("secret123"));
```

Output:

```text
true
```

JavaScript does not have a native `protected` access modifier. Protected-style access is commonly implemented using naming conventions or carefully designed methods.

# 🗁 Inheritance vs Composition

Inheritance and composition are two different approaches to reusing functionality.

**Inheritance**

A child class extends another class and establishes an `is-a` relationship.

Example:

```javascript
class Animal {
  eat() {
    console.log("Eating...");
  }
}

class Dog extends Animal {}
```

A dog is an animal.

**Composition**

An object receives functionality from other objects or components and establishes a `has-a` or uses-a relationship.

Example:

```javascript
const engine = {
  start() {
    console.log("Engine started");
  }
};

class Car {
  constructor() {
    this.engine = engine;
  }

  start() {
    this.engine.start();
  }
}

const car = new Car();

car.start();
```

A car has an engine.

| Inheritance | Composition |
| --- | --- |
| Establishes an `is-a` relationship | Often establishes a `has-a` relationship |
| Uses `extends` | Combines objects or components |
| Creates a class hierarchy | Combines independent behaviors |
| Can create tight coupling | Often provides greater flexibility |
| Useful for genuine specialization | Useful for reusable independent capabilities |

Prefer composition when objects need to combine several independent behaviors without forming a strict class hierarchy.

# 🗁 Practical Example: Employee Management System

The following example demonstrates inheritance in a small employee management system.

```javascript
class Employee {
  constructor(name, salary) {
    this.name = name;
    this.salary = salary;
  }

  getDetails() {
    return `${this.name} earns $${this.salary}`;
  }

  calculateBonus() {
    return this.salary * 0.05;
  }
}

class Developer extends Employee {
  constructor(name, salary, language) {
    super(name, salary);

    this.language = language;
  }

  writeCode() {
    console.log(`${this.name} writes ${this.language}`);
  }

  calculateBonus() {
    return this.salary * 0.10;
  }
}

class Manager extends Employee {
  constructor(name, salary, teamSize) {
    super(name, salary);

    this.teamSize = teamSize;
  }

  manageTeam() {
    console.log(`${this.name} manages ${this.teamSize} employees`);
  }

  calculateBonus() {
    return this.salary * 0.15;
  }
}

const developer = new Developer(
  "Muhammad",
  5000,
  "JavaScript"
);

const manager = new Manager(
  "Alex",
  7000,
  8
);

console.log(developer.getDetails());
developer.writeCode();
console.log(developer.calculateBonus());

console.log(manager.getDetails());
manager.manageTeam();
console.log(manager.calculateBonus());
```

Output:

```text
Muhammad earns $5000
Muhammad writes JavaScript
500
Alex earns $7000
Alex manages 8 employees
1050
```

Both child classes reuse the common employee functionality while implementing their own specialized behavior.

# 🗁 Common Mistakes

* Forgetting to use `extends` when defining a child class.
* Forgetting to call `super()` before accessing `this` in a derived constructor.
* Assuming inherited methods are copied directly onto instances.
* Trying to extend multiple classes.
* Attempting to access a parent's private fields directly from a child class.
* Overriding a parent method without considering its existing behavior.
* Creating unnecessarily deep inheritance hierarchies.
* Using inheritance when composition would provide a simpler design.
* Assuming static members belong to instances.
* Confusing inheritance with object composition.

# 🗁 Best Practices

* Use inheritance when there is a genuine `is-a` relationship.
* Keep inheritance hierarchies relatively shallow.
* Place shared behavior in parent classes.
* Keep specialized behavior in child classes.
* Use method overriding when a subclass needs different behavior.
* Call `super()` appropriately in derived constructors.
* Avoid exposing internal implementation details unnecessarily.
* Use composition when behavior needs to be combined flexibly.
* Follow the Liskov Substitution Principle: a child class should remain usable wherever its parent class is expected.
* Avoid creating parent classes solely to eliminate a small amount of duplicated code.

# 🗁 Key Takeaways

* Inheritance allows one class to reuse and extend another class's functionality.
* The `extends` keyword establishes class inheritance.
* A parent class is also called a superclass or base class.
* A child class is also called a subclass or derived class.
* Child classes inherit accessible instance methods and properties through the prototype chain.
* Derived constructors must call `super()` before accessing `this`.
* Method overriding allows child classes to customize inherited behavior.
* JavaScript supports single class inheritance, multilevel inheritance, and hierarchical inheritance.
* Static members can also be inherited.
* Private fields cannot be accessed directly by child classes.
* Composition is an alternative to inheritance when combining independent behaviors.
