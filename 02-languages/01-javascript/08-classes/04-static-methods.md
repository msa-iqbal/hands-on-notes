# Static Methods

Static methods are methods defined inside a JavaScript class that belong to the class itself rather than individual instances.

They are commonly used for functionality that relates to the class as a whole and does not require access to a particular instance's state.

Example:

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

Here:

* `MathHelper` is the class.
* `add()` is a static method.
* `static` declares the method as a static member.
* The method is called directly through the class name.

# 🗁 Defining Static Methods

Use the `static` keyword before a method name to define a static method.

Syntax:

```javascript
class ClassName {
  static methodName(parameters) {
    // Method implementation
  }
}
```

Example:

```javascript
class Calculator {
  static add(a, b) {
    return a + b;
  }

  static subtract(a, b) {
    return a - b;
  }

  static multiply(a, b) {
    return a * b;
  }
}

console.log(Calculator.add(10, 5));
console.log(Calculator.subtract(10, 5));
console.log(Calculator.multiply(10, 5));
```

Output:

```text
15
5
50
```

# 🗁 Calling Static Methods

Static methods are called using the class name.

```javascript
class User {
  static getApplicationName() {
    return "User Management System";
  }
}

console.log(User.getApplicationName());
```

Output:

```text
User Management System
```

You do not need to create an instance to call a static method.

# 🗁 Static Methods Cannot Be Called Through Instances

Static methods belong to the class, not its instances.

```javascript
class User {
  static getRole() {
    return "Administrator";
  }
}

const user = new User();

console.log(User.getRole());
console.log(user.getRole);
```

Output:

```text
Administrator
undefined
```

Calling the method through the instance throws a `TypeError`:

```javascript
user.getRole();
```

The method is not available as an instance method.

# 🗁 Static Methods and the `this` Keyword

Inside a static method, `this` normally refers to the class through which the method was called.

```javascript
class User {
  static applicationName = "User Management System";

  static getApplicationName() {
    return this.applicationName;
  }
}

console.log(User.getApplicationName());
```

Output:

```text
User Management System
```

The `this` keyword refers to `User` in this example.

Static methods can access other static properties and methods through `this`.

# 🗁 Calling Other Static Methods

A static method can call another static method using `this`.

```javascript
class Calculator {
  static add(a, b) {
    return a + b;
  }

  static calculate(a, b) {
    return this.add(a, b);
  }
}

console.log(Calculator.calculate(10, 20));
```

Output:

```text
30
```

This helps organize related static functionality into reusable methods.

# 🗁 Static Properties

JavaScript classes also support static properties.

Static properties belong to the class itself rather than individual instances.

```javascript
class User {
  static count = 0;

  constructor(name) {
    this.name = name;
    User.count++;
  }
}

const user1 = new User("Muhammad");
const user2 = new User("Alex");

console.log(User.count);
```

Output:

```text
2
```

The `count` property is shared at the class level.

It is not automatically available through instances.

```javascript
console.log(user1.count);
```

Output:

```text
undefined
```

# 🗁 Static Methods Accessing Static Properties

Static methods can access and modify static properties.

```javascript
class User {
  static count = 0;

  static incrementCount() {
    this.count++;
  }

  static getCount() {
    return this.count;
  }
}

User.incrementCount();
User.incrementCount();

console.log(User.getCount());
```

Output:

```text
2
```

This pattern is useful for class-level counters, shared configuration, and utility operations.

# 🗁 Static Methods and Instance Methods

Static methods and instance methods have different access patterns.

```javascript
class User {
  constructor(name) {
    this.name = name;
  }

  introduce() {
    return `Hello, ${this.name}`;
  }

  static createGuest() {
    return new User("Guest");
  }
}

const user = User.createGuest();

console.log(user.introduce());
```

Output:

```text
Hello, Guest
```

The static method creates and returns an instance of the class.

Comparison:

| Feature | Instance Method | Static Method |
| --- | --- | --- |
| Belongs to | Class prototype | Class itself |
| Called through | Instance | Class |
| Accesses instance state through `this` | Yes | Not directly |
| Accesses static members through `this` | Not normally | Yes |
| Shared implementation | Yes | Yes |

# 🗁 Static Methods and Inheritance

Static methods are inherited by subclasses through the constructor's prototype chain.

```javascript
class Animal {
  static category() {
    return "Animal";
  }
}

class Dog extends Animal {}

console.log(Dog.category());
```

Output:

```text
Animal
```

Although `Dog` does not define `category()`, it inherits the static method from `Animal`.

# 🗁 The `this` Keyword in Inherited Static Methods

When an inherited static method is called through a subclass, `this` refers to the subclass.

```javascript
class Animal {
  static getType() {
    return this.name;
  }
}

class Dog extends Animal {}

console.log(Animal.getType());
console.log(Dog.getType());
```

Output:

```text
Animal
Dog
```

This behavior is useful for creating reusable static methods that work with subclasses.

# 🗁 Overriding Static Methods

A subclass can define its own static method with the same name as a parent class's static method.

```javascript
class Animal {
  static category() {
    return "General animal";
  }
}

class Dog extends Animal {
  static category() {
    return "Domestic animal";
  }
}

console.log(Animal.category());
console.log(Dog.category());
```

Output:

```text
General animal
Domestic animal
```

The subclass implementation takes precedence when the method is called through the subclass.

# 🗁 Calling Parent Static Methods with `super`

Inside a static method, `super` can access static methods defined on the parent class.

```javascript
class Animal {
  static describe() {
    return "This is an animal";
  }
}

class Dog extends Animal {
  static describe() {
    return `${super.describe()} and it is a dog`;
  }
}

console.log(Dog.describe());
```

