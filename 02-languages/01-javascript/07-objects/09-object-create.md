# Object.create()

`Object.create()` is a built-in JavaScript method that creates a new object with a specified prototype object.

It allows you to establish prototype-based inheritance directly when creating an object.

Unlike object literals, which normally inherit from `Object.prototype`, `Object.create()` gives you explicit control over the new object's prototype.

Example:

```javascript
const parent = {
  name: "Muhammad",

  introduce() {
    console.log(`Hello, I am ${this.name}`);
  }
};

const child = Object.create(parent);

child.introduce();
```

Output:

```text
Hello, I am Muhammad
```

The `child` object inherits the `name` property and `introduce()` method from `parent`.

# 🗁 Syntax

```javascript
Object.create(proto);
```

With property descriptors:

```javascript
Object.create(proto, propertiesObject);
```

Parameters:

| Parameter | Description |
| --- | --- |
| `proto` | The object that should become the new object's prototype, or `null` |
| `propertiesObject` | Optional object containing property descriptors for the new object's own properties |

Return value:

* A newly created object with the specified prototype.
* Throws a `TypeError` if the first argument is neither an object nor `null`.

# 🗁 Creating an Object with a Prototype

You can use an existing object as the prototype of a new object.

```javascript
const userPrototype = {
  role: "Member",

  introduce() {
    console.log(`My name is ${this.name}`);
  }
};

const user = Object.create(userPrototype);

user.name = "Muhammad";

user.introduce();
console.log(user.role);
```

Output:

```text
My name is Muhammad
Member
```

The `user` object inherits `role` and `introduce()` from `userPrototype`.

Its `name` property belongs directly to the `user` object.

# 🗁 Understanding the Prototype Relationship

`Object.create()` establishes the prototype relationship during object creation.

```javascript
const parent = {
  country: "Bangladesh"
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

The prototype chain looks like this:

```text
child
  |
  v
parent
  |
  v
Object.prototype
  |
  v
null
```

The new object can access properties from every object in its prototype chain.

# 🗁 Creating an Object with No Prototype

You can pass `null` to create an object without a prototype.

```javascript
const dictionary = Object.create(null);

console.log(Object.getPrototypeOf(dictionary));
```

Output:

```text
null
```

This object does not inherit from `Object.prototype`.

For example:

```javascript
const dictionary = Object.create(null);

dictionary.name = "JavaScript";
dictionary.version = "ES2025";

console.log(dictionary.name);
console.log(dictionary.toString);
```

Output:

```text
JavaScript
undefined
```

Unlike ordinary objects, it does not automatically have methods such as:

* `toString()`
* `valueOf()`
* `hasOwnProperty()`
* `isPrototypeOf()`

This can be useful when creating dictionary-like objects that should not inherit unrelated properties.

For checking own properties, use:

```javascript
console.log(Object.hasOwn(dictionary, "name"));
```

Output:

```text
true
```

# 🗁 Adding Own Properties During Object Creation

The second argument of `Object.create()` allows you to define properties using property descriptors.

```javascript
const user = Object.create(
  Object.prototype,
  {
    name: {
      value: "Muhammad",
      writable: true,
      enumerable: true,
      configurable: true
    },

    age: {
      value: 25,
      writable: true,
      enumerable: true,
      configurable: true
    }
  }
);

console.log(user);
```

Output:

```javascript
{
  name: "Muhammad",
  age: 25
}
```

The properties are created directly on the new object.

Their behavior is controlled by the descriptor options.

| Descriptor | Description |
| --- | --- |
| `value` | Defines the property's value |
| `writable` | Determines whether the value can be changed |
| `enumerable` | Determines whether the property appears in enumeration |
| `configurable` | Determines whether the property can be deleted or reconfigured |

# 🗁 Using Default Property Descriptor Values

When a property descriptor omits its Boolean attributes, they default to `false`.

```javascript
const user = Object.create(
  Object.prototype,
  {
    name: {
      value: "Muhammad"
    }
  }
);

console.log(user.name);

