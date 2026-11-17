# Object Methods

An object method is a function stored as a property of an object.

Methods allow objects to combine related data and behavior in a single structure.

Example:

```javascript
const user = {
  name: "Muhammad",
  age: 25,

  greet() {
    console.log(`Hello, ${this.name}!`);
  }
};

user.greet();
```

Output:

```text
Hello, Muhammad!
```

In this example:

* `user` is an object.
* `name` and `age` are data properties.
* `greet` is a method.
* `this.name` accesses the `name` property of the object.

# 🗁 Creating Object Methods

JavaScript supports several ways to define methods inside objects.

**Method shorthand**

The modern and concise syntax for defining an object method is method shorthand.

```javascript
const user = {
  name: "Muhammad",

  greet() {
    console.log("Hello!");
  }
};

user.greet();
```

**Function expression**

A function expression can also be assigned to an object property.

```javascript
const user = {
  name: "Muhammad",

  greet: function () {
    console.log("Hello!");
  }
};

user.greet();
```

**Arrow function**

An arrow function can be assigned to an object property.

```javascript
const user = {
  name: "Muhammad",

  greet: () => {
    console.log("Hello!");
  }
};

user.greet();
```

However, arrow functions do not have their own `this`. They inherit `this` from the surrounding lexical scope.

For methods that need to access the calling object's properties through `this`, method shorthand or regular functions are generally preferable.

# 🗁 Calling Object Methods

Object methods are called using the object's property name followed by parentheses.

```javascript
const calculator = {
  add(a, b) {
    return a + b;
  }
};

const result = calculator.add(10, 20);

console.log(result);
```

Output:

```text
30
```

Methods can accept parameters and return values just like ordinary functions.

# 🗁 The this Keyword in Object Methods

The `this` keyword refers to the value of the current execution context.

For a method invoked as `object.method()`, `this` normally refers to the object before the dot.

```javascript
const user = {
  name: "Muhammad",
  age: 25,

  getProfile() {
    return `${this.name} is ${this.age} years old.`;
  }
};

console.log(user.getProfile());
```

Output:

```text
Muhammad is 25 years old.
```

The value of `this` depends on how a function is called, not simply where it is defined.

# 🗁 Accessing Multiple Object Properties

Methods can access multiple properties through `this`.

```javascript
const product = {
  name: "Laptop",
  price: 1000,
  quantity: 2,

  getTotal() {
    return this.price * this.quantity;
  },

  getDescription() {
    return `${this.name}: $${this.price}`;
  }
};

console.log(product.getTotal());
console.log(product.getDescription());
```

Output:

```text
2000
Laptop: $1000
```

This keeps related operations inside the object that owns the data.

# 🗁 Modifying Object Properties Through Methods

Methods can modify the object's properties.

```javascript
const user = {
  name: "Muhammad",
  isActive: false,

  activate() {
    this.isActive = true;
  },

  deactivate() {
    this.isActive = false;
  }
};

user.activate();

console.log(user.isActive);

user.deactivate();

console.log(user.isActive);
```

Output:

```text
true
false
```

Methods provide a controlled way to update object state.

# 🗁 Methods with Parameters

Methods can receive arguments to perform operations using external values.

```javascript
const calculator = {
  add(a, b) {
    return a + b;
  },

  subtract(a, b) {
    return a - b;
  },

  multiply(a, b) {
    return a * b;
  },

  divide(a, b) {
    if (b === 0) {
      throw new Error("Cannot divide by zero");
    }

    return a / b;
  }
};

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

# 🗁 Methods Calling Other Methods

An object method can call another method belonging to the same object.

```javascript
const user = {
  firstName: "Muhammad",
  lastName: "Iqbal",

  getFullName() {
    return `${this.firstName} ${this.lastName}`;
  },

  greet() {
    return `Hello, ${this.getFullName()}!`;
  }
};

console.log(user.greet());
```

Output:

```text
Hello, Muhammad Iqbal!
```

Using `this` allows methods to access other methods through the same object.

# 🗁 Dynamic Method Access

Methods can be accessed dynamically using bracket notation.

```javascript
const calculator = {
  add(a, b) {
    return a + b;
  },

  multiply(a, b) {
    return a * b;
  }
};

const operation = "multiply";

console.log(calculator[operation](5, 4));
```

Output:

```text
20
```

This technique is useful when the method name is determined at runtime.

# 🗁 Methods as Callback Functions

Object methods can be passed to other functions as callbacks.

However, passing a method directly can cause it to lose its original `this` context.

```javascript
const user = {
  name: "Muhammad",

  greet() {
    console.log(`Hello, ${this.name}`);
  }
};

const callback = user.greet;

callback();
```

In a strict-mode context, `this` is `undefined` inside `greet()`. Accessing `this.name` therefore throws a `TypeError`.

The original object is not automatically preserved when a method is extracted.

**Using bind()**

The `bind()` method creates a new function with a permanently bound `this` value.

```javascript
const user = {
  name: "Muhammad",

  greet() {
    console.log(`Hello, ${this.name}`);
  }
};

const callback = user.greet.bind(user);

callback();
```

Output:

```text
Hello, Muhammad
```

**Using an arrow function wrapper**

```javascript
const user = {
  name: "Muhammad",

  greet() {
    console.log(`Hello, ${this.name}`);
  }
};

const callback = () => user.greet();

