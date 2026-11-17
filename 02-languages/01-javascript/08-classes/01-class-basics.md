# Class Basics

Classes are templates for creating objects in JavaScript.

They provide a structured way to define an object's properties and methods. Objects created from a class are called instances.

Classes are commonly used to implement object-oriented programming (OOP) concepts such as:

* Encapsulation
* Inheritance
* Abstraction
* Polymorphism

Example:

```javascript
class User {
  constructor(name, email) {
    this.name = name;
    this.email = email;
  }

  introduce() {
    return `Hello, my name is ${this.name}`;
  }
}

const user = new User("Muhammad", "muhammad@example.com");

console.log(user.name);
console.log(user.introduce());
```

Output:

```text
Muhammad
Hello, my name is Muhammad
```

Here:

* `User` is a class.
* `constructor()` initializes new instances.
* `name` and `email` are instance properties.
* `introduce()` is an instance method.
* `user` is an instance of the `User` class.

# 🗁 Why Use Classes?

Before ES6, developers commonly used constructor functions and prototypes to create reusable object structures.

Example using a constructor function:

```javascript
function User(name) {
  this.name = name;
}

User.prototype.introduce = function () {
  return `Hello, ${this.name}`;
};

const user = new User("Muhammad");

console.log(user.introduce());
```

Classes provide a cleaner syntax for expressing the same general object-oriented pattern.

Equivalent class implementation:

```javascript
class User {
  constructor(name) {
    this.name = name;
  }

  introduce() {
    return `Hello, ${this.name}`;
  }
}

const user = new User("Muhammad");

console.log(user.introduce());
```

JavaScript classes are built on top of the prototype system. They do not introduce a separate object model.

# 🗁 Class Declaration

A class declaration defines a named class.

Syntax:

```javascript
class ClassName {
  // Constructor
  // Properties
  // Methods
}
```

Example:

```javascript
class Product {
  constructor(name, price) {
    this.name = name;
    this.price = price;
  }

  getDetails() {
    return `${this.name}: $${this.price}`;
  }
}

const product = new Product("Laptop", 1200);

console.log(product.getDetails());
```

Output:

```text
Laptop: $1200
```

Class names conventionally use PascalCase.

Examples:

```javascript
class User {}
class ShoppingCart {}
class PaymentProcessor {}
class DatabaseConnection {}
```

# 🗁 Creating Class Instances

The `new` operator creates an instance of a class.

```javascript
class User {
  constructor(name) {
    this.name = name;
  }
}

const user1 = new User("Muhammad");
const user2 = new User("Alex");

console.log(user1.name);
console.log(user2.name);
```

Output:

```text
Muhammad
Alex
```

Each instance has its own instance properties.

```javascript
console.log(user1 === user2);
```

Output:

```text
false
```

Both objects were created from the same class, but they are separate objects.

# 🗁 The `constructor` Method

The `constructor()` method initializes a newly created instance.

```javascript
class User {
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }
}

const user = new User("Muhammad", 25);

console.log(user);
```

Output:

```javascript
User {
  name: "Muhammad",
  age: 25
}
```

Important characteristics:

* A class can have only one constructor method.
* The constructor executes automatically when an instance is created with `new`.
* It can receive parameters.
* It commonly initializes instance properties.
* If no constructor is provided, JavaScript supplies a default constructor.

Constructor details will be covered in the next topic.

# 🗁 Instance Properties

Instance properties belong to individual class instances.

They are commonly initialized inside the constructor using `this`.

```javascript
class User {
  constructor(name, email) {
    this.name = name;
    this.email = email;
  }
}

const user1 = new User(
  "Muhammad",
  "muhammad@example.com"
);

const user2 = new User(
  "Alex",
  "alex@example.com"
);

console.log(user1.name);
console.log(user2.name);
```

Output:

```text
Muhammad
Alex
```

Changing one instance's property does not automatically change another instance's property.

```javascript
user1.name = "John";

console.log(user1.name);
console.log(user2.name);
```

Output:

```text
John
Alex
```

# 🗁 Instance Methods

Instance methods define behavior that instances can perform.

```javascript
class Calculator {
  add(a, b) {
    return a + b;
  }

  subtract(a, b) {
    return a - b;
  }
}

const calculator = new Calculator();

console.log(calculator.add(10, 5));
console.log(calculator.subtract(10, 5));
```

