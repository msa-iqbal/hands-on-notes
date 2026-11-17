# Instance Methods

Instance methods are functions defined inside a JavaScript class that describe the behavior of its instances.

They allow objects created from the same class to share reusable functionality.

Example:

```javascript
class User {
  constructor(name) {
    this.name = name;
  }

  introduce() {
    return `Hello, my name is ${this.name}`;
  }
}

const user = new User("Muhammad");

console.log(user.introduce());
```

Output:

```text
Hello, my name is Muhammad
```

Here:

* `User` is the class.
* `introduce()` is an instance method.
* `user` is an instance of `User`.
* `this.name` accesses the current instance's property.

# 🗁 Defining Instance Methods

Instance methods are defined directly inside the class body.

Syntax:

```javascript
class ClassName {
  methodName(parameters) {
    // Method implementation
  }
}
```

Example:

```javascript
class Calculator {
  add(a, b) {
    return a + b;
  }

  subtract(a, b) {
    return a - b;
  }

  multiply(a, b) {
    return a * b;
  }
}

const calculator = new Calculator();

console.log(calculator.add(10, 5));
console.log(calculator.subtract(10, 5));
console.log(calculator.multiply(10, 5));
```

Output:

```text
15
5
50
```

# 🗁 Calling Instance Methods

Instance methods are called through an object created from the class.

```javascript
class User {
  greet() {
    return "Welcome!";
  }
}

const user = new User();

console.log(user.greet());
```

Output:

```text
Welcome!
```

Calling the method directly through the class name does not work:

```javascript
User.greet();
```

This throws a `TypeError` because `greet()` belongs to the instance's prototype, not the class itself.

# 🗁 The `this` Keyword in Instance Methods

Inside an instance method, `this` normally refers to the object used to invoke the method.

```javascript
class User {
  constructor(name, role) {
    this.name = name;
    this.role = role;
  }

  getProfile() {
    return `${this.name} is a ${this.role}`;
  }
}

const user1 = new User("Muhammad", "Developer");
const user2 = new User("Alex", "Designer");

console.log(user1.getProfile());
console.log(user2.getProfile());
```

Output:

```text
Muhammad is a Developer
Alex is a Designer
```

The same method works with different instances because `this` refers to the current receiver.

# 🗁 Accessing Other Instance Methods

An instance method can call another instance method using `this`.

```javascript
class User {
  constructor(firstName, lastName) {
    this.firstName = firstName;
    this.lastName = lastName;
  }

  getFullName() {
    return `${this.firstName} ${this.lastName}`;
  }

  introduce() {
    return `Hello, I am ${this.getFullName()}`;
  }
}

const user = new User("Muhammad", "Iqbal");

console.log(user.introduce());
```

Output:

```text
Hello, I am Muhammad Iqbal
```

This allows related behavior to be organized into smaller, reusable methods.

# 🗁 Instance Methods Are Stored on the Prototype

Normal methods defined in a class are stored on the class's prototype.

They are not copied into every instance.

```javascript
class User {
  introduce() {
    return "Hello!";
  }
}

const user1 = new User();
const user2 = new User();

console.log(
  Object.hasOwn(user1, "introduce")
);

console.log(
  Object.hasOwn(User.prototype, "introduce")
);

console.log(
  user1.introduce === user2.introduce
);
```

Output:

```text
false
true
true
```

Explanation:

* `introduce` is not an own property of `user1`.
* `introduce` exists on `User.prototype`.
* Both instances reference the same method.

This prototype-based sharing helps avoid creating duplicate method functions for every instance.

# 🗁 Inspecting the Prototype Method

You can access an instance method directly through the class prototype.

```javascript
class User {
  greet() {
    return "Hello!";
  }
}

console.log(
  typeof User.prototype.greet
);
```

Output:

```text
function
```

You can also invoke it with an explicit receiver:

```javascript
const user = {
  name: "Muhammad"
};

console.log(
  User.prototype.greet.call(user)
);
```

This example requires the method to use `this.name` to access the supplied receiver:

```javascript
class User {
  greet() {
    return `Hello, ${this.name}`;
  }
}

const user = {
  name: "Muhammad"
};

console.log(
  User.prototype.greet.call(user)
);
```

Output:

```text
Hello, Muhammad
```

# 🗁 Instance Methods with Parameters

Instance methods can accept parameters just like ordinary functions.

