# Object Basics

An object is a non-primitive data type in JavaScript that stores data in the form of **key-value pairs**.

Each key is called a property name, and its associated value can be any JavaScript data type, including:

* Strings
* Numbers
* Booleans
* `null`
* `undefined`
* Arrays
* Other objects
* Functions

Objects are commonly used to represent real-world entities and organize related data and behavior.

Example:

```javascript
const user = {
  name: "Muhammad",
  age: 25,
  profession: "Software Engineer",
  isActive: true
};

console.log(user);
```

Output:

```javascript
{
  name: "Muhammad",
  age: 25,
  profession: "Software Engineer",
  isActive: true
}
```

Here:

* `user` is an object.
* `name`, `age`, `profession`, and `isActive` are property names.
* Their associated values are property values.

# 🗁 Creating Objects

JavaScript provides several ways to create objects.

**Object literal**

The object literal syntax is the most common and straightforward way to create an object.

```javascript
const user = {
  name: "Muhammad",
  age: 25
};
```

**Object constructor**

The `Object` constructor creates an object.

```javascript
const user = new Object();

user.name = "Muhammad";
user.age = 25;

console.log(user);
```

**Object.create()**

The `Object.create()` method creates a new object with a specified prototype.

```javascript
const userPrototype = {
  greet() {
    console.log(`Hello, ${this.name}`);
  }
};

const user = Object.create(userPrototype);

user.name = "Muhammad";

user.greet();
```

Output:

```text
Hello, Muhammad
```

**Constructor functions**

Constructor functions can create multiple objects with a shared structure.

```javascript
function User(name, age) {
  this.name = name;
  this.age = age;
}

const user1 = new User("Muhammad", 25);
const user2 = new User("Alex", 30);

console.log(user1);
console.log(user2);
```

**Classes**

Classes provide a modern syntax for creating objects with shared behavior.

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

# 🗁 Accessing Object Properties

JavaScript provides two common ways to access object properties.

**Dot notation**

Use dot notation when the property name is a valid identifier.

```javascript
const user = {
  name: "Muhammad",
  age: 25
};

console.log(user.name);
console.log(user.age);
```

Output:

```text
Muhammad
25
```

**Bracket notation**

Use bracket notation when accessing properties dynamically or when property names contain special characters.

```javascript
const user = {
  name: "Muhammad",
  "first-name": "Muhammad",
  age: 25
};

console.log(user["name"]);
console.log(user["first-name"]);
```

Output:

```text
Muhammad
Muhammad
```

Bracket notation also allows property names to be determined at runtime.

```javascript
const user = {
  name: "Muhammad",
  age: 25
};

const property = "name";

console.log(user[property]);
```

Output:

```text
Muhammad
```

# 🗁 Adding Object Properties

Properties can be added after an object has been created.

```javascript
const user = {
  name: "Muhammad"
};

user.age = 25;
user.profession = "Software Engineer";

console.log(user);
```

Output:

```javascript
{
  name: "Muhammad",
  age: 25,
  profession: "Software Engineer"
}
```

Bracket notation can also add properties dynamically.

```javascript
const user = {};

const property = "name";

user[property] = "Muhammad";

console.log(user);
```

# 🗁 Modifying Object Properties

Existing property values can be changed by assigning a new value.

```javascript
const user = {
  name: "Muhammad",
  age: 25
};

user.age = 26;

console.log(user.age);
```

Output:

```text
26
```

The object remains the same object; only its property value changes.

# 🗁 Removing Object Properties

The `delete` operator removes a property from an object.

```javascript
const user = {
  name: "Muhammad",
  age: 25,
  profession: "Software Engineer"
};

delete user.age;

console.log(user);
```

Output:

```javascript
{
  name: "Muhammad",
  profession: "Software Engineer"
}
```

The `delete` operator removes the property itself rather than assigning `undefined`.

# 🗁 Object Methods

A function stored as an object property is commonly called a method.

```javascript
const user = {
  name: "Muhammad",

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

Methods allow objects to combine related data and behavior.

# 🗁 Nested Objects

An object can contain other objects as property values.

```javascript
const user = {
  name: "Muhammad",

  address: {
    city: "Dhaka",
    country: "Bangladesh"
  }
};

console.log(user.address.city);
console.log(user.address.country);
```

Output:

```text
Dhaka
Bangladesh
```

Nested objects are useful for representing structured data.

# 🗁 Arrays Inside Objects

Objects can contain arrays as property values.

```javascript
const user = {
  name: "Muhammad",
  skills: ["JavaScript", "TypeScript", "Node.js"]
};

console.log(user.skills[0]);
console.log(user.skills.length);
```

Output:

```text
JavaScript
3
```

# 🗁 Objects Inside Arrays

Arrays can contain objects as elements.

```javascript
const users = [
  {
    id: 1,
    name: "Muhammad"
  },
  {
    id: 2,
    name: "Alex"
  },
  {
    id: 3,
    name: "John"
  }
];

