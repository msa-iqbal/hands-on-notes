# JavaScript Object

An **Object** is a non-primitive data type in JavaScript that stores collections of related data and functionality as key-value pairs.

Objects are fundamental to JavaScript programming. They are used to represent real-world entities, organize application data, define reusable structures, and implement complex application behavior.

```javascript
const user = {
    name: "John",
    age: 25,
    email: "john@example.com",
    isActive: true
};

console.log(user);
```

In this example:

* `user` is an object.
* `name`, `age`, `email`, and `isActive` are property keys.
* `"John"`, `25`, `"john@example.com"`, and `true` are property values.

# 🗁 Creating Objects

JavaScript provides several ways to create objects.

**Object literal**

The object literal syntax is the most common and straightforward way to create an object.

```javascript
const user = {
    name: "John",
    age: 25,
    isActive: true
};

console.log(user.name); // "John"
```

**Object constructor**

You can create an object using the built-in `Object` constructor.

```javascript
const user = new Object();

user.name = "John";
user.age = 25;

console.log(user);
```

Object literals are generally preferred because they are shorter and easier to read.

**Object.create()**

`Object.create()` creates a new object using a specified prototype.

```javascript
const userPrototype = {
    greet() {
        return "Hello!";
    }
};

const user = Object.create(userPrototype);

user.name = "John";

console.log(user.name);    // "John"
console.log(user.greet()); // "Hello!"
```

**Constructor functions**

Constructor functions can create multiple objects with a shared structure and prototype methods.

```javascript
function User(name, age) {
    this.name = name;
    this.age = age;
}

const user1 = new User("John", 25);
const user2 = new User("Sarah", 30);

console.log(user1);
console.log(user2);
```

**Class syntax**

Classes provide a structured syntax for creating objects through constructors.

```javascript
class User {
    constructor(name, age) {
        this.name = name;
        this.age = age;
    }
}

const user = new User("John", 25);

console.log(user);
```

# 🗁 Object Properties

Properties are key-value pairs stored inside an object.

```javascript
const product = {
    id: 101,
    name: "Laptop",
    price: 1200,
    inStock: true
};
```

An object property can contain different JavaScript data types.

```javascript
const data = {
    name: "John",
    age: 25,
    isActive: true,
    address: null,
    hobbies: ["reading", "coding"],
    profile: {
        city: "Dhaka",
        country: "Bangladesh"
    }
};
```

Property keys are strings or symbols. When other primitive values are used as keys in object literals or property access, they are converted to strings.

```javascript
const user = {
    1: "John",
    true: "Active"
};

console.log(user["1"]);     // "John"
console.log(user["true"]);  // "Active"
```

# 🗁 Accessing Object Properties

JavaScript provides two primary ways to access object properties.

**Dot notation**

Use dot notation when the property name is a valid identifier.

```javascript
const user = {
    name: "John",
    age: 25
};

console.log(user.name); // "John"
console.log(user.age);  // 25
```

**Bracket notation**

Use bracket notation when property names contain spaces, special characters, start with numbers, or are stored in variables.

```javascript
const user = {
    name: "John",
    "email address": "john@example.com",
    "user-id": 101
};

console.log(user["name"]);          // "John"
console.log(user["email address"]); // "john@example.com"
console.log(user["user-id"]);       // 101
```

**Dynamic property access**

Bracket notation supports dynamic property names.

```javascript
const user = {
    name: "John",
    age: 25,
    email: "john@example.com"
};

const property = "email";

console.log(user[property]); // "john@example.com"
```

Using dot notation with a variable does not access the variable's value as a property name.

```javascript
const property = "name";

console.log(user.property); // undefined
console.log(user[property]); // "John"
```

# 🗁 Adding and Updating Properties

You can add new properties or update existing properties using assignment.

```javascript
const user = {
    name: "John"
};

// Add a property
user.age = 25;

// Update a property
user.name = "Sarah";

// Add another property
user["email"] = "sarah@example.com";

console.log(user);
```

Objects declared with `const` can still have their properties modified. The `const` declaration prevents reassignment of the variable, not mutation of the object.

```javascript
const user = {
    name: "John"
};

user.name = "Sarah"; // Allowed

// user = {}; // TypeError: Assignment to constant variable
```

# 🗁 Removing Properties

