# JavaScript Prototypes

A prototype is an object from which another object can inherit properties and methods.

JavaScript uses prototype-based inheritance to allow objects to share functionality without duplicating the same methods in every object.

Every ordinary JavaScript object has an internal link to another object called its prototype. This link is commonly referred to as the object's `[[Prototype]]` internal property.

When JavaScript cannot find a property directly on an object, it looks for that property on the object's prototype.

Example:

```javascript
const user = {
  name: "Muhammad"
};

console.log(Object.getPrototypeOf(user));
```

The result is the prototype object associated with `user`, which is normally `Object.prototype`.

# 🗁 Understanding Prototype-Based Inheritance

Consider two objects that need the same method.

Without prototypes, each object may contain its own copy of the method.

```javascript
const user1 = {
  name: "Muhammad",

  introduce() {
    console.log(`Hello, I am ${this.name}`);
  }
};

const user2 = {
  name: "Alex",

  introduce() {
    console.log(`Hello, I am ${this.name}`);
  }
};

user1.introduce();
user2.introduce();
```

This duplicates the method definition.

With prototypes, both objects can share one method.

```javascript
const userMethods = {
  introduce() {
    console.log(`Hello, I am ${this.name}`);
  }
};

const user1 = {
  name: "Muhammad"
};

const user2 = {
  name: "Alex"
};

Object.setPrototypeOf(user1, userMethods);
Object.setPrototypeOf(user2, userMethods);

user1.introduce();
user2.introduce();
```

Output:

```text
Hello, I am Muhammad
Hello, I am Alex
```

Both objects inherit `introduce()` from `userMethods`.

The method is shared, but `this` refers to the object that invokes it.

# 🗁 The `[[Prototype]]` Internal Property

JavaScript objects have an internal property called `[[Prototype]]`.

It stores a reference to another object or `null`.

You cannot directly access this internal property using ordinary property syntax.

JavaScript provides methods for inspecting and modifying the prototype.

**Get an object's prototype**

```javascript
const user = {
  name: "Muhammad"
};

const prototype = Object.getPrototypeOf(user);

console.log(prototype === Object.prototype);
```

Output:

```text
true
```

**Set an object's prototype**

```javascript
const userMethods = {
  introduce() {
    console.log(`Hello, I am ${this.name}`);
  }
};

const user = {
  name: "Muhammad"
};

Object.setPrototypeOf(user, userMethods);

user.introduce();
```

Output:

```text
Hello, I am Muhammad
```

`Object.setPrototypeOf()` changes the prototype of an existing object. Although useful for understanding prototypes and certain specialized cases, it is generally better to establish the prototype when creating an object.

# 🗁 The `Object.prototype` Object

`Object.prototype` is the root prototype of most ordinary JavaScript object inheritance chains.

It provides commonly inherited methods such as:

* `toString()`
* `valueOf()`
* `hasOwnProperty()`
* `isPrototypeOf()`
* `propertyIsEnumerable()`

Example:

```javascript
const user = {
  name: "Muhammad"
};

console.log(user.toString());
console.log(user.hasOwnProperty("name"));
console.log(user.hasOwnProperty("age"));
```

Output:

```text
[object Object]
true
false
```

The `user` object does not define these methods directly. It inherits them through its prototype chain.

Objects created with `Object.create(null)` are an exception because they have no prototype.

# 🗁 The `prototype` Property of Constructor Functions

Regular JavaScript constructor functions have a public `prototype` property.

This property is used when creating objects with the `new` operator.

```javascript
function User(name) {
  this.name = name;
}

console.log(User.prototype);
```

The `User.prototype` object becomes the prototype of instances created with `new User()`.

Example:

```javascript
function User(name) {
  this.name = name;
}

User.prototype.introduce = function () {
  console.log(`Hello, I am ${this.name}`);
};

const user1 = new User("Muhammad");
const user2 = new User("Alex");

user1.introduce();
user2.introduce();
```

Output:

```text
Hello, I am Muhammad
Hello, I am Alex
```

Both instances share the same `introduce()` method through their prototype.

# 🗁 Difference Between `prototype` and `[[Prototype]]`

These two concepts are related but are not the same.