console.log(users[0].name);
console.log(users[1].name);
```

Output:

```text
Muhammad
Alex
```

This structure is common when working with API responses and collections of records.

# 🗁 Checking Whether a Property Exists

The `in` operator checks whether a property exists in an object or anywhere in its prototype chain.

```javascript
const user = {
  name: "Muhammad",
  age: 25
};

console.log("name" in user);
console.log("email" in user);
```

Output:

```text
true
false
```

The `Object.hasOwn()` method checks whether a property belongs directly to the object.

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

Unlike `in`, `Object.hasOwn()` does not consider inherited properties.

# 🗁 Object Reference Behavior

Objects are reference values.

When an object is assigned to another variable, JavaScript copies the reference rather than creating an independent object.

```javascript
const user1 = {
  name: "Muhammad"
};

const user2 = user1;

user2.name = "Alex";

console.log(user1.name);
console.log(user2.name);
```

Output:

```text
Alex
Alex
```

Both variables refer to the same object in memory.

# 🗁 Comparing Objects

Objects are compared by reference, not by their property contents.

```javascript
const user1 = {
  name: "Muhammad"
};

const user2 = {
  name: "Muhammad"
};

console.log(user1 === user2);
```

Output:

```text
false
```

Although both objects contain identical properties and values, they are separate objects.

However:

```javascript
const user1 = {
  name: "Muhammad"
};

const user2 = user1;

console.log(user1 === user2);
```

Output:

```text
true
```

Both variables reference the same object.

# 🗁 Object Mutability

JavaScript objects are mutable by default.

Their properties can be added, modified, and removed even when the object is declared using `const`.

```javascript
const user = {
  name: "Muhammad"
};

user.name = "Alex";
user.age = 25;

console.log(user);
```

Output:

```javascript
{
  name: "Alex",
  age: 25
}
```

However, `const` prevents reassignment of the variable itself.

```javascript
const user = {
  name: "Muhammad"
};

user = {
  name: "Alex"
};
```

This throws a `TypeError` because the `user` binding cannot be reassigned.

# 🗁 Useful Object Methods

JavaScript provides built-in methods for working with objects.

**Object.keys()**

Returns an array containing an object's own enumerable string-keyed property names.

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

Returns an array containing the object's own enumerable string-keyed property values.

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

Returns an array containing the object's own enumerable string-keyed key-value pairs.

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

**Object.assign()**

Copies enumerable own properties from one or more source objects into a target object.

```javascript
const user = {
  name: "Muhammad"
};

const additionalInfo = {
  age: 25
};

Object.assign(user, additionalInfo);

console.log(user);
```

Output:

```javascript
{
  name: "Muhammad",
  age: 25
}
```

`Object.assign()` performs a shallow copy of property values.

# 🗁 Object Destructuring

Destructuring allows properties to be extracted into variables.

```javascript
const user = {
  name: "Muhammad",
  age: 25
};

const { name, age } = user;

console.log(name);
console.log(age);
```

Output:

```text
Muhammad
25
```

Object destructuring is particularly useful when working with function parameters and API responses.

# 🗁 Practical Example: User Management

```javascript
const user = {
  id: 101,
  name: "Muhammad",
  email: "muhammad@example.com",
  isActive: true,

  activate() {
    this.isActive = true;
  },

  deactivate() {
    this.isActive = false;
  },

  getProfile() {
    return {
      id: this.id,
      name: this.name,
      email: this.email
    };
  }
};

console.log(user.getProfile());

user.deactivate();

console.log(user.isActive);
```

Output:

```javascript
{
  id: 101,
  name: "Muhammad",
  email: "muhammad@example.com"
}

false
```

This example demonstrates how objects organize related data and behavior.

# 🗁 Best Practices

* Use object literals for simple object creation.
* Use meaningful property names.
* Prefer dot notation when property names are known and valid identifiers.
* Use bracket notation for dynamic property access.
* Use `const` when an object binding does not need reassignment.
* Avoid unnecessary deep nesting.
* Use `Object.hasOwn()` when checking for an object's own properties.
* Understand reference behavior when assigning and comparing objects.
* Keep object methods focused on related responsibilities.
* Consider immutability patterns when managing application state.

# 🗁 Key Takeaways

* Objects store data as key-value pairs.
* Properties represent data, while methods represent behavior.
* Objects can contain nested objects, arrays, and functions.
* Properties can be accessed using dot notation or bracket notation.
* Objects are mutable by default.
* Assigning an object to another variable copies its reference.
* Objects are compared by reference rather than structural equality.
* JavaScript provides built-in methods such as `Object.keys()`, `Object.values()`, and `Object.entries()`.
* Objects are fundamental to JavaScript application development.