Use the `delete` operator to remove an object property.

```javascript
const user = {
    name: "John",
    age: 25,
    email: "john@example.com"
};

delete user.email;

console.log(user);
// { name: "John", age: 25 }
```

The `delete` operator returns a Boolean indicating whether the property was successfully deleted.

```javascript
const user = {
    name: "John"
};

console.log(delete user.name); // true
console.log(delete user.age);  // true
```

Deleting a property that does not exist generally returns `true`.

# 🗁 Nested Objects

An object can contain other objects as property values. These are called nested objects.

```javascript
const user = {
    name: "John",
    age: 25,

    address: {
        city: "Dhaka",
        country: "Bangladesh",

        location: {
            latitude: 23.8103,
            longitude: 90.4125
        }
    }
};
```

Access nested properties using chained dot notation or bracket notation.

```javascript
console.log(user.address.city); // "Dhaka"

console.log(user.address.location.latitude);
// 23.8103

console.log(user["address"]["country"]);
// "Bangladesh"
```

Optional chaining can safely access properties when an intermediate object might be missing.

```javascript
console.log(user.profile?.phone); // undefined
```

# 🗁 Object Methods

A method is a function stored as an object property.

```javascript
const user = {
    name: "John",

    greet() {
        return `Hello, ${this.name}!`;
    }
};

console.log(user.greet()); // "Hello, John!"
```

Methods allow objects to combine data and behavior.

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
    }
};

console.log(calculator.add(10, 5));      // 15
console.log(calculator.subtract(10, 5)); // 5
console.log(calculator.multiply(10, 5)); // 50
```

# 🗁 Checking Object Properties

JavaScript provides several ways to determine whether an object contains a property.

**The `in` operator**

Checks whether a property exists in an object or anywhere in its prototype chain.

```javascript
const user = {
    name: "John",
    age: 25
};

console.log("name" in user);  // true
console.log("email" in user); // false
```

**Object.hasOwn()**

Checks whether an object directly owns a specified property, without checking its prototype chain.

```javascript
const user = {
    name: "John"
};

console.log(Object.hasOwn(user, "name"));  // true
console.log(Object.hasOwn(user, "email")); // false
```

**hasOwnProperty()**

An older instance method that checks whether a property belongs directly to an object.

```javascript
const user = {
    name: "John"
};

console.log(user.hasOwnProperty("name"));  // true
console.log(user.hasOwnProperty("email")); // false
```

`Object.hasOwn()` is generally preferred because it also works with objects that do not inherit from `Object.prototype`.

# 🗁 Object.keys(), Object.values(), and Object.entries()

JavaScript provides built-in methods for extracting object properties and values.

**Object.keys()**

Returns an array of an object's own enumerable string-keyed property names.

```javascript
const user = {
    name: "John",
    age: 25,
    email: "john@example.com"
};

console.log(Object.keys(user));
// ["name", "age", "email"]
```

**Object.values()**

Returns an array of an object's own enumerable string-keyed property values.

```javascript
console.log(Object.values(user));
// ["John", 25, "john@example.com"]
```

**Object.entries()**

Returns an array of key-value pairs.

```javascript
console.log(Object.entries(user));

// [
//     ["name", "John"],
//     ["age", 25],
//     ["email", "john@example.com"]
// ]
```

These methods are useful for iteration and data transformation.

```javascript
for (const [key, value] of Object.entries(user)) {
    console.log(`${key}: ${value}`);
}
```

# 🗁 Object Destructuring

Destructuring extracts properties from an object into variables.

```javascript
const user = {
    name: "John",
    age: 25,
    email: "john@example.com"
};

const { name, age } = user;

console.log(name); // "John"
console.log(age);  // 25
```

You can rename extracted properties.

```javascript
const { name: username } = user;

console.log(username); // "John"
```

You can also provide default values.

```javascript
const { role = "user" } = user;

console.log(role); // "user"
```

# 🗁 Object Spread Syntax

The spread syntax (`...`) copies an object's own enumerable properties into another object.

```javascript
const user = {
    name: "John",
    age: 25
};

const updatedUser = {
    ...user,
    email: "john@example.com"
};

console.log(updatedUser);
```

Later properties override earlier properties with the same key.

```javascript
const defaults = {
    theme: "light",
    language: "en"
};

