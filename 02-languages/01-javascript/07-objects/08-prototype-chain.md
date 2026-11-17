# Prototype Chain

The prototype chain is the sequence of objects that JavaScript follows when searching for a property or method that does not exist directly on an object.

Every object has an internal `[[Prototype]]` reference that points to another object or `null`.

When JavaScript cannot find a requested property on the current object, it searches its prototype. If the property is not found there, it continues searching through the next prototype.

This process continues until the property is found or the chain reaches `null`.

This mechanism is called **prototype chain lookup**.

Example:

```javascript
const person = {
  name: "Muhammad"
};

console.log(person.toString());
```

Output:

```text
[object Object]
```

The `person` object does not directly define `toString()`.

JavaScript finds it on `Object.prototype` through the prototype chain.

# 🗁 How the Prototype Chain Works

Consider the following object:

```javascript
const user = {
  name: "Muhammad"
};
```

Its prototype relationship looks like this:

```text
user
  |
  v
Object.prototype
  |
  v
null
```

The `user` object contains its own `name` property.

When JavaScript searches for `toString()`, it follows this process:

* Checks whether `user` owns `toString()`.
* Checks whether `Object.prototype` owns `toString()`.
* Finds the method and returns it.

Example:

```javascript
const user = {
  name: "Muhammad"
};

console.log(user.hasOwnProperty("name"));
console.log(user.hasOwnProperty("toString"));
```

Output:

```text
true
true
```

The second result is `true` because `hasOwnProperty()` is inherited from `Object.prototype` and is available through the prototype chain.

To distinguish own properties from inherited properties, use `Object.hasOwn()`:

```javascript
console.log(Object.hasOwn(user, "name"));
console.log(Object.hasOwn(user, "toString"));
```

Output:

```text
true
false
```

# 🗁 Prototype Chain Structure

A typical object created using a constructor function has a prototype chain like this:

```text
user
  |
  v
User.prototype
  |
  v
Object.prototype
  |
  v
null
```

Example:

```javascript
function User(name) {
  this.name = name;
}

User.prototype.introduce = function () {
  console.log(`Hello, I am ${this.name}`);
};

const user = new User("Muhammad");

user.introduce();
```

Output:

```text
Hello, I am Muhammad
```

JavaScript searches for `introduce()` on `user` first.

Because the method is not an own property, it searches `User.prototype` and finds it there.

# 🗁 Property Lookup Process

JavaScript performs property lookup through the prototype chain whenever a property is accessed.

Consider:

```javascript
const parent = {
  country: "Bangladesh"
};

const child = Object.create(parent);

child.name = "Muhammad";

console.log(child.name);
console.log(child.country);
```

Output:

```text
Muhammad
Bangladesh
```

For `child.name`:

* JavaScript checks `child`.
* Finds `name` directly.
* Returns `"Muhammad"`.

For `child.country`:

* JavaScript checks `child`.
* Does not find `country`.
* Checks `parent`, which is `child`'s prototype.
* Finds `country`.
* Returns `"Bangladesh"`.

The lookup stops as soon as the property is found.

# 🗁 Multiple Levels of Inheritance

Prototype chains can contain multiple levels.

```javascript
const grandparent = {
  country: "Bangladesh"
};

const parent = Object.create(grandparent);

parent.city = "Dhaka";

const child = Object.create(parent);

child.name = "Muhammad";

console.log(child.name);
console.log(child.city);
console.log(child.country);
```

Output:

```text
Muhammad
Dhaka
Bangladesh
```

The prototype chain is:

```text
child
  |
  v
parent
  |
  v
grandparent
  |
  v
Object.prototype
  |
  v
null
```

The `child` object inherits properties from both `parent` and `grandparent`.

# 🗁 Property Shadowing

Property shadowing occurs when an object defines its own property with the same name as a property inherited from its prototype.

The object's own property takes precedence during property lookup.

```javascript
const parent = {
  role: "User"
};

const child = Object.create(parent);

console.log(child.role);

child.role = "Administrator";

console.log(child.role);
console.log(parent.role);
```

Output:

```text
User
Administrator
User
```

Initially, `child.role` is inherited from `parent`.

After assigning a new value, `child` gets its own `role` property.

The inherited property remains unchanged.

You can verify this:

```javascript
console.log(Object.hasOwn(child, "role"));
console.log(Object.hasOwn(parent, "role"));
```

Output:

```text
true
true
```

Both objects now have their own `role` properties.

# 🗁 Prototype Chain and `Object.getPrototypeOf()`

The `Object.getPrototypeOf()` method returns the immediate prototype of an object.

```javascript
const parent = {
  greet() {
    console.log("Hello");
  }
};

const child = Object.create(parent);

console.log(
  Object.getPrototypeOf(child) === parent
);
```

Output:

```text
true
```

You can use it repeatedly to inspect the complete prototype chain.

```javascript
const parent = {
  name: "Parent"
};

const child = Object.create(parent);

const prototype = Object.getPrototypeOf(child);

console.log(prototype === parent);

console.log(
  Object.getPrototypeOf(prototype) === Object.prototype
);

console.log(
  Object.getPrototypeOf(Object.prototype)
);
```

Output:

```text
true
true
null
```

The last result confirms that `Object.prototype` is the root of this ordinary object's prototype chain.

# 🗁 Checking Prototype Relationships with `isPrototypeOf()`

The `isPrototypeOf()` method checks whether an object exists anywhere in another object's prototype chain.

Syntax:

```javascript
prototypeObject.isPrototypeOf(targetObject);
```

Example:

```javascript
const grandparent = {};
const parent = Object.create(grandparent);
const child = Object.create(parent);

console.log(parent.isPrototypeOf(child));
console.log(grandparent.isPrototypeOf(child));
```

Output:

```text
true
true
```

Both objects are part of the `child` object's prototype chain.

The method returns `false` when the object is not in that chain.

# 🗁 Prototype Chain and `instanceof`

The `instanceof` operator checks whether a constructor's `prototype` property appears anywhere in an object's prototype chain.

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

The prototype chain contains both `User.prototype` and `Object.prototype`.

Example with inheritance:

```javascript
class Person {}

class User extends Person {}

const user = new User();

console.log(user instanceof User);
console.log(user instanceof Person);
console.log(user instanceof Object);
```

Output:

```text
true
true
true
```

The `User` instance inherits through the `Person` prototype and ultimately reaches `Object.prototype`.

# 🗁 Prototype Chain with Constructor Functions

Constructor functions allow objects to share methods through their prototype objects.

```javascript
function Animal(name) {
  this.name = name;
}

Animal.prototype.eat = function () {
  console.log(`${this.name} is eating`);
};

function Dog(name, breed) {
  Animal.call(this, name);
  this.breed = breed;
}

Dog.prototype = Object.create(Animal.prototype);

Dog.prototype.constructor = Dog;

Dog.prototype.bark = function () {
  console.log(`${this.name} is barking`);
};

const dog = new Dog("Buddy", "German Shepherd");

dog.eat();
dog.bark();
```

Output:

```text
Buddy is eating
Buddy is barking
```

The prototype chain is:

```text
dog
  |
  v
Dog.prototype
  |
  v
Animal.prototype
  |
  v
Object.prototype
  |
  v
null
```

The `Dog` instance can access methods defined on both `Dog.prototype` and `Animal.prototype`.

`Animal.call(this, name)` initializes the instance's `name` property. It does not establish prototype inheritance; that is handled by `Object.create(Animal.prototype)`.

# 🗁 Prototype Chain with Classes

