# Getters and Setters

Getters and setters are special methods in JavaScript classes and objects that allow you to control how properties are accessed and modified.

* A **getter** executes when a property is read.
* A **setter** executes when a value is assigned to a property.

They allow you to implement computed properties, validation, controlled updates, and encapsulation.

Example:

```javascript
class User {
  constructor(firstName, lastName) {
    this.firstName = firstName;
    this.lastName = lastName;
  }

  get fullName() {
    return `${this.firstName} ${this.lastName}`;
  }

  set fullName(value) {
    const [firstName, ...lastName] = value.split(" ");

    this.firstName = firstName;
    this.lastName = lastName.join(" ");
  }
}

const user = new User("Muhammad", "Iqbal");

console.log(user.fullName);

user.fullName = "Alex Morgan";

console.log(user.firstName);
console.log(user.lastName);
```

Output:

```text
Muhammad Iqbal
Alex
Morgan
```

The getter calculates `fullName` when accessed, while the setter updates the underlying properties when a new value is assigned.

# 🗁 Getter Syntax

A getter is defined using the `get` keyword before a method name.

Syntax:

```javascript
class ClassName {
  get propertyName() {
    return value;
  }
}
```

Example:

```javascript
class Product {
  constructor(name, price) {
    this.name = name;
    this.price = price;
  }

  get details() {
    return `${this.name}: $${this.price}`;
  }
}

const product = new Product("Laptop", 1200);

console.log(product.details);
```

Output:

```text
Laptop: $1200
```

Notice that the getter is accessed like a property:

```javascript
product.details;
```

It is not called like an ordinary method:

```javascript
product.details();
```

Calling it with parentheses throws a `TypeError` because the getter returns a value rather than a function.

# 🗁 Setter Syntax

A setter is defined using the `set` keyword before a method name.

Syntax:

```javascript
class ClassName {
  set propertyName(value) {
    // Handle the assigned value
  }
}
```

Example:

```javascript
class User {
  set name(value) {
    this._name = value;
  }
}

const user = new User();

user.name = "Muhammad";

console.log(user._name);
```

Output:

```text
Muhammad
```

The setter executes when a value is assigned to `user.name`.

A setter must accept exactly one parameter.

# 🗁 Using Getters and Setters Together

Getters and setters are commonly used together to provide controlled access to an underlying property.

```javascript
class User {
  constructor(name) {
    this._name = name;
  }

  get name() {
    return this._name;
  }

  set name(value) {
    this._name = value;
  }
}

const user = new User("Muhammad");

console.log(user.name);

user.name = "Alex";

console.log(user.name);
```

Output:

```text
Muhammad
Alex
```

Here:

* `get name()` handles property access.
* `set name(value)` handles property assignment.
* `_name` stores the actual value.

The underscore is a naming convention indicating that `_name` is intended for internal use. It does not make the property private.

# 🗁 Getters for Computed Properties

A getter can calculate a value dynamically instead of storing it directly.

```javascript
class Rectangle {
  constructor(width, height) {
    this.width = width;
    this.height = height;
  }

  get area() {
    return this.width * this.height;
  }

  get perimeter() {
    return 2 * (this.width + this.height);
  }
}

const rectangle = new Rectangle(10, 5);

console.log(rectangle.area);
console.log(rectangle.perimeter);
```

Output:

```text
50
30
```

The values are calculated whenever their getters are accessed.

If the underlying dimensions change, the computed values automatically reflect the new state.

```javascript
rectangle.width = 20;

console.log(rectangle.area);
```

Output:

```text
100
```

This avoids storing redundant calculated values that could become inconsistent.

# 🗁 Setters for Validation

A setter can validate incoming values before updating an object's state.

```javascript
class Product {
  constructor(price) {
    this.price = price;
  }

  get price() {
    return this._price;
  }

  set price(value) {
    if (typeof value !== "number" || !Number.isFinite(value)) {
      throw new TypeError("Price must be a finite number");
    }

    if (value < 0) {
      throw new RangeError("Price cannot be negative");
    }

    this._price = value;
  }
}

const product = new Product(100);

product.price = 250;

console.log(product.price);
```

Output:

```text
250
```

Invalid assignment is rejected:

```javascript
product.price = -50;
```

This throws a `RangeError`.

The constructor also uses the setter because assigning `this.price` invokes the setter.

This ensures that both initial values and later updates follow the same validation rules.

# 🗁 Avoiding Recursive Getters

A common mistake is returning the same property from inside its getter.

Incorrect:

```javascript
class User {
  get name() {
    return this.name;
  }
}

const user = new User();

console.log(user.name);
```