const settings = {
    ...defaults,
    theme: "dark"
};

console.log(settings);
// { theme: "dark", language: "en" }
```

Object spread performs a shallow copy. Nested objects are still shared references.

# 🗁 Object.assign()

`Object.assign()` copies enumerable own properties from one or more source objects into a target object.

```javascript
const user = {
    name: "John"
};

const details = {
    age: 25,
    email: "john@example.com"
};

Object.assign(user, details);

console.log(user);
// { name: "John", age: 25, email: "john@example.com" }
```

It can also create a shallow copy by using an empty object as the target.

```javascript
const original = {
    name: "John",
    age: 25
};

const copy = Object.assign({}, original);

console.log(copy);
```

Both `Object.assign()` and object spread perform shallow copies.

# 🗁 Object References

Objects are reference values. Assigning an object to another variable copies the reference, not the object's contents.

```javascript
const user1 = {
    name: "John"
};

const user2 = user1;

user2.name = "Sarah";

console.log(user1.name); // "Sarah"
console.log(user2.name); // "Sarah"
```

Both variables refer to the same object in memory.

You can verify this using strict equality.

```javascript
const first = { value: 10 };
const second = first;

console.log(first === second); // true
```

Two separately created objects with identical properties are not equal by reference.

```javascript
const first = { value: 10 };
const second = { value: 10 };

console.log(first === second); // false
```

# 🗁 Shallow Copy vs Deep Copy

A shallow copy duplicates the top-level properties but retains references to nested objects.

```javascript
const original = {
    name: "John",
    address: {
        city: "Dhaka"
    }
};

const copy = { ...original };

copy.name = "Sarah";
copy.address.city = "Chittagong";

console.log(original.name);         // "John"
console.log(original.address.city); // "Chittagong"
```

The nested address object is shared.

For supported data structures, `structuredClone()` creates a deep copy.

```javascript
const original = {
    name: "John",
    address: {
        city: "Dhaka"
    }
};

const copy = structuredClone(original);

copy.address.city = "Chittagong";

console.log(original.address.city); // "Dhaka"
console.log(copy.address.city);     // "Chittagong"
```

`structuredClone()` supports many built-in data types, including circular references, but does not clone every JavaScript value. For example, functions cannot be structured-cloned.

# 🗁 Object.freeze()

`Object.freeze()` prevents an object from being extended or modified at the property level.

```javascript
const user = {
    name: "John",
    age: 25
};

Object.freeze(user);

user.name = "Sarah"; // No effect in non-strict mode
user.email = "john@example.com"; // No effect

console.log(user);
```

In strict mode, attempts to modify frozen properties can throw a `TypeError`.

`Object.freeze()` is shallow. Nested objects are not automatically frozen.

# 🗁 Object.seal()

`Object.seal()` prevents adding or removing properties, but existing writable properties can still be modified.

```javascript
const user = {
    name: "John",
    age: 25
};

Object.seal(user);

user.name = "Sarah"; // Allowed

user.email = "john@example.com"; // Not added

delete user.age; // Not deleted

console.log(user);
```

# 🗁 Object.preventExtensions()

`Object.preventExtensions()` prevents new properties from being added to an object.

Existing properties can still be updated or deleted, depending on their property descriptors.

```javascript
const user = {
    name: "John",
    age: 25
};

Object.preventExtensions(user);

user.name = "Sarah"; // Allowed

user.email = "john@example.com"; // Not added

delete user.age; // Allowed

console.log(user);
```

# 🗁 Comparing Object.freeze(), Object.seal(), and Object.preventExtensions()

| Feature | `freeze()` | `seal()` | `preventExtensions()` |
| --- | --- | --- | --- |
| Add properties | No | No | No |
| Delete properties | No | No | Yes, if configurable |
| Modify writable properties | No | Yes | Yes |
| Change property descriptors | No | Limited | Limited |
| Nested objects protected | No | No | No |

Use the method that matches the required level of object protection.

# 🗁 Object.is()

`Object.is()` compares two values using a comparison algorithm similar to strict equality, with two important differences:

* `Object.is(NaN, NaN)` returns `true`.
* `Object.is(+0, -0)` returns `false`.

```javascript
console.log(Object.is(10, 10));   // true
console.log(Object.is(10, "10")); // false