callback();
```

Output:

```text
Hello, Muhammad
```

The wrapper invokes the method through `user`, preserving the correct receiver.

# 🗁 The call(), apply(), and bind() Methods

JavaScript functions provide three important methods for controlling the value of `this`.

**call()**

Invokes a function immediately with a specified `this` value and individual arguments.

```javascript
function greet(greeting) {
  console.log(`${greeting}, ${this.name}!`);
}

const user = {
  name: "Muhammad"
};

greet.call(user, "Hello");
```

Output:

```text
Hello, Muhammad!
```

**apply()**

Works like `call()`, but accepts arguments as an array or array-like object.

```javascript
function introduce(city, country) {
  console.log(
    `${this.name} lives in ${city}, ${country}.`
  );
}

const user = {
  name: "Muhammad"
};

introduce.apply(user, ["Dhaka", "Bangladesh"]);
```

Output:

```text
Muhammad lives in Dhaka, Bangladesh.
```

**bind()**

Returns a new function with a specified `this` value without immediately invoking it.

```javascript
function greet() {
  console.log(`Hello, ${this.name}!`);
}

const user = {
  name: "Muhammad"
};

const boundGreet = greet.bind(user);

boundGreet();
```

Output:

```text
Hello, Muhammad!
```

# 🗁 Method Shorthand vs Arrow Functions

These two method definitions behave differently regarding `this`.

**Method shorthand**

```javascript
const user = {
  name: "Muhammad",

  greet() {
    console.log(this.name);
  }
};

user.greet();
```

Output:

```text
Muhammad
```

The method receives `user` as its `this` value when called through `user.greet()`.

**Arrow function**

```javascript
const user = {
  name: "Muhammad",

  greet: () => {
    console.log(this.name);
  }
};

user.greet();
```

The arrow function does not create its own `this`. It uses the `this` value from the surrounding lexical scope.

In an ES module, top-level `this` is `undefined`, so this example attempts to access `name` on `undefined` and throws a `TypeError`.

Use method shorthand when a method needs its calling object's `this` value.

# 🗁 Built-in Object Methods

JavaScript provides several built-in methods for working with objects.

**Object.keys()**

Returns an array of an object's own enumerable string-keyed property names.

```javascript
const user = {
  name: "Muhammad",
  age: 25
};

console.log(Object.keys(user));
```

Output:

```javascript
["name", "age"]
```

**Object.values()**

Returns an array of an object's own enumerable string-keyed property values.

```javascript
const user = {
  name: "Muhammad",
  age: 25
};

console.log(Object.values(user));
```

Output:

```javascript
["Muhammad", 25]
```

**Object.entries()**

Returns an array of an object's own enumerable string-keyed key-value pairs.

```javascript
const user = {
  name: "Muhammad",
  age: 25
};

console.log(Object.entries(user));
```

Output:

```javascript
[
  ["name", "Muhammad"],
  ["age", 25]
]
```

**Object.fromEntries()**

Creates an object from an iterable of key-value pairs.

```javascript
const entries = [
  ["name", "Muhammad"],
  ["age", 25]
];

const user = Object.fromEntries(entries);

console.log(user);
```

Output:

```javascript
{
  name: "Muhammad",
  age: 25
}
```

**Object.hasOwn()**

Checks whether an object directly contains a specified property.

```javascript
const user = {
  name: "Muhammad"
};

console.log(Object.hasOwn(user, "name"));
console.log(Object.hasOwn(user, "age"));
```

Output:

```text
true
false
```

# 🗁 Practical Example: Shopping Cart

```javascript
const cart = {
  items: [],

  addItem(name, price, quantity = 1) {
    if (price < 0 || quantity <= 0) {
      throw new Error("Invalid item details");
    }

    this.items.push({
      name,
      price,
      quantity
    });
  },

  removeItem(name) {
    this.items = this.items.filter(
      item => item.name !== name
    );
  },

  getTotal() {
    return this.items.reduce(
      (total, item) => total + item.price * item.quantity,
      0
    );
  },

  getItemCount() {
    return this.items.reduce(
      (count, item) => count + item.quantity,
      0
    );
  }
};

cart.addItem("Laptop", 1000, 1);
cart.addItem("Mouse", 25, 2);

console.log(cart.getItemCount());
console.log(cart.getTotal());

cart.removeItem("Mouse");

console.log(cart.getTotal());
```

Output:

```text
3
1050
1000
```

This example demonstrates how methods manage an object's internal data and provide reusable operations.

# 🗁 Best Practices

* Use method shorthand for ordinary object methods.
* Use `this` when accessing the calling object's properties.
* Prefer regular functions or method shorthand when a method needs its own `this`.
* Use arrow functions for callbacks that intentionally inherit lexical `this`.
* Understand `call()`, `apply()`, and `bind()` when working with function context.
* Keep methods focused on a single responsibility.
* Validate arguments before modifying object state.
* Avoid extracting methods without considering their `this` context.
* Use descriptive method names that communicate their behavior.

# 🗁 Key Takeaways

* Object methods are functions stored as object properties.
* Methods combine data and behavior inside objects.
* Method shorthand is a concise way to define object methods.
* The value of `this` depends on how a function is called.
* Arrow functions inherit `this` from their surrounding lexical scope.
* Extracting a method can lose its original receiver.
* `call()`, `apply()`, and `bind()` provide ways to control `this`.
* JavaScript provides built-in object methods for inspecting and transforming objects.
* Methods are fundamental to object-oriented programming in JavaScript.
