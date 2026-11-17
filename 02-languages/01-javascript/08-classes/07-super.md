# The super Keyword

The `super` keyword in JavaScript is used inside classes to access the parent class's constructor and methods.

It is primarily used in inheritance when a child class needs to:

* Call the parent class constructor.
* Reuse a parent class method.
* Override a parent method while preserving its original behavior.
* Access inherited static methods.

The `super` keyword has two primary forms:

* `super()` — calls the parent class constructor.
* `super.method()` — calls a method from the parent class.

Example:

```javascript
class Animal {
  constructor(name) {
    this.name = name;
  }

  eat() {
    console.log(`${this.name} is eating`);
  }
}

class Dog extends Animal {
  constructor(name, breed) {
    super(name);

    this.breed = breed;
  }

  bark() {
    console.log(`${this.name} is barking`);
  }
}

const dog = new Dog("Buddy", "Golden Retriever");

dog.eat();
dog.bark();
```

Output:

```text
Buddy is eating
Buddy is barking
```

Here, `super(name)` invokes the `Animal` constructor and initializes the inherited `name` property.

# 🗁 Calling the Parent Constructor with super()

When a child class defines its own constructor, it must call `super()` before accessing `this`.

Syntax:

```javascript
class Parent {
  constructor(value) {
    this.value = value;
  }
}

class Child extends Parent {
  constructor(value, additionalValue) {
    super(value);

    this.additionalValue = additionalValue;
  }
}
```

Example:

```javascript
class Person {
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }
}

class Developer extends Person {
  constructor(name, age, language) {
    super(name, age);

    this.language = language;
  }
}

const developer = new Developer(
  "Muhammad",
  25,
  "JavaScript"
);

console.log(developer.name);
console.log(developer.age);
console.log(developer.language);
```

Output:

```text
Muhammad
25
JavaScript
```

The parent constructor initializes `name` and `age`, while the child constructor initializes `language`.

# 🗁 Why super() Must Be Called Before this

In a derived class constructor, JavaScript does not initialize `this` until the parent constructor has been called.

Incorrect:

```javascript
class Parent {
  constructor(name) {
    this.name = name;
  }
}

class Child extends Parent {
  constructor(name) {
    this.name = name;

    super();
  }
}
```

This throws a `ReferenceError` because `this` is accessed before `super()`.

Correct:

```javascript
class Parent {
  constructor(name) {
    this.name = name;
  }
}

class Child extends Parent {
  constructor(name) {
    super(name);

    console.log(this.name);
  }
}

const child = new Child("Muhammad");
```

Output:

```text
Muhammad
```

Calling `super()` initializes the derived instance and makes `this` available.

# 🗁 Using super() Without Arguments

If the parent constructor does not require arguments, `super()` can be called without arguments.

```javascript
class Animal {
  constructor() {
    this.alive = true;
  }
}

class Dog extends Animal {
  constructor(name) {
    super();

    this.name = name;
  }
}

const dog = new Dog("Buddy");

console.log(dog.alive);
console.log(dog.name);
```

Output:

```text
true
Buddy
```

The parent constructor initializes `alive`, and the child constructor initializes `name`.

# 🗁 Calling Parent Methods with super.method()

The `super` keyword can access methods defined in the parent class.

Syntax:

```javascript
super.methodName();
```

Example:

```javascript
class Animal {
  eat() {
    console.log("Animal is eating");
  }
}

class Dog extends Animal {
  eat() {
    super.eat();

    console.log("Dog is eating dog food");
  }
}

const dog = new Dog();

dog.eat();
```

Output:

```text
Animal is eating
Dog is eating dog food
```

The child class overrides `eat()` but still executes the parent implementation through `super.eat()`.

# 🗁 Extending Parent Method Functionality

A child class can call a parent method and then add additional behavior.

```javascript
class Employee {
  constructor(name) {
    this.name = name;
  }

  getDetails() {
    return `Employee: ${this.name}`;
  }
}

class Developer extends Employee {
  constructor(name, language) {
    super(name);

    this.language = language;
  }

  getDetails() {
    const parentDetails = super.getDetails();

    return `${parentDetails}, Language: ${this.language}`;
  }
}

const developer = new Developer("Muhammad", "JavaScript");

console.log(developer.getDetails());
```

Output:

```text
Employee: Muhammad, Language: JavaScript
```

This approach avoids duplicating the parent method's existing logic.

# 🗁 Using super with Method Arguments

Arguments can be passed to parent methods normally.

```javascript
class Calculator {
  add(a, b) {
    return a + b;
  }
}

class AdvancedCalculator extends Calculator {
  calculate(a, b) {
    const result = super.add(a, b);

    console.log(`Result: ${result}`);

    return result;
  }
}

const calculator = new AdvancedCalculator();

calculator.calculate(10, 20);
```

Output:

```text
Result: 30
```

The child class reuses the parent's `add()` method.

# 🗁 Using super with Getters and Setters

The `super` keyword can access getters and setters inherited from a parent class.

Example using a getter:

```javascript
class Person {
  constructor(name) {
    this._name = name;
  }

  get name() {
    return this._name;
  }
}

class Developer extends Person {
  get name() {
    return `Developer: ${super.name}`;
  }
}

const developer = new Developer("Muhammad");

console.log(developer.name);
```

Output:

```text
Developer: Muhammad
```

Example using a setter:

```javascript
class Person {
  constructor(name) {
    this._name = name;
  }

  get name() {
    return this._name;
  }

  set name(value) {
    if (!value.trim()) {
      throw new TypeError("Name cannot be empty");
    }

    this._name = value;
  }
}

class Developer extends Person {
  set name(value) {
    super.name = value.trim();
  }

  get name() {
    return super.name;
  }
}

const developer = new Developer("Muhammad");

developer.name = "  Alex  ";

console.log(developer.name);
```