```javascript
class ShoppingCart {
  constructor() {
    this.items = [];
  }

  addItem(item) {
    this.items.push(item);
  }

  removeItem(id) {
    this.items = this.items.filter(
      item => item.id !== id
    );
  }
}

const cart = new ShoppingCart();

cart.addItem({
  id: 1,
  name: "Keyboard"
});

cart.addItem({
  id: 2,
  name: "Mouse"
});

cart.removeItem(1);

console.log(cart.items);
```

Output:

```javascript
[
  {
    id: 2,
    name: "Mouse"
  }
]
```

# 🗁 Instance Methods with Return Values

An instance method can return a value to the caller.

```javascript
class Rectangle {
  constructor(width, height) {
    this.width = width;
    this.height = height;
  }

  getArea() {
    return this.width * this.height;
  }

  getPerimeter() {
    return 2 * (this.width + this.height);
  }
}

const rectangle = new Rectangle(10, 5);

console.log(rectangle.getArea());
console.log(rectangle.getPerimeter());
```

Output:

```text
50
30
```

If a method does not explicitly return a value, it returns `undefined`.

# 🗁 Method Chaining

Method chaining allows multiple method calls to be performed in a single expression.

To support chaining, an instance method can return `this`.

```javascript
class Counter {
  constructor() {
    this.value = 0;
  }

  increment() {
    this.value++;
    return this;
  }

  decrement() {
    this.value--;
    return this;
  }

  reset() {
    this.value = 0;
    return this;
  }
}

const counter = new Counter();

counter
  .increment()
  .increment()
  .decrement();

console.log(counter.value);
```

Output:

```text
1
```

Each method returns the same instance, allowing the next method to be called.

# 🗁 Method Chaining with a Builder Pattern

Method chaining is commonly used in builder-style APIs.

```javascript
class UserBuilder {
  constructor() {
    this.user = {};
  }

  setName(name) {
    this.user.name = name;
    return this;
  }

  setEmail(email) {
    this.user.email = email;
    return this;
  }

  setRole(role) {
    this.user.role = role;
    return this;
  }

  build() {
    return { ...this.user };
  }
}

const user = new UserBuilder()
  .setName("Muhammad")
  .setEmail("muhammad@example.com")
  .setRole("Developer")
  .build();

console.log(user);
```

Output:

```javascript
{
  name: "Muhammad",
  email: "muhammad@example.com",
  role: "Developer"
}
```

The `build()` method returns the completed object instead of the builder instance.

# 🗁 The `this` Binding Problem

A normal instance method gets its `this` value from how it is called.

Consider:

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

const greet = user.introduce;

console.log(greet());
```

In strict mode, which applies automatically to class methods, this throws a `TypeError` because `this` is `undefined` when `greet()` is called as a standalone function.

The method was extracted from the object, so the original receiver was lost.

# 🗁 Fixing `this` with `bind()`

The `bind()` method creates a new function with a permanently specified `this` value.

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

const greet = user.introduce.bind(user);

console.log(greet());
```

Output:

```text
Hello, Muhammad
```

The new function always uses `user` as its `this` value, even when called separately.

# 🗁 Fixing `this` with an Arrow Function Wrapper

You can also use an arrow function to preserve the surrounding `this`.

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

const greet = () => user.introduce();

console.log(greet());
```

Output:

```text
Hello, Muhammad
```

The arrow function closes over the `user` variable.

# 🗁 Instance Methods vs Arrow Function Fields

A class can define an arrow function as an instance field.

```javascript
class User {
  constructor(name) {
    this.name = name;
  }

  introduce = () => {
    return `Hello, ${this.name}`;
  };
}

const user = new User("Muhammad");

const greet = user.introduce;

console.log(greet());
```

Output:

```text
Hello, Muhammad
```

Unlike normal class methods, arrow function fields:

* Are created separately for each instance.
* Are own properties of each instance.
* Capture `this` lexically during instance initialization.
* Are not stored on the class prototype.

Comparison:

```javascript
class User {
  normalMethod() {
    return "Normal method";
  }

  arrowMethod = () => {
    return "Arrow method";
  };
}

const user1 = new User();
const user2 = new User();

console.log(
  user1.normalMethod === user2.normalMethod
);

console.log(
  user1.arrowMethod === user2.arrowMethod
);
```

Output:

```text
true
false
```

Normal methods are shared through the prototype, while each instance receives a separate arrow function.

# 🗁 Overriding Instance Methods

A subclass can define a method with the same name as an inherited method.

This is called method overriding.

```javascript
class Animal {
  speak() {
    return "Animal makes a sound";
  }
}