Output:

```text
This is an animal and it is a dog
```

This allows subclasses to extend parent class-level behavior.

# 🗁 Static Initialization Blocks

JavaScript supports static initialization blocks for performing more complex class-level initialization.

They execute once when the class is evaluated.

```javascript
class Application {
  static configuration;

  static {
    this.configuration = {
      environment: "production",
      debug: false
    };
  }
}

console.log(Application.configuration);
```

Output:

```javascript
{
  environment: "production",
  debug: false
}
```

Static initialization blocks can:

* Initialize multiple static properties.
* Perform conditional setup.
* Execute initialization logic.
* Access private static fields and methods.

Example:

```javascript
class Database {
  static connection;

  static {
    const environment = "production";

    if (environment === "production") {
      this.connection = "Production Database";
    } else {
      this.connection = "Development Database";
    }
  }
}

console.log(Database.connection);
```

Output:

```text
Production Database
```

A class can contain multiple static initialization blocks. They execute in the order they appear in the class body.

# 🗁 Static Methods vs Utility Functions

Static methods are useful when a function is conceptually associated with a class.

Example using a static method:

```javascript
class StringHelper {
  static capitalize(value) {
    return value.charAt(0).toUpperCase() +
      value.slice(1);
  }
}

console.log(StringHelper.capitalize("javascript"));
```

Output:

```text
Javascript
```

Equivalent standalone utility function:

```javascript
function capitalize(value) {
  return value.charAt(0).toUpperCase() +
    value.slice(1);
}

console.log(capitalize("javascript"));
```

Both approaches work.

Use static methods when the functionality logically belongs to the class. Use standalone functions when the functionality is independent of a class.

# 🗁 Practical Example: User Factory

A static method can provide a convenient way to create predefined instances.

```javascript
class User {
  constructor(name, role) {
    this.name = name;
    this.role = role;
  }

  static createAdmin(name) {
    return new User(name, "Administrator");
  }

  static createGuest() {
    return new User("Guest", "Visitor");
  }

  getDetails() {
    return {
      name: this.name,
      role: this.role
    };
  }
}

const admin = User.createAdmin("Muhammad");
const guest = User.createGuest();

console.log(admin.getDetails());
console.log(guest.getDetails());
```

Output:

```javascript
{
  name: "Muhammad",
  role: "Administrator"
}

{
  name: "Guest",
  role: "Visitor"
}
```

The static methods act as factory methods for creating instances.

# 🗁 Practical Example: Shopping Cart

Static methods can provide operations that do not depend on a particular shopping cart instance.

```javascript
class ShoppingCart {
  constructor() {
    this.items = [];
  }

  addItem(item) {
    this.items.push(item);
  }

  getTotal() {
    return ShoppingCart.calculateTotal(this.items);
  }

  static calculateTotal(items) {
    return items.reduce(
      (total, item) => total + item.price,
      0
    );
  }
}

const cart = new ShoppingCart();

cart.addItem({
  name: "Keyboard",
  price: 80
});

cart.addItem({
  name: "Mouse",
  price: 30
});

console.log(cart.getTotal());

console.log(
  ShoppingCart.calculateTotal([
    { price: 100 },
    { price: 200 }
  ])
);
```

Output:

```text
110
300
```

The instance method `getTotal()` uses the current cart's items, while the static method `calculateTotal()` operates on any supplied array of items.

# 🗁 Practical Example: Data Validation

Static methods can group validation functions related to a particular model.

```javascript
class User {
  constructor(name, email) {
    this.name = name;
    this.email = email;
  }

  static isValidEmail(email) {
    return /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(email);
  }

  static create(name, email) {
    if (!this.isValidEmail(email)) {
      throw new TypeError("Invalid email address");
    }

    return new User(name, email);
  }
}

const user = User.create(
  "Muhammad",
  "muhammad@example.com"
);

console.log(user);
```

Output:

```javascript
User {
  name: "Muhammad",
  email: "muhammad@example.com"
}
```

The static `create()` method validates input before returning a new instance.

# 🗁 Common Mistakes

**Calling static methods through instances**

Static methods belong to the class, not its instances.

**Trying to access instance properties directly**

A static method does not automatically have access to a particular instance's state.

**Confusing static properties with instance properties**

Static properties are stored on the class constructor, while instance properties belong to individual objects.

**Assuming static methods are not inherited**

Static methods are inherited by subclasses through the constructor prototype chain.

**Using `this` incorrectly**

Inside a static method, `this` normally refers to the class used to invoke the method, which may be a subclass.

**Making every utility function static**

Static methods should represent behavior that logically belongs to the class. Independent functionality may be better expressed as standalone functions.

# 🗁 Best Practices

* Use static methods for functionality that does not depend on individual instance state.
* Use static properties for class-level data.
* Use factory methods when a class needs convenient instance creation patterns.
* Keep static utility methods logically related to their class.
* Use `this` when static methods should support inheritance and subclassing.
* Use `super` when extending parent static behavior.
* Avoid using static state for data that should belong to individual instances.
* Prefer standalone functions when functionality does not logically belong to a class.
* Keep static initialization blocks focused on class-level setup.

# 🗁 Key Takeaways

* Static methods belong to the class itself rather than individual instances.
* The `static` keyword defines a static method.
* Static methods are called through the class name.
* Static properties store class-level data.
* Static methods can access static properties and other static methods through `this`.
* Static methods are inherited by subclasses.
* Subclasses can override inherited static methods.
* `super` can call parent static methods.
* Static initialization blocks perform one-time class-level initialization.
* Static factory methods can provide convenient ways to create instances.
* Static methods are useful for utility operations, validation, factories, and class-level behavior.