| `prototype` | `[[Prototype]]` |
| --- | --- |
| A regular property of constructor functions | An internal property of objects |
| Used by `new` when creating instances | References the object's prototype |
| Accessible as `Constructor.prototype` | Inspected using `Object.getPrototypeOf()` |
| Usually contains shared instance methods | Determines property inheritance |

Example:

```javascript
function User(name) {
  this.name = name;
}

const user = new User("Muhammad");

console.log(
  Object.getPrototypeOf(user) === User.prototype
);
```

Output:

```text
true
```

The constructor's `prototype` property supplies the prototype reference used by the new instance.

# 🗁 Constructor Functions and Shared Methods

Constructor functions are a common way to create multiple objects that share behavior.

```javascript
function Product(name, price) {
  this.name = name;
  this.price = price;
}

Product.prototype.getDetails = function () {
  return `${this.name}: $${this.price}`;
};

const product1 = new Product("Laptop", 1000);
const product2 = new Product("Phone", 700);

console.log(product1.getDetails());
console.log(product2.getDetails());
```

Output:

```text
Laptop: $1000
Phone: $700
```

Both instances share one `getDetails()` function.

You can verify this:

```javascript
console.log(
  product1.getDetails === product2.getDetails
);
```

Output:

```text
true
```

This avoids creating a separate method function for every instance.

# 🗁 Adding Properties to a Prototype

You can add properties and methods to a prototype after creating the constructor function.

```javascript
function User(name) {
  this.name = name;
}

User.prototype.role = "Member";

User.prototype.introduce = function () {
  console.log(
    `${this.name} is a ${this.role}`
  );
};

const user = new User("Muhammad");

user.introduce();
```

Output:

```text
Muhammad is a Member
```

The instance inherits both `role` and `introduce()`.

# 🗁 Instance Properties vs Prototype Properties

Instance properties belong directly to individual objects.

Prototype properties are shared through inheritance.

```javascript
function User(name) {
  this.name = name;
}

User.prototype.role = "Member";

const user1 = new User("Muhammad");
const user2 = new User("Alex");

console.log(user1.name);
console.log(user2.name);

console.log(user1.role);
console.log(user2.role);
```

Output:

```text
Muhammad
Alex
Member
Member
```

The `name` property belongs to each instance, while `role` is inherited.

You can verify this using `Object.hasOwn()`.

```javascript
console.log(Object.hasOwn(user1, "name"));
console.log(Object.hasOwn(user1, "role"));
```

Output:

```text
true
false
```

# 🗁 Overriding Prototype Properties

An object can define its own property with the same name as an inherited property.

The object's own property takes precedence.

This is called property shadowing.

```javascript
function User(name) {
  this.name = name;
}

User.prototype.role = "Member";

const user = new User("Muhammad");

console.log(user.role);

user.role = "Administrator";

console.log(user.role);
console.log(User.prototype.role);
```

Output:

```text
Member
Administrator
Member
```

The assignment creates an own `role` property on `user`.

It does not modify the shared prototype property.

# 🗁 Shared Mutable Prototype Properties

Be careful when placing mutable objects such as arrays or nested objects on a prototype.

All instances inherit the same reference.

```javascript
function User(name) {
  this.name = name;
}

User.prototype.permissions = [];

const user1 = new User("Muhammad");
const user2 = new User("Alex");

user1.permissions.push("read");

console.log(user2.permissions);
```

Output:

```javascript
["read"]
```

Both instances access the same inherited array.

For independent data, initialize mutable properties inside the constructor.

```javascript
function User(name) {
  this.name = name;
  this.permissions = [];
}

const user1 = new User("Muhammad");
const user2 = new User("Alex");

user1.permissions.push("read");

console.log(user1.permissions);
console.log(user2.permissions);
```

Output:

```javascript
["read"]
[]
```

Each instance now has its own array.

# 🗁 Prototype Methods and `this`

A method inherited from a prototype still uses the calling object as `this`.

```javascript
function User(name) {
  this.name = name;
}

User.prototype.getName = function () {
  return this.name;
};

const user1 = new User("Muhammad");
const user2 = new User("Alex");

console.log(user1.getName());
console.log(user2.getName());
```

Output:

```text
Muhammad
Alex
```

The method is shared, but the receiver determines the value of `this`.

# 🗁 Prototype of Built-in Objects

JavaScript built-in constructors also use prototypes.

For example, arrays inherit methods from `Array.prototype`.