console.log(
  Object.getOwnPropertyDescriptor(user, "name")
);
```

Output:

```javascript
Muhammad

{
  value: "Muhammad",
  writable: false,
  enumerable: false,
  configurable: false
}
```

The property exists, but it cannot normally be reassigned, deleted, or included in ordinary enumeration.

For this reason, explicitly define descriptor attributes when you need ordinary mutable properties.

# 🗁 Inheriting Methods with `Object.create()`

One common use case is creating multiple objects that share methods.

```javascript
const userMethods = {
  introduce() {
    console.log(`Hello, I am ${this.name}`);
  },

  getName() {
    return this.name;
  }
};

const user1 = Object.create(userMethods);
const user2 = Object.create(userMethods);

user1.name = "Muhammad";
user2.name = "Alex";

user1.introduce();
user2.introduce();

console.log(user1.getName());
console.log(user2.getName());
```

Output:

```text
Hello, I am Muhammad
Hello, I am Alex
Muhammad
Alex
```

Both objects share the same method implementations.

```javascript
console.log(
  user1.introduce === user2.introduce
);
```

Output:

```text
true
```

The method exists once on `userMethods`, rather than being duplicated on each object.

# 🗁 Creating an Inheritance Chain

You can use `Object.create()` repeatedly to establish multiple inheritance levels through prototypes.

```javascript
const animal = {
  eat() {
    console.log(`${this.name} is eating`);
  }
};

const mammal = Object.create(animal);

mammal.walk = function () {
  console.log(`${this.name} is walking`);
};

const dog = Object.create(mammal);

dog.name = "Buddy";

dog.eat();
dog.walk();
```

Output:

```text
Buddy is eating
Buddy is walking
```

The prototype chain is:

```text
dog
  |
  v
mammal
  |
  v
animal
  |
  v
Object.prototype
  |
  v
null
```

The `dog` object inherits methods from both `mammal` and `animal`.

# 🗁 Creating Objects from Existing Objects

`Object.create()` can create a new object that inherits from an existing object without modifying the original.

```javascript
const user = {
  name: "Muhammad",
  role: "Developer"
};

const admin = Object.create(user);

admin.role = "Administrator";

console.log(admin.name);
console.log(admin.role);

console.log(user.role);
```

Output:

```text
Muhammad
Administrator
Developer
```

The `admin` object inherits `name` from `user` but has its own `role` property.

# 🗁 Property Shadowing

If a new object defines a property with the same name as an inherited property, its own property takes precedence.

```javascript
const parent = {
  role: "User"
};

const child = Object.create(parent);

console.log(child.role);

child.role = "Admin";

console.log(child.role);
console.log(parent.role);
```

Output:

```text
User
Admin
User
```

The assignment creates an own property on `child`.

It does not modify the inherited property on `parent`.

# 🗁 Object.create() vs Object Literals

Both approaches create objects, but their prototype behavior differs.

**Using an object literal**

```javascript
const user = {
  name: "Muhammad"
};
```

An ordinary object literal inherits from `Object.prototype`.

**Using Object.create()**

```javascript
const userPrototype = {
  introduce() {
    console.log(`Hello, I am ${this.name}`);
  }
};

const user = Object.create(userPrototype);

user.name = "Muhammad";
```

The new object inherits from `userPrototype`.

Comparison:

| Object Literal | `Object.create()` |
| --- | --- |
| Uses `{}` syntax | Uses a built-in method |
| Normally inherits from `Object.prototype` | Uses the specified prototype |
| Defines own properties directly in the literal | Can define own properties through descriptors |
| Convenient for ordinary objects | Useful for explicit prototype relationships |
| Cannot directly specify a custom prototype in its basic syntax | Supports custom prototypes and `null` |

# 🗁 Object.create() vs Object.assign()

These methods serve different purposes.

`Object.create()` creates a new object with a specified prototype.

`Object.assign()` copies enumerable own properties from source objects into an existing target object.

**Using Object.create()**

```javascript
const parent = {
  role: "Developer"
};

const child = Object.create(parent);