JavaScript classes provide cleaner syntax for prototype-based inheritance.

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
  bark() {
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

The instance prototype chain is:

```text
dog
  |
  v
Dog.prototype
  |
  v
Animal.prototype
  |
  v
Object.prototype
  |
  v
null
```

Class inheritance uses the prototype chain to make inherited instance methods available.

Classes also establish a separate inheritance relationship between constructor functions for inherited static members.

# 🗁 Prototype Chain of Arrays

Arrays are objects and also use prototype-based inheritance.

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

The prototype chain of a normal array is:

```text
numbers
  |
  v
Array.prototype
  |
  v
Object.prototype
  |
  v
null
```

This explains why arrays can access methods such as:

* `map()`
* `filter()`
* `reduce()`
* `forEach()`
* `push()`
* `pop()`

For example:

```javascript
const numbers = [1, 2, 3];

const doubled = numbers.map(number => number * 2);

console.log(doubled);
```

The `map()` method is inherited from `Array.prototype`.

# 🗁 Prototype Chain of Functions

Functions are objects in JavaScript.

A normal function's prototype chain typically looks like this:

```text
myFunction
  |
  v
Function.prototype
  |
  v
Object.prototype
  |
  v
null
```

Example:

```javascript
function greet() {
  console.log("Hello");
}

console.log(
  Object.getPrototypeOf(greet) === Function.prototype
);
```

Output:

```text
true
```

Constructor functions also have their own `prototype` property, which is a different concept from their internal `[[Prototype]]` reference.

# 🗁 The End of the Prototype Chain

Every prototype chain eventually reaches `null`.

`null` indicates that there is no further prototype object to inspect.

```javascript
const user = {};

let current = user;

while (current !== null) {
  console.log(current);

  current = Object.getPrototypeOf(current);
}
```

The loop visits:

* The `user` object.
* `Object.prototype`.

Then it reaches `null` and stops.

Objects created without a prototype have `null` as their immediate prototype.

```javascript
const dictionary = Object.create(null);

console.log(
  Object.getPrototypeOf(dictionary)
);
```

Output:

```text
null
```

Such objects do not inherit methods from `Object.prototype`.

# 🗁 Prototype Chain and Performance

Prototype lookup is generally efficient, but excessively deep or frequently modified prototype chains can complicate property access and object optimization.

Consider these practices:

* Keep inheritance hierarchies understandable.
* Avoid unnecessary prototype modifications.
* Prefer composition when inheritance does not represent a genuine relationship.
* Avoid repeatedly changing an object's prototype.
* Share methods through prototypes instead of duplicating them on every instance.
* Avoid modifying built-in prototypes in application code.

JavaScript engines optimize common object access patterns internally, so prototype depth alone does not automatically make an application slow.

# 🗁 Common Mistakes

**Confusing an object's prototype with a constructor's prototype property**

```javascript
function User() {}

const user = new User();

console.log(user.prototype);
console.log(User.prototype);
```

Output:

```text
undefined
```

```javascript
{
  constructor: User
}
```

Ordinary instances do not automatically have a public `prototype` property. The constructor function does.

**Assuming inherited properties are own properties**

```javascript
const parent = {
  role: "Admin"
};

const child = Object.create(parent);

console.log(Object.hasOwn(child, "role"));
```

Output:

```text
false
```

The property is inherited.

**Assuming changing a child property modifies its prototype**

Assigning an ordinary property to a child usually creates or updates an own property rather than changing the inherited property.

**Assuming `instanceof` checks only the immediate prototype**

It searches the entire prototype chain.

**Assuming every object inherits from `Object.prototype`**

Objects created with `Object.create(null)` do not.

**Confusing property lookup with method execution context**

An inherited method is found through the prototype chain, but its `this` value is determined by how it is invoked.

# 🗁 Best Practices

* Use prototype inheritance when objects share behavior through a meaningful relationship.
* Use `Object.getPrototypeOf()` to inspect prototype relationships.
* Use `Object.create()` to establish prototype relationships during object creation.
* Use `Object.hasOwn()` to distinguish own properties from inherited properties.
* Use `instanceof` when constructor-based prototype membership is appropriate.
* Avoid unnecessary prototype mutations.
* Avoid placing instance-specific mutable data on shared prototypes.
* Prefer composition when it produces simpler and more maintainable designs.
* Understand prototype lookup before designing complex inheritance hierarchies.

# 🗁 Key Takeaways

* The prototype chain is JavaScript's mechanism for property and method inheritance.
* Every object has an internal `[[Prototype]]` reference or `null`.
* JavaScript searches an object's own properties before searching its prototype.
* Property lookup continues through successive prototypes until a property is found or the chain reaches `null`.
* Property shadowing occurs when an object defines an own property with the same name as an inherited property.
* `Object.getPrototypeOf()` retrieves an object's immediate prototype.
* `isPrototypeOf()` checks whether an object exists in another object's prototype chain.
* `instanceof` checks whether a constructor's prototype appears in an object's prototype chain.
* Arrays, functions, and class instances all use prototype-based inheritance.
* JavaScript classes provide syntax built on top of the prototype system.