Output:

```text
Alex
```

Using `super.name` invokes the inherited accessor instead of accessing the underlying property directly.

# 🗁 Using super with Static Methods

Static methods belong to classes rather than instances.

A child class can use `super` to call an inherited static method.

```javascript
class User {
  static getRole() {
    return "User";
  }
}

class Admin extends User {
  static getRole() {
    const parentRole = super.getRole();

    return `${parentRole} and Administrator`;
  }
}

console.log(Admin.getRole());
```

Output:

```text
User and Administrator
```

Here, `super.getRole()` accesses the parent class's static method.

# 🗁 Using super with Static Properties

The `super` keyword can also access inherited static properties.

```javascript
class Application {
  static environment = "development";
}

class AdminApplication extends Application {
  static displayEnvironment() {
    return super.environment;
  }
}

console.log(AdminApplication.displayEnvironment());
```

Output:

```text
development
```

Static property access through `super` follows the parent constructor's prototype chain.

# 🗁 Understanding super and this

Although `super` accesses parent-class behavior, it does not create a separate parent object.

When a parent method is called using `super.method()`, its `this` value refers to the current child instance.

Example:

```javascript
class Parent {
  display() {
    console.log(this.name);
  }
}

class Child extends Parent {
  constructor(name) {
    super();

    this.name = name;
  }

  show() {
    super.display();
  }
}

const child = new Child("Muhammad");

child.show();
```

Output:

```text
Muhammad
```

The parent method executes with the child instance as its `this` value.

This allows inherited methods to access properties initialized by child classes.

# 🗁 super and the Prototype Chain

The behavior of `super` is connected to JavaScript's prototype system.

In instance methods, `super.method()` looks up the method through the parent prototype.

In static methods, `super.method()` looks up the method through the parent constructor.

Example:

```javascript
class Animal {
  eat() {
    console.log("Eating...");
  }
}

class Dog extends Animal {
  eat() {
    console.log("Before eating");

    super.eat();

    console.log("After eating");
  }
}

const dog = new Dog();

dog.eat();
```

Output:

```text
Before eating
Eating...
After eating
```

The parent method is resolved through the inheritance chain, while `this` remains bound to the current instance.

# 🗁 Default Constructors and super()

When a derived class does not define a constructor, JavaScript provides a default constructor that forwards its arguments to the parent constructor.

Conceptually, the default derived constructor behaves like this:

```javascript
constructor(...args) {
  super(...args);
}
```

Example:

```javascript
class Person {
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }
}

class Developer extends Person {}

const developer = new Developer("Muhammad", 25);

console.log(developer.name);
console.log(developer.age);
```

Output:

```text
Muhammad
25
```

The automatically provided constructor forwards the arguments to `Person`.

# 🗁 super in Regular Functions

The `super` keyword is not available in ordinary functions.

Incorrect:

```javascript
function example() {
  super.someMethod();
}
```

This produces a syntax error.

`super` is valid in specific class methods, constructors, and object methods that have the required method context.

# 🗁 super in Arrow Functions

Arrow functions do not define their own `super` binding.

However, an arrow function created inside a class method can access the surrounding method's `super` binding.

Example:

```javascript
class Parent {
  display() {
    console.log("Parent method");
  }
}

class Child extends Parent {
  execute() {
    const callback = () => {
      super.display();
    };

    callback();
  }
}

const child = new Child();

child.execute();
```

Output:

```text
Parent method
```

The arrow function uses the lexical `super` binding from `execute()`.

# 🗁 super in Object Literals

The `super` keyword is also supported inside object methods.

```javascript
const parent = {
  greet() {
    console.log("Hello from parent");
  }
};

const child = {
  greet() {
    super.greet();

    console.log("Hello from child");
  }
};

Object.setPrototypeOf(child, parent);

child.greet();
```

Output:

```text
Hello from parent
Hello from child
```

Object methods use their prototype relationship to resolve `super` references.

# 🗁 Common Mistakes

* Forgetting to call `super()` inside a derived constructor.
* Accessing `this` before calling `super()`.
* Confusing `super()` with `super.method()`.
* Assuming `super` refers to a separate parent object.
* Assuming parent methods execute with the parent instance as `this`.
* Trying to use `super` inside ordinary functions.
* Calling a parent method without considering whether it exists.
* Assuming `super` always refers to the immediate parent class's implementation regardless of the prototype chain.
* Forgetting that static and instance methods use different inheritance chains.

# 🗁 Best Practices

* Call `super()` before accessing `this` in derived constructors.
* Pass only the arguments required by the parent constructor.
* Use `super.method()` when extending existing parent behavior.
* Avoid duplicating parent functionality in child classes.
* Keep overridden methods compatible with their parent method contracts.
* Use `super` to reuse parent accessors when appropriate.
* Understand the difference between static and instance inheritance.
* Avoid unnecessary inheritance levels.
* Prefer composition when parent-child relationships do not represent the actual domain model.

# 🗁 Key Takeaways

* `super` provides access to parent-class behavior.
* `super()` invokes the parent constructor.
* `super.method()` invokes an inherited method.
* A derived constructor must call `super()` before accessing `this`.
* Parent methods called through `super` use the current child instance as `this`.
* `super` can access inherited getters, setters, static methods, and static properties.
* Default derived constructors automatically forward arguments to the parent constructor.
* Arrow functions can access a surrounding method's lexical `super` binding.
* `super` is also available in object literal methods.
* Understanding `super` is essential for implementing and extending class inheritance correctly.