Output:

```text
15
5
```

Instance methods are generally stored on the class's prototype rather than copied into every instance.

```javascript
console.log(
  Object.hasOwn(calculator, "add")
);

console.log(
  Object.hasOwn(Calculator.prototype, "add")
);
```

Output:

```text
false
true
```

This allows instances to share the same method implementation.

# 🗁 Class Expressions

A class can also be defined as an expression and assigned to a variable.

```javascript
const User = class {
  constructor(name) {
    this.name = name;
  }

  introduce() {
    return `Hello, ${this.name}`;
  }
};

const user = new User("Muhammad");

console.log(user.introduce());
```

Output:

```text
Hello, Muhammad
```

Unlike a class declaration, a class expression does not have to be assigned to a variable.

# 🗁 Named Class Expressions

A class expression can have an internal name.

```javascript
const User = class UserModel {
  constructor(name) {
    this.name = name;
  }

  introduce() {
    return `Hello, ${this.name}`;
  }
};

const user = new User("Muhammad");

console.log(user.introduce());
```

The internal name `UserModel` is available inside the class body but is not available as an outer variable.

```javascript
console.log(typeof User);
console.log(typeof UserModel);
```

Output:

```text
function
undefined
```

The class is assigned to the outer variable `User`.

# 🗁 Classes Are Not Hoisted Like Functions

Function declarations can be used before their declaration because they are hoisted.

```javascript
sayHello();

function sayHello() {
  console.log("Hello");
}
```

Output:

```text
Hello
```

Class declarations behave differently.

```javascript
const user = new User();

class User {}
```

This throws a `ReferenceError`.

A class declaration exists in its surrounding scope, but it cannot be accessed before its declaration is evaluated. It remains in the temporal dead zone until then.

Correct implementation:

```javascript
class User {}

const user = new User();

console.log(user instanceof User);
```

Output:

```text
true
```

# 🗁 Classes Use Strict Mode

The body of a JavaScript class automatically executes in strict mode.

You do not need to write `"use strict"` inside a class.

```javascript
class Example {
  test() {
    undeclaredVariable = 10;
  }
}

const example = new Example();

example.test();
```

This throws a `ReferenceError` because assigning to an undeclared variable is not allowed in strict mode.

This behavior applies to class constructors and methods.

# 🗁 Classes Are Special Functions

JavaScript classes are internally represented as functions.

```javascript
class User {}

console.log(typeof User);
```

Output:

```text
function
```

However, classes cannot be called like ordinary functions.

```javascript
class User {}

User();
```

This throws a `TypeError`.

Classes must be invoked with the `new` operator.

```javascript
const user = new User();

console.log(user instanceof User);
```

Output:

```text
true
```

# 🗁 The `instanceof` Operator

The `instanceof` operator checks whether an object's prototype chain contains the prototype of a constructor or class.

```javascript
class User {}

const user = new User();

console.log(user instanceof User);
console.log(user instanceof Object);
```

Output:

```text
true
true
```

The `User` instance is also an instance of `Object` because its prototype chain eventually reaches `Object.prototype`.

For ordinary class instances, `instanceof` is useful for checking their class relationship.

# 🗁 Class Methods and Prototype

Methods defined in a class body are placed on the class's prototype.

```javascript
class User {
  introduce() {
    return "Hello";
  }
}

const user1 = new User();
const user2 = new User();

console.log(user1.introduce === user2.introduce);
```

Output:

```text
true
```

Both instances access the same method through their prototype.

You can inspect the relationship:

```javascript
console.log(
  Object.getPrototypeOf(user1) === User.prototype
);
```

Output:

```text
true
```

This is the foundation of class-based inheritance in JavaScript.

# 🗁 Class Fields

Modern JavaScript supports declaring instance fields directly inside a class body.

```javascript
class User {
  name = "Muhammad";
  age = 25;
}

const user = new User();

console.log(user.name);
console.log(user.age);
```

Output:

```text
Muhammad
25
```

These fields are initialized on each new instance.

They are not shared through the prototype.