This causes infinite recursion because reading `this.name` invokes the same getter again.

Eventually, JavaScript throws a `RangeError` due to maximum call stack size being exceeded.

Correct implementation:

```javascript
class User {
  constructor(name) {
    this._name = name;
  }

  get name() {
    return this._name;
  }
}

const user = new User("Muhammad");

console.log(user.name);
```

Output:

```text
Muhammad
```

Use a separate backing property or a private field to store the underlying value.

# 🗁 Avoiding Recursive Setters

A setter can also cause infinite recursion if it assigns a value to the same property it manages.

Incorrect:

```javascript
class User {
  set name(value) {
    this.name = value;
  }
}
```

The assignment invokes the setter again, creating infinite recursion.

Correct implementation:

```javascript
class User {
  set name(value) {
    this._name = value;
  }

  get name() {
    return this._name;
  }
}

const user = new User();

user.name = "Muhammad";

console.log(user.name);
```

Output:

```text
Muhammad
```

# 🗁 Getters and Setters with Private Fields

JavaScript private fields can be used as backing storage for getters and setters.

Private fields are declared with the `#` prefix and are accessible only inside the class body.

```javascript
class BankAccount {
  #balance = 0;

  get balance() {
    return this.#balance;
  }

  set balance(value) {
    if (typeof value !== "number" || !Number.isFinite(value)) {
      throw new TypeError("Balance must be a finite number");
    }

    if (value < 0) {
      throw new RangeError("Balance cannot be negative");
    }

    this.#balance = value;
  }
}

const account = new BankAccount();

account.balance = 1000;

console.log(account.balance);
```

Output:

```text
1000
```

Direct access to the private field is not allowed:

```javascript
console.log(account.#balance);
```

This causes a `SyntaxError` outside the class body.

Private fields provide stronger encapsulation than underscore-prefixed properties.

# 🗁 Getters Without Setters

A class can define a getter without defining a corresponding setter.

```javascript
class Circle {
  constructor(radius) {
    this.radius = radius;
  }

  get area() {
    return Math.PI * this.radius ** 2;
  }
}

const circle = new Circle(5);

console.log(circle.area);
```

Output:

```text
78.53981633974483
```

Attempting to assign to `area` does not update the calculated value.

```javascript
circle.area = 100;
```

In strict mode, this throws a `TypeError` because the property has no setter.

Class bodies automatically execute in strict mode.

# 🗁 Setters Without Getters

A setter can exist without a corresponding getter.

```javascript
class User {
  set name(value) {
    this._name = value;
  }
}

const user = new User();

user.name = "Muhammad";

console.log(user.name);
console.log(user._name);
```

Output:

```text
undefined
Muhammad
```

The setter accepts and stores the assigned value, but reading `user.name` returns `undefined` because no getter exists.

# 🗁 Getters and Setters in Object Literals

Getters and setters are not limited to classes. They can also be defined inside ordinary object literals.

```javascript
const user = {
  firstName: "Muhammad",
  lastName: "Iqbal",

  get fullName() {
    return `${this.firstName} ${this.lastName}`;
  },

  set fullName(value) {
    const [firstName, ...lastName] = value.split(" ");

    this.firstName = firstName;
    this.lastName = lastName.join(" ");
  }
};

console.log(user.fullName);

user.fullName = "Alex Morgan";

console.log(user.fullName);
```

Output:

```text
Muhammad Iqbal
Alex Morgan
```

The behavior is similar to getters and setters defined inside classes.

# 🗁 Defining Getters and Setters with Object.defineProperty()

You can define accessor properties programmatically using `Object.defineProperty()`.

```javascript
const user = {
  _name: "Muhammad"
};

Object.defineProperty(user, "name", {
  get() {
    return this._name;
  },

  set(value) {
    this._name = value;
  },

  enumerable: true,
  configurable: true
});

console.log(user.name);

user.name = "Alex";

console.log(user.name);
```

Output:

```text
Muhammad
Alex
```

This approach is useful when you need to add accessor behavior to an existing object.

# 🗁 Getters and Setters Inheritance

Getters and setters defined in a parent class can be inherited by subclasses through the prototype chain.

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
  constructor(name, language) {
    super(name);
    this.language = language;
  }
}

const developer = new Developer("Muhammad", "JavaScript");

console.log(developer.name);

developer.name = "Alex";

console.log(developer.name);
```

Output:

```text
Muhammad
Alex
```

The `Developer` class inherits the getter and setter from `Person`.

# 🗁 Overriding Inherited Getters and Setters

A subclass can provide its own getter or setter with the same property name.

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
    return `Developer: ${this._name}`;
  }
}

const developer = new Developer("Muhammad");

console.log(developer.name);
```