class Dog extends Animal {
  speak() {
    return "Dog barks";
  }
}

const animal = new Animal();
const dog = new Dog();

console.log(animal.speak());
console.log(dog.speak());
```

Output:

```text
Animal makes a sound
Dog barks
```

The subclass implementation takes precedence when the method is accessed through a `Dog` instance.

# 🗁 Calling a Parent Instance Method with `super`

A subclass can call an overridden parent method using `super`.

```javascript
class Animal {
  speak() {
    return "Animal makes a sound";
  }
}

class Dog extends Animal {
  speak() {
    return `${super.speak()} and dog barks`;
  }
}

const dog = new Dog();

console.log(dog.speak());
```

Output:

```text
Animal makes a sound and dog barks
```

The `super` keyword provides access to methods defined on the parent class's prototype.

# 🗁 Instance Methods and Encapsulation

Instance methods can control how an object's internal state is accessed and modified.

```javascript
class BankAccount {
  constructor(initialBalance) {
    this.balance = initialBalance;
  }

  deposit(amount) {
    if (amount <= 0) {
      throw new RangeError(
        "Deposit must be greater than zero"
      );
    }

    this.balance += amount;
  }

  withdraw(amount) {
    if (amount <= 0 || amount > this.balance) {
      throw new RangeError("Invalid withdrawal");
    }

    this.balance -= amount;
  }

  getBalance() {
    return this.balance;
  }
}

const account = new BankAccount(1000);

account.deposit(500);
account.withdraw(200);

console.log(account.getBalance());
```

Output:

```text
1300
```

The methods provide controlled operations for changing the account balance.

Note that `balance` is still publicly accessible in this example. JavaScript private fields can provide stronger encapsulation and will be covered separately.

# 🗁 Practical Example: Task Management

```javascript
class Task {
  constructor(title) {
    this.title = title;
    this.completed = false;
  }

  complete() {
    this.completed = true;
  }

  reopen() {
    this.completed = false;
  }

  getStatus() {
    return this.completed
      ? "Completed"
      : "Pending";
  }

  getDetails() {
    return {
      title: this.title,
      completed: this.completed,
      status: this.getStatus()
    };
  }
}

const task = new Task("Learn JavaScript classes");

console.log(task.getDetails());

task.complete();

console.log(task.getDetails());
```

Output:

```javascript
{
  title: "Learn JavaScript classes",
  completed: false,
  status: "Pending"
}

{
  title: "Learn JavaScript classes",
  completed: true,
  status: "Completed"
}
```

This example demonstrates how instance methods operate on and manage the state of individual objects.

# 🗁 Common Mistakes

**Calling an instance method directly on the class**

Instance methods are accessed through instances, not directly through the class.

**Losing the `this` binding**

Extracting a normal method and calling it independently can make `this` undefined.

**Creating unnecessary arrow function fields**

Arrow function fields allocate a separate function for each instance. Use normal methods when shared behavior is sufficient.

**Forgetting to return `this` when implementing chaining**

A chainable method must return the instance if the next call should operate on the same object.

**Confusing instance methods with static methods**

Instance methods belong to the prototype and are accessed through instances. Static methods belong to the class itself.

**Modifying instance state without validation**

Methods that update important state should validate incoming values and preserve object invariants.

# 🗁 Best Practices

* Use normal class methods for behavior shared by instances.
* Use descriptive method names that communicate intent.
* Keep each method focused on a specific responsibility.
* Use `this` to access the current instance's state.
* Validate values before modifying important properties.
* Use `bind()` or an appropriate wrapper when passing methods as callbacks.
* Use arrow function fields only when their lexical `this` behavior or per-instance identity is needed.
* Return `this` when implementing an intentional chainable API.
* Prefer encapsulated operations over exposing unnecessary internal implementation details.
* Use inheritance-based overriding only when the subclass genuinely specializes the parent behavior.

# 🗁 Key Takeaways

* Instance methods define behavior for class instances.
* Normal instance methods are stored on the class prototype.
* All instances share the same normal method implementation.
* The `this` keyword normally refers to the object invoking the method.
* Instance methods can access properties and call other methods.
* Methods can accept parameters and return values.
* Returning `this` enables method chaining.
* Extracting a method can lose its original `this` binding.
* `bind()` can preserve a specific `this` value.
* Arrow function fields capture `this` lexically but create separate functions for each instance.
* Subclasses can override inherited instance methods.
* Instance methods are useful for organizing behavior and controlling object state.