```javascript
const user1 = new User();
const user2 = new User();

console.log(
  Object.hasOwn(user1, "name")
);

console.log(
  Object.hasOwn(user2, "name")
);
```

Output:

```text
true
true
```

Class fields can also use constructor-independent initialization.

```javascript
class ShoppingCart {
  items = [];
  total = 0;

  addItem(item) {
    this.items.push(item);
    this.total += item.price;
  }
}

const cart = new ShoppingCart();

cart.addItem({
  name: "Keyboard",
  price: 50
});

console.log(cart.total);
```

Output:

```text
50
```

# 🗁 Instance Fields vs Prototype Methods

Instance fields and prototype methods have different storage behavior.

```javascript
class User {
  name = "Muhammad";

  introduce() {
    return `Hello, ${this.name}`;
  }
}

const user1 = new User();
const user2 = new User();

console.log(
  Object.hasOwn(user1, "name")
);

console.log(
  Object.hasOwn(user1, "introduce")
);

console.log(
  user1.introduce === user2.introduce
);
```

Output:

```text
true
false
true
```

Explanation:

* `name` is an own property of each instance.
* `introduce()` is inherited from `User.prototype`.
* Both instances share the same `introduce()` method.

# 🗁 Static Members Preview

Classes can also define static methods and properties that belong to the class itself rather than its instances.

```javascript
class MathHelper {
  static add(a, b) {
    return a + b;
  }
}

console.log(MathHelper.add(10, 20));
```

Output:

```text
30
```

Static methods are accessed through the class name.

```javascript
const helper = new MathHelper();

console.log(helper.add);
```

Output:

```text
undefined
```

Static methods and properties are covered in a dedicated topic.

# 🗁 Practical Example: User Management

A class can represent a reusable user model.

```javascript
class User {
  constructor(id, name, email) {
    this.id = id;
    this.name = name;
    this.email = email;
  }

  getProfile() {
    return {
      id: this.id,
      name: this.name,
      email: this.email
    };
  }

  updateEmail(email) {
    this.email = email;
  }
}

const user = new User(
  1,
  "Muhammad",
  "muhammad@example.com"
);

console.log(user.getProfile());

user.updateEmail("new@example.com");

console.log(user.email);
```

Output:

```javascript
{
  id: 1,
  name: "Muhammad",
  email: "muhammad@example.com"
}

new@example.com
```

This example demonstrates how classes combine data and behavior into reusable object structures.

# 🗁 Common Mistakes

**Creating an instance without `new`**

```javascript
class User {}

const user = User();
```

This throws a `TypeError`.

Use:

```javascript
const user = new User();
```

**Confusing a class with an instance**

```javascript
class User {}

const user = new User();
```

`User` is the class, while `user` is an instance.

**Assuming instance methods are copied into every object**

Normal class methods are stored on the prototype and shared by instances.

**Assuming class declarations are hoisted like function declarations**

Classes cannot be accessed before their declaration is evaluated.

**Using arrow functions for every class method**

Arrow-function class fields create a separate function on each instance. Ordinary class methods are generally more memory-efficient when shared behavior is sufficient.

**Forgetting that class bodies use strict mode**

Class code automatically follows strict-mode rules.

# 🗁 Best Practices

* Use PascalCase for class names.
* Use classes when objects share a meaningful structure and behavior.
* Keep constructors focused on initializing instance state.
* Use instance methods for behavior shared by instances.
* Use class fields for instance-specific state.
* Use static members for behavior or data associated with the class itself.
* Prefer composition when inheritance would create unnecessarily complex class hierarchies.
* Keep classes focused on a clear responsibility.
* Avoid creating classes for simple data structures when ordinary objects are sufficient.
* Understand the prototype system because JavaScript classes rely on it.

# 🗁 Key Takeaways

* A class is a template for creating objects.
* Objects created from a class are called instances.
* The `class` keyword defines a class.
* The `new` operator creates class instances.
* The constructor initializes newly created instances.
* Instance properties store object-specific data.
* Instance methods define reusable behavior.
* Class expressions provide an alternative way to define classes.
* Class declarations cannot be accessed before their declaration is evaluated.
* Class bodies automatically execute in strict mode.
* JavaScript classes are built on top of prototypes.
* Class fields create properties on individual instances.
* Static members belong to the class rather than individual instances.