console.log(child.role);
console.log(Object.hasOwn(child, "role"));
```

Output:

```text
Developer
false
```

The property is inherited.

**Using Object.assign()**

```javascript
const parent = {
  role: "Developer"
};

const child = Object.assign({}, parent);

console.log(child.role);
console.log(Object.hasOwn(child, "role"));
```

Output:

```text
Developer
true
```

The property is copied directly onto the target object.

# 🗁 Object.create() vs Constructor Functions

Both techniques can establish prototype-based inheritance.

**Using Object.create()**

```javascript
const userPrototype = {
  introduce() {
    console.log(`Hello, I am ${this.name}`);
  }
};

const user = Object.create(userPrototype);

user.name = "Muhammad";

user.introduce();
```

**Using a constructor function**

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

The constructor function provides a convenient way to initialize instances with individual data while sharing methods through its prototype.

`Object.create()` is useful when you want to specify the prototype directly without invoking a constructor.

# 🗁 Practical Example: Creating a User Object System

You can use `Object.create()` to build a simple object system with shared behavior.

```javascript
const userPrototype = {
  introduce() {
    console.log(
      `Hello, I am ${this.name}`
    );
  },

  getDetails() {
    return {
      name: this.name,
      role: this.role
    };
  }
};

function createUser(name, role) {
  const user = Object.create(userPrototype);

  user.name = name;
  user.role = role;

  return user;
}

const user1 = createUser(
  "Muhammad",
  "Developer"
);

const user2 = createUser(
  "Alex",
  "Designer"
);

user1.introduce();
user2.introduce();

console.log(user1.getDetails());
console.log(user2.getDetails());
```

Output:

```text
Hello, I am Muhammad
Hello, I am Alex

{
  name: "Muhammad",
  role: "Developer"
}

{
  name: "Alex",
  role: "Designer"
}
```

Both objects share the same methods while maintaining independent properties.

# 🗁 Common Mistakes

**Confusing inheritance with copying**

```javascript
const parent = {
  name: "Muhammad"
};

const child = Object.create(parent);
```

The `child` object does not receive a copied `name` property. It inherits the property through its prototype.

**Forgetting that inherited objects are shared references**

If a prototype contains an array or nested object, all inheriting objects access the same reference unless they define their own property.

**Expecting Object.create(null) to inherit Object methods**

```javascript
const dictionary = Object.create(null);

console.log(dictionary.toString);
```

Output:

```text
undefined
```

There is no prototype chain leading to `Object.prototype`.

**Omitting descriptor attributes**

Properties defined through the second argument default to non-writable, non-enumerable, and non-configurable unless specified otherwise.

**Using Object.create() when a normal object is sufficient**

For ordinary data objects, object literals are usually simpler and easier to read.

**Confusing Object.create() with Object.assign()**

`Object.create()` establishes inheritance, while `Object.assign()` copies property values.

# 🗁 Best Practices

* Use `Object.create()` when you need explicit control over an object's prototype.
* Use `Object.create(null)` for dictionary-like objects that should not inherit from `Object.prototype`.
* Use property descriptors deliberately when defining properties during creation.
* Use shared prototype methods to avoid duplicating behavior.
* Keep instance-specific data on the instance rather than on shared prototypes.
* Prefer object literals for simple data structures.
* Prefer class syntax or constructor functions when you need a reusable instance initialization pattern.
* Avoid unnecessarily deep prototype chains.
* Use `Object.hasOwn()` to check whether a property belongs directly to an object.

# 🗁 Key Takeaways

* `Object.create()` creates a new object with a specified prototype.
* Its first argument can be an object or `null`.
* Its optional second argument defines own properties using property descriptors.
* Objects created with `Object.create(null)` have no prototype.
* Objects can inherit methods and properties without copying them.
* Multiple inheritance levels can be established through successive prototype relationships.
* Own properties take precedence over inherited properties.
* `Object.create()` establishes inheritance, whereas `Object.assign()` copies properties.
* It is useful for understanding and implementing JavaScript's prototype-based inheritance system.