console.log(Object.is(NaN, NaN)); // true

console.log(Object.is(+0, -0)); // false
```

For ordinary object comparisons, it checks reference identity.

```javascript
const first = { id: 1 };
const second = { id: 1 };

console.log(Object.is(first, second)); // false
```

# 🗁 Object.entries() and Object.fromEntries()

`Object.fromEntries()` converts an iterable of key-value pairs into an object.

It is commonly used with `Object.entries()` for transforming objects.

```javascript
const user = {
    name: "John",
    age: 25
};

const entries = Object.entries(user);

console.log(entries);
// [["name", "John"], ["age", 25]]

const result = Object.fromEntries(entries);

console.log(result);
// { name: "John", age: 25 }
```

You can transform object values by combining these methods.

```javascript
const prices = {
    laptop: 1000,
    keyboard: 50,
    mouse: 25
};

const discountedPrices = Object.fromEntries(
    Object.entries(prices).map(([key, value]) => [
        key,
        value * 0.9
    ])
);

console.log(discountedPrices);
```

# 🗁 Object Property Descriptors

Every own property has a property descriptor that controls its behavior.

For ordinary data properties, descriptors include:

* `value`: The property's stored value.
* `writable`: Whether the value can be changed.
* `enumerable`: Whether the property appears in common enumeration operations.
* `configurable`: Whether the property can be deleted or its descriptor changed.

```javascript
const user = {
    name: "John"
};

console.log(
    Object.getOwnPropertyDescriptor(user, "name")
);

// {
//     value: "John",
//     writable: true,
//     enumerable: true,
//     configurable: true
// }
```

Property descriptors can be customized using `Object.defineProperty()`.

```javascript
const user = {};

Object.defineProperty(user, "id", {
    value: 101,
    writable: false,
    enumerable: true,
    configurable: false
});

console.log(user.id); // 101
```

# 🗁 Object Prototype

JavaScript objects can inherit properties and methods from other objects through the prototype chain.

```javascript
const user = {
    name: "John"
};

console.log(user.toString());
```

The `toString()` method is not defined directly on `user`. It is inherited through its prototype chain.

You can inspect an object's prototype using `Object.getPrototypeOf()`.

```javascript
console.log(Object.getPrototypeOf(user) === Object.prototype);
// true
```

Prototype inheritance is covered in detail in the JavaScript objects and prototype sections.

# 🗁 Objects and Arrays

Arrays are specialized JavaScript objects designed to store ordered collections of values.

```javascript
const user = {
    name: "John"
};

const users = ["John", "Sarah", "Alex"];

console.log(typeof user);  // "object"
console.log(typeof users); // "object"

console.log(Array.isArray(users)); // true
console.log(Array.isArray(user));  // false
```

Use `Array.isArray()` to distinguish arrays from ordinary objects.

# 🗁 Object Best Practices

* Use object literals for straightforward object creation.
* Use descriptive property names.
* Use dot notation when property names are known and valid identifiers.
* Use bracket notation for dynamic or unusual property names.
* Use `Object.hasOwn()` to check directly owned properties.
* Avoid unnecessary mutation when working with shared application state.
* Remember that assigning an object copies its reference.
* Understand shallow copying before using spread syntax or `Object.assign()`.
* Use `structuredClone()` when a supported deep copy is required.
* Use `Object.freeze()`, `Object.seal()`, or `Object.preventExtensions()` when object protection is necessary.
* Use optional chaining when accessing potentially missing nested properties.
* Avoid assuming that `const` makes an object immutable.

# 🗁 Key Takeaways

* Objects store related data and functionality as key-value pairs.
* Objects are non-primitive reference values.
* Object properties can contain any JavaScript value, including other objects and functions.
* Properties can be accessed using dot notation or bracket notation.
* Objects can be created using literals, constructors, classes, and `Object.create()`.
* Object destructuring extracts properties into variables.
* Spread syntax and `Object.assign()` create shallow copies.
* Object references explain why changes through one variable can affect another.
* `Object.keys()`, `Object.values()`, and `Object.entries()` provide ways to inspect object properties.
* `Object.freeze()`, `Object.seal()`, and `Object.preventExtensions()` control object modification.
* Objects support prototype-based inheritance.
* Arrays are specialized objects with additional collection behavior.