```javascript
const numbers = [10, 20, 30];

console.log(
  Object.getPrototypeOf(numbers) === Array.prototype
);
```

Output:

```text
true
```

This is why arrays have methods such as:

* `map()`
* `filter()`
* `reduce()`
* `find()`
* `forEach()`
* `push()`
* `pop()`

Example:

```javascript
const numbers = [1, 2, 3];

const doubled = numbers.map(number => number * 2);

console.log(doubled);
```

Output:

```javascript
[2, 4, 6]
```

The `map()` method is inherited from `Array.prototype`, rather than being copied into every array instance.

Strings and functions also use prototype-based inheritance.

```javascript
console.log(
  Object.getPrototypeOf("hello") === String.prototype
);

console.log(
  Object.getPrototypeOf(function () {}) === Function.prototype
);
```

Both expressions return `true` because JavaScript temporarily wraps primitive strings when accessing their methods, and functions are objects.

# 🗁 Checking Prototype Relationships

JavaScript provides several methods for inspecting prototype relationships.

**Object.getPrototypeOf()**

Returns the prototype of an object.

```javascript
const user = {};

console.log(
  Object.getPrototypeOf(user) === Object.prototype
);
```

**Object.prototype.isPrototypeOf()**

Checks whether an object exists in another object's prototype chain.

```javascript
const parent = {
  greet() {
    console.log("Hello");
  }
};

const child = Object.create(parent);

console.log(parent.isPrototypeOf(child));
```

Output:

```text
true
```

**instanceof**

Checks whether a constructor's `prototype` property appears in an object's prototype chain.

```javascript
function User(name) {
  this.name = name;
}

const user = new User("Muhammad");

console.log(user instanceof User);
console.log(user instanceof Object);
```

Output:

```text
true
true
```

# 🗁 Prototype-Based Inheritance with `Object.create()`

`Object.create()` creates a new object using a specified prototype.

```javascript
const userMethods = {
  introduce() {
    console.log(`Hello, I am ${this.name}`);
  }
};

const user = Object.create(userMethods);

user.name = "Muhammad";

user.introduce();
```

Output:

```text
Hello, I am Muhammad
```

The newly created object inherits methods from `userMethods`.

This approach establishes the prototype during object creation instead of changing it afterward.

# 🗁 Prototypes and Classes

JavaScript classes provide a more familiar syntax for creating constructor functions and prototype-based inheritance.

```javascript
class User {
  constructor(name) {
    this.name = name;
  }

  introduce() {
    console.log(`Hello, I am ${this.name}`);
  }
}

const user = new User("Muhammad");

user.introduce();
```

The `introduce()` method is stored on `User.prototype`, not copied into every instance.

```javascript
console.log(
  Object.hasOwn(user, "introduce")
);

console.log(
  Object.hasOwn(User.prototype, "introduce")
);
```

Output:

```text
false
true
```

Classes do not replace the prototype system. They provide syntax built on top of JavaScript's existing prototype-based object model.

# 🗁 Best Practices

* Use prototypes to share methods across multiple instances.
* Keep instance-specific data inside constructors or class constructors.
* Avoid placing mutable arrays and objects on shared prototypes unless shared state is intentional.
* Prefer `Object.create()` or class syntax when establishing prototype relationships.
* Avoid modifying built-in prototypes such as `Object.prototype` or `Array.prototype` in application code.
* Use `Object.getPrototypeOf()` instead of the legacy `__proto__` accessor.
* Use `Object.hasOwn()` to distinguish own properties from inherited properties.
* Understand the difference between a constructor's `prototype` property and an object's internal `[[Prototype]]`.
* Remember that inherited methods use the calling object as `this`.

# 🗁 Key Takeaways

* A prototype is an object from which another object can inherit properties and methods.
* JavaScript uses prototype-based inheritance.
* Every ordinary object has an internal `[[Prototype]]` reference, which may be `null`.
* Constructor functions expose a `prototype` property used by the `new` operator.
* `Object.prototype` is the root prototype of most ordinary objects.
* Prototype methods are shared among instances.
* Instance properties take precedence over inherited properties with the same name.
* Mutable prototype properties are shared between instances.
* `Object.create()` creates objects with a specified prototype.
* JavaScript classes use the same underlying prototype mechanism.