Output:

```text
Developer: Muhammad
```

The subclass getter overrides the inherited getter.

If a subclass defines only a getter for a property, it does not automatically retain the inherited setter. The subclass's accessor definition takes precedence for that property.

# 🗁 Getters and Setters with Static Members

JavaScript also supports static getters and setters.

They belong to the class rather than individual instances.

```javascript
class Application {
  static _environment = "development";

  static get environment() {
    return this._environment;
  }

  static set environment(value) {
    const allowed = ["development", "production", "test"];

    if (!allowed.includes(value)) {
      throw new RangeError("Invalid environment");
    }

    this._environment = value;
  }
}

console.log(Application.environment);

Application.environment = "production";

console.log(Application.environment);
```

Output:

```text
development
production
```

Static accessors are useful for controlling class-level configuration.

# 🗁 Practical Example: Temperature Conversion

Getters can expose calculated values while setters update the underlying state.

```javascript
class Temperature {
  constructor(celsius = 0) {
    this.celsius = celsius;
  }

  get celsius() {
    return this._celsius;
  }

  set celsius(value) {
    if (typeof value !== "number" || !Number.isFinite(value)) {
      throw new TypeError("Temperature must be a finite number");
    }

    this._celsius = value;
  }

  get fahrenheit() {
    return (this.celsius * 9) / 5 + 32;
  }

  set fahrenheit(value) {
    if (typeof value !== "number" || !Number.isFinite(value)) {
      throw new TypeError("Temperature must be a finite number");
    }

    this.celsius = ((value - 32) * 5) / 9;
  }
}

const temperature = new Temperature(25);

console.log(temperature.celsius);
console.log(temperature.fahrenheit);

temperature.fahrenheit = 212;

console.log(temperature.celsius);
```

Output:

```text
25
77
100
```

The Celsius value is the underlying state, while Fahrenheit is calculated or converted through accessors.

# 🗁 Practical Example: Shopping Cart Total

A getter can calculate the total price of a shopping cart from its current items.

```javascript
class ShoppingCart {
  constructor() {
    this.items = [];
  }

  addItem(item) {
    this.items.push(item);
  }

  get total() {
    return this.items.reduce(
      (sum, item) => sum + item.price * item.quantity,
      0
    );
  }

  get itemCount() {
    return this.items.reduce(
      (count, item) => count + item.quantity,
      0
    );
  }
}

const cart = new ShoppingCart();

cart.addItem({
  name: "Keyboard",
  price: 80,
  quantity: 1
});

cart.addItem({
  name: "Mouse",
  price: 30,
  quantity: 2
});

console.log(cart.total);
console.log(cart.itemCount);
```

Output:

```text
140
3
```

The `total` and `itemCount` properties are calculated dynamically from the current cart contents.

# 🗁 Common Mistakes

**Calling getters like ordinary methods**

Use `object.property`, not `object.property()`.

**Assigning values to getter-only properties**

A getter without a setter cannot be assigned a new value in strict mode.

**Creating recursive getters**

Returning the same property from its getter causes infinite recursion.

**Creating recursive setters**

Assigning to the same property inside its setter repeatedly invokes the setter.

**Assuming underscore properties are private**

A property such as `_name` is still publicly accessible. Use private fields when actual privacy is required.

**Using getters for expensive operations**

A getter looks like a simple property access. Expensive calculations or operations with side effects can make code difficult to understand.

**Using setters for unrelated business operations**

Setters should generally represent property assignment. Complex actions are often clearer as explicitly named methods.

# 🗁 Best Practices

* Use getters for computed or derived properties.
* Use setters when assignments require validation or normalization.
* Keep getters predictable and free of side effects.
* Avoid expensive operations inside getters.
* Use separate backing properties or private fields to prevent recursion.
* Prefer private fields when internal state requires genuine encapsulation.
* Keep setters focused on updating a single logical property.
* Use ordinary methods for complex operations rather than hiding them behind property assignment.
* Make accessor behavior consistent and easy to understand.
* Avoid storing values that can be reliably calculated from existing state.

# 🗁 Key Takeaways

* Getters execute when a property is read.
* Setters execute when a value is assigned to a property.
* The `get` keyword defines a getter.
* The `set` keyword defines a setter.
* Accessors are used like properties rather than ordinary method calls.
* Getters are useful for computed properties.
* Setters are useful for validation and controlled updates.
* Backing properties or private fields prevent recursive accessor calls.
* Getters and setters can be defined in classes, object literals, and property descriptors.
* Accessors can be inherited, overridden, and declared as static members.
* Well-designed accessors improve encapsulation and make object APIs easier to use.
